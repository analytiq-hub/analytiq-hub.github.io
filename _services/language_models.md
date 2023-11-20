---
title: "Language Models AI"
date: 2018-11-18T12:33:46+10:00
weight: 2
---

We implement on-premise, on-VPC infrastructure based on all major vendors: OpenAI, Databricks, AWS, GCP, Cohere.

We design and develop using open source models like Langchain, LlamaIndex, Unstructured. We employ similarity databases like Pinecone, Weaviate, ChromaDB, and Postgres.

We can design the product architecture, provide the full implementation - or implement specific components, as needed.

#### Example implementation

An example implementation is the [Analytiq Language App](https://github.com/analytiq-hub/analytiq-language-app).

#### What is the difficult part in the implementation?

Tools are already available to build the entire Large Language Models architecture in-house, on your own cloud. Multiple options are available at each step.

The difficult part, often times, is data selection, preparation, and annotation, which is very customer-specific. For example:
- Some types of data may not be parsed correctly during ingestion.
- Or, some types of data require specific permissions to access, and cannot be commingled with all the data
- Or, some data may be company-confidential, and must not leave the premises. This requires a full implementation of the language model architecture in-house, at least inasfar as model serving, but also possibly model fine tuning.

For this reason, any off the shelf tools need to be paired with substantive work in selecting and preparing data inputs to the tools.