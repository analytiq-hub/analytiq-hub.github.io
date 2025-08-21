---
layout: default
title: Categories
---

<div class="max-w-6xl mx-auto px-4 sm:px-6 md:px-8 py-4 md:py-12">
    <!-- Header -->
    <header class="mb-12 pb-8 border-b border-gray-200">
        <h1 class="text-3xl md:text-4xl font-bold text-gray-900 mb-4">Categories</h1>
        <p class="text-gray-600 text-xl leading-relaxed">
            Browse blog posts by topic
        </p>
    </header>

    <div class="flex flex-col lg:flex-row gap-8 lg:items-start">
        <!-- Main Content -->
        <main class="lg:w-0 lg:flex-1">
            {% assign urlParams = page.url | split: '?' %}
            {% if urlParams.size > 1 %}
                <div>
            {% else %}
                <div data-pagination="categories">
            {% endif %}
                {% assign all_categories = site.posts | map: 'categories' | join: ',' | split: ',' | uniq | sort %}
                {% for category in all_categories %}
                    {% if category != '' %}
                        {% assign posts_in_category = site.posts | where_exp: "post", "post.categories contains category" %}
                        <div class="mb-12" id="{{ category | slugify }}">
                            <h2 class="text-2xl font-bold text-gray-900 mb-6">
                                <a href="{{ site.baseurl }}/categories?category={{ category | url_encode }}" 
                                   class="text-gray-900 hover:text-blue-600 transition-colors">
                                    {{ category | replace: '_', ' ' | capitalize }}
                                </a>
                                <span class="text-lg font-normal text-gray-500">({{ posts_in_category | size }} posts)</span>
                            </h2>
                            
                            <div class="space-y-6">
                                {% for post in posts_in_category %}
                                    <article class="border-b border-gray-100 pb-6 last:border-b-0 last:pb-0">
                                        <div class="flex flex-col md:flex-row gap-6">
                                            <div class="md:w-1/3 flex-shrink-0">
                                                {%- if post.image -%}
                                                    <div class="aspect-video bg-gray-50 rounded-lg overflow-hidden">
                                                        <img src="{{ post.image | relative_url }}" alt="{{ post.title | escape }}" class="w-full h-full object-contain p-2">
                                                    </div>
                                                {%- else -%}
                                                    <div class="aspect-video bg-gradient-to-br from-blue-50 to-indigo-100 rounded-lg flex items-center justify-center">
                                                        <svg class="w-12 h-12 text-blue-300" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1" d="M19 20H5a2 2 0 01-2-2V6a2 2 0 012-2h10a2 2 0 012 2v1m2 13a2 2 0 01-2-2V7m2 13a2 2 0 002-2V9.5a2 2 0 00-2-2h-1"/>
                                                        </svg>
                                                    </div>
                                                {%- endif -%}
                                            </div>
                                            <div class="flex-1">
                                                <header class="mb-4">
                                                    <h3 class="text-lg md:text-xl font-bold text-gray-900 mb-2">
                                                        <a href="{{ post.url | relative_url }}" class="text-gray-900 hover:text-blue-600 transition-colors">
                                                            {{ post.title }}
                                                        </a>
                                                    </h3>
                                                    <div class="flex flex-wrap items-center gap-3 text-sm text-gray-600">
                                                        <time datetime="{{ post.date | date: '%Y-%m-%d' }}">
                                                            {{ post.date | date: '%B %d, %Y' }}
                                                        </time>
                                                        {% if post.categories %}
                                                            <span class="text-gray-400">•</span>
                                                            <div class="flex flex-wrap gap-2">
                                                                {% for post_category in post.categories %}
                                                                    <span class="bg-blue-100 text-blue-700 px-2 py-1 rounded-md text-xs font-medium">
                                                                        {{ post_category | replace: '_', ' ' | capitalize }}
                                                                    </span>
                                                                {% endfor %}
                                                            </div>
                                                        {% endif %}
                                                    </div>
                                                </header>
                                                
                                                {% if post.excerpt %}
                                                    <div class="text-gray-700 leading-relaxed mb-4">
                                                        {{ post.excerpt | strip_html | truncate: 200 }}
                                                    </div>
                                                {% endif %}
                                                
                                                <footer>
                                                    <a href="{{ post.url | relative_url }}" 
                                                       class="inline-flex items-center gap-1 text-blue-600 hover:text-blue-800 font-medium transition-colors">
                                                        Read more
                                                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/>
                                                        </svg>
                                                    </a>
                                                </footer>
                                            </div>
                                        </div>
                                    </article>
                                {% endfor %}
                            </div>
                        </div>
                    {% endif %}
                {% endfor %}
            </div>
            
            {% if site.posts.size == 0 %}
                <div class="text-center py-12">
                    <p class="text-gray-600 text-lg">No blog posts available yet.</p>
                </div>
            {% endif %}
        </main>

        <!-- Sidebar -->
        <aside class="hidden lg:block w-80 flex-shrink-0">
            {% include blog-sidebar.html %}
        </aside>
    </div>
</div>

<script>
// Handle URL parameters for deep linking to specific categories
document.addEventListener('DOMContentLoaded', function() {
    const urlParams = new URLSearchParams(window.location.search);
    const category = urlParams.get('category');
    
    if (category) {
        // Find the category section and scroll to it
        const categoryId = category.toLowerCase().replace(/\s+/g, '-');
        const categoryElement = document.getElementById(categoryId);
        
        if (categoryElement) {
            // Wait a bit for the page to load completely
            setTimeout(() => {
                categoryElement.scrollIntoView({ behavior: 'smooth', block: 'start' });
                
                // Highlight the section briefly
                categoryElement.style.backgroundColor = '#f0f9ff';
                setTimeout(() => {
                    categoryElement.style.backgroundColor = '';
                }, 2000);
            }, 100);
        }
    }
});
</script>
