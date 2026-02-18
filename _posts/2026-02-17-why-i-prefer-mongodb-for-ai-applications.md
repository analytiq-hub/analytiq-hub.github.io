---
layout: post
title: "Why I Prefer MongoDB For AI Applications"
date: 2026-02-17 00:00:00 +0000
author: "Andrei Radulescu-Banu"
image: /assets/images/mongodb-ai-applications-splash.png
categories: [tech, programming, ai, databases]
---

I use MongoDB as the primary database for AI-powered products like [DocRouter.AI](https://docrouter.ai) and [SigAgent.AI](https://sigagent.ai). This post explains how it’s implemented—migrations, indices, vector search, knowledge bases—and why I prefer it over alternatives like Postgres for these workloads.

## DocRouter and SigAgent: One Backend, Two Products

**DocRouter.AI** is a smart document router: you upload documents, define schemas and prompts, and it extracts structured data (e.g. from invoices, medical records, forms) using LLMs. **SigAgent** is a Claude agent monitor with a different UX and product focus, but it’s built on the same stack.

Roughly **90% of the backend is shared**. Both use the same Python package, `analytiq_data`: MongoDB client, migrations, queue layer, auth, and app startup. The same MongoDB database layout, indices, and migration history apply to both. Product-specific code lives in routes and frontends; the data layer is common.

## Why MongoDB Scales Better Horizontally Than Postgres

For AI apps that need to grow read/write and storage without a single bottleneck, MongoDB is a better fit than Postgres in several ways:

- **Sharding is built-in.** You add shards and MongoDB distributes data by shard key. No need for application-level partitioning or external tools to get horizontal scaling.
- **Replica sets give read scaling and HA.** Adding secondaries increases read capacity and failover; `readPreference=secondaryPreferred` (which we use) sends reads to secondaries when possible.
- **Schema flexibility reduces migration pain at scale.** Evolving document shape over time doesn’t require locking the whole table or long-running `ALTER TABLE` on huge datasets. We still treat schema seriously (see below), but we upgrade via application-level migrations instead of DDL.
- **Operational simplicity.** Managed offerings (Atlas, DocumentDB) handle replication, backups, and scaling. For self-hosted, the same concepts apply: replica set + optional sharding.

Postgres can scale with Citus, read replicas, and partitioning, but getting to the same “add nodes and rebalance” story is more involved. For document-centric, JSON-heavy AI workloads, MongoDB’s model aligns well with how we store prompts, runs, and vectors.

## Strict Schema via Discipline: Migrations as “Schema”

MongoDB doesn’t enforce a schema. I still want **predictable structure and safe upgrades**, so we enforce it in code and process:

- **Consistent document shape per collection.** We use a fixed set of field names and types in application code (and in TypeScript/Pydantic where applicable). In practice, each collection behaves like a table with a known “schema.”
- **Every change goes through migrations.** Renaming fields, adding/removing fields, splitting or renaming collections—all of it is done in versioned migration classes with `up()` and `down()`. The current schema version is stored in a `migrations` collection; on startup we run `run_migrations(analytiq_client)` and bring the DB to the latest version.
- **Element types as schema.** We treat document fields as if they were typed: e.g. `schema_id`, `prompt_version`, `organization_id` are always present where we expect them. New code assumes the post-migration shape. So we get **the same kind of safety as Postgres** (known structure, no surprise shapes) while staying in a document model—as long as we’re disciplined and never bypass migrations.

So: schema is “enforced” by convention and migrations, not by the database. That keeps development fast and “vibe coded” without giving up control.

## How Migrations and Indices Are Implemented

### Migrations

Migrations live in [analytiq_data/migrations/migration.py](https://github.com/analytiq-hub/doc-router/blob/main/packages/python/analytiq_data/migrations/migration.py). Each migration is a class with:

- `description`: short human-readable summary
- `up(db)`: apply the change (e.g. rename field, backfill, add index)
- `down(db)`: revert the change when possible

The runner loads the list `MIGRATIONS`, assigns a version by index, and stores the current version in `db.migrations` under `_id: "schema_version"`. On startup we run pending migrations in order. Examples from the codebase:

- **Field renames:** e.g. `RenameUserFields` (camelCase → snake_case), `RenamePromptRevIdToPromptRevid`
- **New fields:** e.g. `LlmResultFieldsMigration` (adds `updated_llm_result`, `is_edited`, `is_verified`, timestamps)
- **Structural changes:** e.g. `RenameCollections` (schemas → schema_revisions, schema_versions → schemas, etc.), `UseMongoObjectIDs`
- **Indexes:** e.g. `AddQueueAndCollectionIndexes` (queue polling, document_index, docs, llm_runs, knowledge_bases, prompt_revisions, schema_revisions), `AddWebhookDeliveriesIndexes`, `AddAccessTokenUniquenessIndex`

So collection layout and index strategy evolve in one place, with a clear history and rollback path.

### Indices

We manage indices in two ways:

1. **Inside migrations** for schema-related or long-term indices (e.g. queue `status_created_at_idx`, `docs` org + upload_date, `llm_runs` by document and prompt, KB and revision indexes). These are created with `create_index(..., background=True)` and, in the migration’s `down()`, dropped so rollback is consistent.
2. **At runtime where needed** via `ensure_index()` in `analytiq_data/mongodb/index.py`: given a collection, index spec, and name, it creates the index if missing (and optionally drops other non-`_id` indexes). We use this for things like `payments_usage_records (org_id, timestamp)` when the payments module initializes.

Example from a migration:

- **Queue collections:** `(status, created_at)` for `find_one_and_update({ status: "pending" }, sort: { created_at: 1 })`.
- **docs:** `(organization_id, upload_date desc)` for paginated listing by org.
- **llm_runs:** `(document_id, prompt_id, prompt_version desc)` for “latest run by document and prompt,” and `(document_id, prompt_revid)` for exact revision lookup.
- **document_index:** unique `(kb_id, document_id)` and a non-unique `document_id` index for cascade deletes.

Optimizing queries follows the same idea: identify hot queries (from Atlas Query Insights or the profiler), then add or adjust an index and, when it’s a lasting change, encode it in a migration.

## Local Mongo for Dev, Atlas (or DocumentDB) for Production

- **Development:** `MONGODB_URI` defaults to `mongodb://localhost:27017`. We run a local MongoDB (or the Atlas Local Docker image for vector search—see below). The database name is driven by `ENV` (e.g. `dev`).
- **Production:** We use **MongoDB Atlas** for our own products. For consulting or on-prem, we use **AWS DocumentDB** when the client standardizes on AWS. The client is configured with `w='majority'`, `readPreference='secondaryPreferred'`, and `retryWrites=False` (DocumentDB doesn’t support retryWrites). Same code, different URI and env.

So: one codebase, local Mongo for dev, Atlas or DocumentDB for prod.

## Finding Heavy Queries: Atlas vs Open-Source Mongo

### Atlas: Query Insights

In **MongoDB Atlas**, we use **Query Insights** (Performance Advisor / slow-query tools) to see which operations are slow or return a lot of data. A practical workflow:

1. Open the cluster → **Metrics** / **Query Insights** (or Performance Advisor).
2. Filter or sort by operations that return a large number of documents (e.g. over 1,000).
3. Take the reported query shape and add a compound index that matches the filter and sort (e.g. `organization_id` + `upload_date` for doc listing). The index is easily vibe coded with Claude or another AI editor.
4. Add that index in code (migration or `ensure_index`) and deploy.

That way we avoid full collection scans and large result sets in hot paths.

### Open-Source MongoDB: Profiler and `nReturned`

With **open-source MongoDB** (Community Edition), there’s no built-in “queries returning more than N documents” view like Atlas’s UI. You can still get there with the **database profiler**:

1. **Enable profiling** (e.g. level 1 for slow ops, or level 2 for all):
   ```javascript
   db.setProfilingLevel(1, { slowms: 100 })   // slow only
   // or
   db.setProfilingLevel(2)                     // all operations
   ```
2. **Query `system.profile`** and filter by `nReturned` (number of documents returned):
   ```javascript
   db.system.profile.find({ nReturned: { $gt: 1000 } }).sort({ ts: -1 }).limit(20)
   ```

So yes—**you can detect these queries in open-source Mongo** by enabling profiling and inspecting `nReturned` in `system.profile`. The tradeoff is that level 2 profiling has cost (I/O and storage), so we use it for short bursts or in staging. Level 1 (slow only) is cheaper and still surfaces many heavy queries.

## Setting Up Vector Search in MongoDB Community Edition

For **local development** we use the **MongoDB Atlas Local** Docker image (`mongodb/mongodb-atlas-local:latest`). It bundles `mongod` and **mongot** (the search/vector process) in one container, so vector search works with no extra configuration. Our compose file uses this image and exposes the usual port; the app creates vector indexes via the same API we use in production.

For **self-hosted MongoDB Community Edition** (e.g. your own server or VM), vector search is supported starting with **MongoDB 8.2** and requires running **mongot** alongside **mongod**:

1. **Install MongoDB Community Server 8.2+** on a supported Linux host.
2. **Run as a replica set.** Search/vector features expect a replica set (e.g. a single-node replica set is enough for dev/staging).
3. **Download and run mongot.** The search/vector engine is a separate binary from the MongoDB downloads page. Start mongot so it connects to your replica set (configuration is documented in the MongoDB manual).
4. **Create vector indexes with the same API.** Our code uses the `createSearchIndexes` command with a `vectorSearch` index definition (path, dimensions, similarity). That command works for both Atlas and self-hosted 8.2+ with mongot—no separate code path.

Example of what the application sends (and what you could run via `mongosh` or any driver):

```javascript
db.runCommand({
  createSearchIndexes: "kb_vectors_<kb_id>",
  indexes: [{
    name: "kb_vector_index",
    type: "vectorSearch",
    definition: {
      fields: [
        { type: "vector", path: "embedding", numDimensions: 1536, similarity: "cosine" },
        { type: "filter", path: "organization_id" },
        { type: "filter", path: "metadata_snapshot.tag_ids" },
        { type: "filter", path: "metadata_snapshot.upload_date" }
      ]
    }
  }]
})
```

So: **local dev** = Atlas Local image (easiest). **Production** = Atlas (managed) or DocumentDB. **Self-hosted Community** = MongoDB 8.2+ + mongot + replica set, same `createSearchIndexes` and `$vectorSearch` usage.

## Knowledge Bases on Top of Vector Search

We implement **knowledge bases** in DocRouter (and the shared backend) using MongoDB’s vector search and embeddings:

- **Per–knowledge base collection:** Vectors live in collections named `kb_vectors_<kb_id>`. Each document has an `embedding` (array of floats), `chunk_text`, `document_id`, `chunk_index`, `metadata_snapshot` (e.g. document name, tag_ids, upload_date), and org scoping.
- **Indexing pipeline:** Text is chunked (e.g. with Chonkie: token/sentence/recursive), embeddings are generated via LiteLLM (with caching and optional batching), and vectors are written in an atomic “blue-green” style: delete old vectors for the document, insert new ones, update `document_index` and KB stats inside a transaction.
- **Search:** The query string is embedded with the same model, then we run an aggregation whose first stage is `$vectorSearch` (index `kb_vector_index`, path `embedding`, `queryVector`, `numCandidates`, `limit`). We apply filters (e.g. `organization_id`, tag_ids, date range) in the index definition or in the pipeline, add `vectorSearchScore`, and optionally coalesce neighboring chunks for context.
- **Vector index creation:** When a KB is created or its embedding model (dimensions) is set, we call `create_vector_search_index()` which uses `createSearchIndexes` with the vector + filter fields above. Same path for Atlas and Community 8.2+ with mongot.

So the “knowledge base” is: a config document in `knowledge_bases`, a dedicated vector collection per KB, a vector search index, and the indexing/search logic in `analytiq_data.kb` (indexing, search, reconciliation, embedding cache).

---

In short: we get **horizontal scaling**, **strict schema via migrations**, **predictable indexing**, and **vector search for RAG**—with one backend shared between DocRouter and SigAgent, and the same patterns whether we run on local Mongo, Atlas, or DocumentDB (and, for vector search, MongoDB Community 8.2+ with mongot when self-hosting).
