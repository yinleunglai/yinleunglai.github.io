---
title: "陪诊师创业联盟"
layout: default
pagination:
    enabled: true
description: "陪诊知识经验分享、陪诊预约指南、异地就医陪诊、全程陪诊"
---


<div class="container">
    
{% if page.url == "/" %}
    
    
    
    {% assign displayed_posts = "" | split: "," %}

    <!-- Prominent Latest Post -->
    {% assign latest_post = site.posts[0] %}
    {% if latest_post %}
        <div class="row mb-4 remove-site-content-margin">
            <div class="col-md-12">
                <div class="card border-0 box-shadow hero-post">   
                    <a href="{{site.baseurl}}{{latest_post.url}}">
                        <div class="topfirstimage" style="background-image: url({% if latest_post.image contains "://" %}{{ latest_post.image }}{% else %} {{site.baseurl}}/{{ latest_post.image}}{% endif %}); height: 300px; background-size: cover; background-repeat: no-repeat; background-position: center;"></div>     
                    </a>
                    <div class="card-body px-0 pb-0 d-flex flex-column align-items-start">
                        <h2 class="h3 font-weight-bold">
                            <a class="text-dark" href="{{site.baseurl}}{{latest_post.url}}">{{ latest_post.title }}</a>
                        </h2>
                        <p class="excerpt">
                            {{ latest_post.excerpt | strip_html | strip_newlines | truncate: 200 }}
                        </p>
                        <div>
                            <small class="d-block text-muted">
                                In <span class="catlist">
                                    {% for category in latest_post.categories %}
                                    <a class="text-capitalize text-muted smoothscroll" href="{{site.baseurl}}/categories.html#{{ category | downcase }}">{{ category }}</a><span class="sep">, </span>
                                    {% endfor %}
                                </span>                   
                            </small>
                            <small class="text-muted">
                                {{ latest_post.date | date: '%b %d, %Y' }}
                            </small>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        {% assign displayed_posts = displayed_posts | push: latest_post.url %}
    {% endif %}

    <!-- Grid/Row of Other Recent/Featured Posts -->
    <div class="row recent-posts-grid mb-4">
        {% assign limit_recent = 3 %}
        {% assign recent_counter = 0 %}
        {% for post_item in site.posts offset:1 %}
            {% if recent_counter < limit_recent %}
                {% unless displayed_posts contains post_item.url %}
                    <div class="col-md-4">
                        {% include post-card-condensed.html post=post_item %}
                    </div>
                    {% assign displayed_posts = displayed_posts | push: post_item.url %}
                    {% assign recent_counter = recent_counter | plus: 1 %}
                {% endunless %}
            {% endif %}
        {% endfor %}

        {% comment %} Display sticky posts that haven't been displayed yet, up to a certain limit if needed {% endcomment %}
        {% assign sticky_limit = 3 %} {% comment %} Max number of sticky posts in this section if not already shown {% endcomment %}
        {% assign sticky_displayed_count = 0 %}
        {% for sticky_post in site.tags.sticky %}
            {% if sticky_displayed_count < sticky_limit %}
                {% unless displayed_posts contains sticky_post.url %}
                    <div class="col-md-4">
                         {% include post-card-condensed.html post=sticky_post highlighted=true %}
                    </div>
                    {% assign displayed_posts = displayed_posts | push: sticky_post.url %}
                    {% assign sticky_displayed_count = sticky_displayed_count | plus: 1 %}
                {% endunless %}
            {% endif %}
        {% endfor %}
    </div>

{% endif %} <!--endif page url is / -->
    
<!-- Rest of the posts with pagination -->
<div class="row mt-3">
   
    <div class="col-md-8 main-loop">
        
        <h4 class="font-weight-bold spanborder"><span>陪诊经验分享</span></h4>
        

        {% for post in paginator.posts %}
            {% unless displayed_posts contains post.url %}
                {% include main-loop-card.html %}
            {% endunless %}
        {% endfor %}
        
        <div class="mt-5">
         <!-- Pagination links -->
            {% if paginator.total_pages > 1 %}
            <ul class="pagination"> 
              {% if paginator.previous_page %}
                <li class="page-item"><a class="page-link" href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">&laquo; Prev</a></li>
              {% else %}
                <li class="page-item disabled"><span class="prev page-link">&laquo;</span></li>
              {% endif %}

              {% for page in (1..paginator.total_pages) %}
                {% if page == paginator.page %}
                <li class="page-item disabled"><span class="webjeda page-link">{{ page }}</span></li>
                {% elsif page == 1 %}
                <li class="page-item"><a class="page-link" href="{{site.baseurl}}/">{{ page }}</a></li>
                {% else %}
                <li class="page-item"><a class="page-link" href="{{ site.paginate_path | prepend: site.baseurl | replace: '//', '/' | replace: ':num', page }}">{{ page }}</a></li>
                {% endif %}
              {% endfor %}

              {% if paginator.next_page %}
                <li class="page-item"><a class="page-link" href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Next &raquo;</a></li>
              {% else %}
                <li class="page-item disabled"><span class="next page-link">&raquo;</span></li>
              {% endif %}
            </ul>
            {% endif %}      
        </div>
        
    </div>
    
    <div class="col-md-4">
        {% include sidebar-featured.html %}    
    </div>
    
</div>



</div>