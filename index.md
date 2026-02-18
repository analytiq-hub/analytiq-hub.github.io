---
layout: default
title: "Analytiq Hub - Data+AI Solutions"
---

<div class="max-w-6xl mx-auto px-4 sm:px-6 md:px-8 py-4 md:py-12">
    <!-- Hero Section -->
    <header class="text-center md:mb-12 mb-8">
        <h1 class="text-4xl md:text-5xl font-bold text-gray-900 mb-6">From Zero to <span class="bg-gradient-to-r from-blue-600 to-purple-600 bg-clip-text text-transparent">Scale</span>: AI Solutions That Actually Work</h1>
        <div class="text-xl md:text-2xl text-gray-600 mb-8">
            <p class="mb-4">Specialized expertise for biotech, healthtech, and regulated industries</p>
        </div>
        <div class="flex justify-center items-center">
            <a href="/contact/" class="inline-block bg-gradient-to-r from-blue-600 to-purple-600 hover:from-blue-700 hover:to-purple-700 text-white px-8 py-4 rounded-lg font-semibold text-lg transition-all duration-200 shadow-lg hover:shadow-xl">
                Contact Us
            </a>
        </div>
    </header>

    <main>
        <!-- Success Stories Section -->
        <section class="bg-white rounded-lg shadow-lg p-8 mb-12">
            <h2 class="text-3xl font-semibold text-gray-900 mb-6 text-center">Success Stories</h2>
            <div class="max-w-3xl mx-auto text-center">
                <p class="text-lg text-gray-600 mb-6">
                    See how we've helped businesses like yours streamline operations with AI
                </p>
                <div class="grid md:grid-cols-3 gap-8">
                    {% include case-study-card.html 
                        title="Healthcare Automation That Delivers ROI" 
                        description="How we built a zero-to-ten DME automation platform that enabled 4x team growth" 
                        url="/case-studies/dme/" %}
                    
                    {% include case-study-card.html 
                        title="Epic EHR Integration That Reduces Errors" 
                        description="Zero-touch Epic automation reducing processing time by 90% while ensuring 100% compliance" 
                        url="/case-studies/epic_dme_order_processing/" %}
                    
                    {% include case-study-card.html 
                        title="Insurance Automation That Reduces Manual Work" 
                        description="AI-powered submission processing reducing manual work by 75%" 
                        url="/case-studies/insurance_wholesaler/" %}
                </div>
                <div class="text-center mt-6">
                    <a href="/case-studies/" class="text-blue-600 hover:text-blue-800 font-medium">View All Success Stories →</a>
                </div>
            </div>
        </section>

        <!-- Testimonials Section -->
        <section class="bg-blue-50 rounded-lg shadow-lg p-8 mb-12">
            <h2 class="text-3xl font-semibold text-gray-900 mb-8 text-center">What Our Clients Say</h2>
            <div id="testimonials-container" class="grid md:grid-cols-2 gap-8 max-w-6xl mx-auto">
                <!-- Featured testimonials (always visible) -->
                {% include testimonial-card.html 
                    quote="Analytiq Hub was fundamental to our early growth. Andrei helped us with foundational architectural decisions that drove our early successes. Highly recommended for early startups looking for technical leadership!"
                    name="Kevin Chen"
                    title="Director of Engineering"
                    company="Ashvin.AI"
                    image="/assets/images/kevin_chen.jpg"
                    linkedin="https://www.linkedin.com/in/kev1nch3n/" %}
                    
                {% include testimonial-card.html 
                    quote="...Thanks to his contributions, we made tremendous progress and significantly accelerated our AI roadmap. Would definitely recommend him to any team that is looking for guidance in implementing AI. He is an outstanding engineer and architect."
                    name="Marius Popovici"
                    title="Senior Software Engineering Manager"
                    company="STARLIMS"
                    image="/assets/images/marius_popovici.jpg"
                    linkedin="https://www.linkedin.com/in/mariuspopovici/" %}
                
                <!-- Hidden testimonials (shown on "Load More") -->
                <div class="testimonial-hidden hidden">
                    {% include testimonial-card.html
                        quote="Andrei was instrumental to our efforts to build a foundation of our AI co-pilot and chat assistant at STARLIMS. Andrei quickly learned the STARLIMS product, tech, goals and deliverables. In just a few months, in partnership with our team, he delivered a foundation that will transform our business by reducing both development and support times for STARLIMS and our customers."
                        name="Lauren Whitsell"
                        title="Product Manager"
                        company="STARLIMS"
                        image="/assets/images/lauren_whitsell.jpg" %}
                </div>

                <div class="testimonial-hidden hidden">
                    {% include testimonial-card.html 
                        quote="Analytiq helped us create custom AI solutions for the healthcare industry. Andrei has great knowledge of the current AI technologies and helped us apply them in an effective manner. Their experience helped generate tremendous value to our business."
                        name="Albert Woo"
                        title="President"
                        company="Boston Medical Data"
                        image="/assets/images/albert_woo.jpeg"
                        linkedin="https://www.linkedin.com/in/albert-woo-56a96168/" %}
                </div>
                
                <div class="testimonial-hidden hidden">
                    {% include testimonial-card.html 
                        quote="Andrei continues to impress over the 20+ years I've known him. He listens to customer needs and applies the right technologies and processes to the solution. And now he's leading the way with value-building AI solutions."
                        name="Brian Suthoff"
                        title="Entrepreneur & Investor"
                        image="/assets/images/brian_suthoff.jpeg"
                        linkedin="https://www.linkedin.com/in/suthoff/" %}
                </div>
                
                <div class="testimonial-hidden hidden">
                    {% include testimonial-card.html
                        quote="🎉🎉 a fantastic meetup last week in Boston by AICamp Boston community. Thanks speakers Andrei Radulescu-Banu … for their deep dive into cutting-edge tech and insightful discussions."
                        name="Bill Liu"
                        title="Founder"
                        company="AICamp"
                        image="/assets/images/bill_liu.jpeg"
                        linkedin="https://www.linkedin.com/in/billliu1202/" %}
                </div>

            </div>
            
            <!-- Load More / Load Less Button -->
            <div class="text-center mt-8">
                <button id="load-more-testimonials" class="bg-blue-600 hover:bg-blue-700 text-white px-6 py-3 rounded-lg font-semibold transition-colors duration-200">
                    Load More Testimonials
                </button>
            </div>
        </section>
        
        <script src="{{ "/assets/js/testimonials.js" | relative_url }}"></script>

        <!-- Services Section -->
        <section class="bg-white rounded-lg shadow-lg p-8 mb-12">
            <h2 class="text-3xl font-semibold text-gray-900 mb-6 text-center">Our Services</h2>
            <div class="grid md:grid-cols-3 gap-8">
                <div class="text-center">
                    <div class="bg-blue-100 rounded-full w-16 h-16 flex items-center justify-center mx-auto mb-4">
                        <!-- Heroicon: briefcase -->
                        <svg class="w-8 h-8 text-blue-600" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M20.25 14.15v4.25c0 1.094-.787 2.036-1.872 2.18-2.087.277-4.216.42-6.378.42s-4.291-.143-6.378-.42c-1.085-.144-1.872-1.086-1.872-2.18v-4.25m16.5 0a2.18 2.18 0 0 0 .75-1.661V8.706c0-1.081-.768-2.015-1.837-2.175a48.114 48.114 0 0 0-3.413-.387m4.5 8.006c-.194.165-.42.295-.673.38A23.978 23.978 0 0 1 12 15.75c-2.648 0-5.195-.429-7.577-1.22a2.016 2.016 0 0 1-.673-.38m0 0A2.18 2.18 0 0 1 3 12.489V8.706c0-1.081.768-2.015 1.837-2.175a48.111 48.111 0 0 1 3.413-.387m7.5 0V5.25A2.25 2.25 0 0 0 13.5 3h-3a2.25 2.25 0 0 0-2.25 2.25v.894m7.5 0a48.667 48.667 0 0 0-7.5 0" />
                        </svg>
                    </div>
                    <h3 class="text-xl font-medium text-gray-900 mb-2">Fractional CTO</h3>
                    <p class="text-gray-600">Strategic technology leadership and AI adoption guidance for startups and growing companies</p>
                </div>
                <div class="text-center">
                    <div class="bg-blue-100 rounded-full w-16 h-16 flex items-center justify-center mx-auto mb-4">
                        <!-- Heroicon: cube-transparent -->
                        <svg class="w-8 h-8 text-blue-600" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" d="m21 7.5-9-5.25L3 7.5m18 0-9 5.25m9-5.25v9l-9 5.25M3 7.5l9 5.25M3 7.5v9l9 5.25m0-9v9" />
                        </svg>
                    </div>
                    <h3 class="text-xl font-medium text-gray-900 mb-2">Full Stack Implementation</h3>
                    <p class="text-gray-600">End-to-end development of AI solutions and custom platforms across Azure, AWS, and Databricks</p>
                </div>
                <div class="text-center">
                    <div class="bg-blue-100 rounded-full w-16 h-16 flex items-center justify-center mx-auto mb-4">
                        <!-- Heroicon: cpu-chip -->
                        <svg class="w-8 h-8 text-blue-600" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M8.25 3v1.5M4.5 8.25H3m18 0h-1.5M4.5 12H3m18 0h-1.5m-15 3.75H3m18 0h-1.5M8.25 19.5V21M12 3v1.5m0 15V21m3.75-18v1.5m0 15V21m-9-1.5h10.5a2.25 2.25 0 0 0 2.25-2.25V6.75a2.25 2.25 0 0 0-2.25-2.25H6.75A2.25 2.25 0 0 0 4.5 6.75v10.5a2.25 2.25 0 0 0 2.25 2.25Zm.75-12h9v9h-9v-9Z" />
                        </svg>
                    </div>
                    <h3 class="text-xl font-medium text-gray-900 mb-2">AI and Data Solutions</h3>
                    <p class="text-gray-600">Design AI systems that streamline operations and meet regulatory requirements</p>
                </div>
            </div>
        </section>

        <!-- Why Choose Us -->
        <section class="bg-gradient-to-r from-blue-600 to-purple-600 rounded-lg shadow-lg p-8 mb-12">
            <div class="text-center">
                <h2 class="text-3xl font-semibold text-white mb-6">Why Analytiq Hub?</h2>
                <p class="text-xl text-blue-100 mb-8">
                    Specialized expertise in biotech and healthtech with comprehensive AI solutions
                </p>
                <div class="grid md:grid-cols-2 gap-8 max-w-4xl mx-auto">
                    <div class="bg-white bg-opacity-10 rounded-lg p-6">
                        <h3 class="text-xl font-medium text-white mb-3">Industry Focus</h3>
                        <ul class="text-blue-100 space-y-2 text-left">
                            <li>• Biotech and healthtech specialization</li>
                            <li>• Lab informatics expertise</li>
                            <li>• Medical equipment sector knowledge</li>
                            <li>• Startup-focused approach</li>
                        </ul>
                    </div>
                    <div class="bg-white bg-opacity-10 rounded-lg p-6">
                        <h3 class="text-xl font-medium text-white mb-3">Technical Excellence</h3>
                        <ul class="text-blue-100 space-y-2 text-left">
                            <li>• AI and machine learning expertise</li>
                            <li>• Multi-cloud platform experience</li>
                            <li>• Custom AI solution development</li>
                            <li>• Strategic technology partnership</li>
                        </ul>
                    </div>
                </div>
            </div>
        </section>
    </main>
</div>