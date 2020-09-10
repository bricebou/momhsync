---
title: 'Twig pagination for Pico 2 '
date: '01-07-2018 00:00'
taxonomy:
    category:
        - Web
    tag:
        - pagination
        - 'Pico CMS'
        - Twig
twitterenable: true
twittercardoptions: summary
facebookenable: true
---

Pico 2.0 introduces some major changes including the possibility to [access HTTP parameters](http://picocms.org/in-depth/features/http-params/).    
This allows us to let pagination plugins aside and to code a pagination only with Twig.    
Here is how!

Inside your `index.twig` (for example), paste this code and set the variables:

```twig
{#  number of pages to display per page #}
{% set pagi_slice_length = 5 %}
{#  the array of pages to use #}
{% set pagi_pages_array = pages %}
{#  the base URL of the page the pagination applied
    it can be a more complex string, concatenated like this:
    base_url ~ "/tag/" ~ current_tag
#}
{% set pagi_base_url = base_url %}
{#  the name to use as the URL parameter (?page=xxx) #}
{% set pagi_http_param = 'page' %}
 
{% if url_param(pagi_http_param, 'int') is not null %}
{% set pagi_basepage = url_param(pagi_http_param, 'int') %}
{% set pagi_slice_start = pagi_slice_length * (pagi_basepage - 1) %}
{% else %}
{% set pagi_slice_start = 0 %}
{% set pagi_basepage = 1 %}
{% endif %}
 
{% set pagi_maxpage = (pagi_pages_array|length / pagi_slice_length)|round(0, 'ceil') %}
 
{% for page in pagi_pages_array|slice(pagi_slice_start, pagi_slice_length, preserve_keys) %}      
    <article>
        <h2 class="h1"><a href="{{ page.url }}">{{ page.title }}</a></h2>
    </article>
{% endfor %}
 
{% include 'includes/pagination.twig' with [pagi_basepage, pagi_maxpage, pagi_base_url, pagi_http_param] %}
```

You have to create a `includes/pagination.twig` file in which you have to paste this code:

```twig
{% set pagi_next_page = pagi_basepage + 1 %}
{% set pagi_prev_page = pagi_basepage - 1 %}
{% if pagi_maxpage > 1 %}
  <ul>
    {% if pagi_prev_page is not null %}
      <li class="pi" {% if pagi_prev_page < 1 %}style="visibility: hidden;"{% endif %}><a href="{% if pagi_prev_page == 1 %}{{ pagi_base_url }}{% else %}{{ pagi_base_url ~ '/?' ~ pagi_http_param ~ '=' ~  pagi_prev_page }}{% endif %}" title="Newest posts" id="prev_page_link">Newest posts</a></li>
    {% endif %}

    {% for item in range(1, pagi_maxpage|number_format) %}
      <li class="pi {%if item == pagi_basepage %} pi_a{%endif %}"><a href="{% if item == 1 %}{{ pagi_base_url }}{% else %}{{ pagi_base_url ~ '/?' ~ pagi_http_param ~ '=' ~ item }}{% endif %}" class="page_item {%if item == pagi_basepage %} page_active{%endif %}">{{ item }}</a></li>
    {% endfor %}
    
    {% if pagi_next_page is not null %}
      <li class="pi" {% if pagi_next_page > pagi_maxpage %}style="visibility: hidden;"{% endif %}><a href="{{ pagi_base_url ~ '/?' ~ pagi_http_param ~ '=' ~ pagi_next_page }}" title="Older posts" id="next_page_link">Older posts</a></li>
    {% endif %}
  </ul>
{% endif %}
```

It's awful, but it works ! ^^

If you have too many pages or simply want to reduce the number of displayed items, read the post ["Twig pagination for Pico 2, round 2"](/blog/twig-pagination-for-pico-2-round-2).