---
title: 'Twig pagination for Pico 2, round 2'
date: '28-07-2018 00:00'
taxonomy:
    category:
        - Web
    tag:
        - pagination
        - 'Pico CMS'
        - Twig
twitterenable: true
twittercardoptions: summary
articleenabled: false
musiceventenabled: false
orgaenabled: false
orga:
    ratingValue: 2.5
orgaratingenabled: false
eventenabled: false
personenabled: false
musicalbumenabled: false
productenabled: false
product:
    ratingValue: 2.5
restaurantenabled: false
restaurant:
    acceptsReservations: 'yes'
    priceRange: $
facebookenable: true
---

After a [first round](/blog/twig-pagination-for-pico-2) where the challenge was to realize a pure Twig pagination for Pico 2 using it's new [HTTP parameters feature](http://picocms.org/in-depth/features/http-params/), I've worked on a new version. It's purpose? Dynamically reducing the number of displayed pages...

So, I've added the `pagi_max_near_by` variable to define how many pages around the active page should be displayed. Then, you have to add some more tests inside the `includes/pagination.twig` file:

```twig
{% set pagi_next_page = pagi_basepage + 1 %}
{% set pagi_prev_page = pagi_basepage - 1 %}
{% if pagi_max_near_by is null %}
  {% set pagi_max_near_by = 2 %}
{% endif %}

{% if pagi_maxpage > 1 %}
  <ul>
    {% if pagi_prev_page is not null %}
      <li class="pi" {% if pagi_prev_page < 1 %}style="visibility: hidden;"{% endif %}><a href="{% if pagi_prev_page == 1 %}{{ pagi_base_url }}{% else %}{{ pagi_base_url ~ '/?' ~ pagi_http_param ~ '=' ~  pagi_prev_page }}{% endif %}" title="Newest posts" id="prev_page_link">Newest posts</a></li>
    {% endif %}
    
    {% if pagi_basepage - pagi_max_near_by > 1 %}
      <li class="pi">&hellip;</li>
    {% endif %}
    
    {% for item in range(1, pagi_maxpage|number_format) %}
      <li {% if pagi_basepage - pagi_max_near_by > item or pagi_basepage + pagi_max_near_by < item  %}style="display:none;"{% endif %} class="pi {%if item == pagi_basepage %} pi_a{%endif %}"><a href="{% if item == 1 %}{{ pagi_base_url }}{% else %}{{ pagi_base_url ~ '/?' ~ pagi_http_param ~ '=' ~ item }}{% endif %}" class="page_item {%if item == pagi_basepage %} page_active{%endif %}">{{ item }}</a></li>
    {% endfor %}
    
    {% if pagi_basepage + pagi_max_near_by < pagi_maxpage %}
      <li class="pi">&hellip;</li>
    {% endif %}

    {% if pagi_next_page is not null %}
      <li class="pi" {% if pagi_next_page > pagi_maxpage %}style="visibility: hidden;"{% endif %}><a href="{{ pagi_base_url ~ '/?' ~ pagi_http_param ~ '=' ~ pagi_next_page }}" title="Older posts" id="next_page_link">Older posts</a></li>
    {% endif %}
  </ul>
{% endif %}
```

Of course, you can adjust the `pagi_max_near_by` variable template by template by passing it a an argument to your Twig `include`; for example, inside your `index.twig` template (please refer to [the first post](/blog/twig-pagination-for-pico-2) if you don't know how to use):

```twig
{% set pagi_slice_length = 5 %}
{% set pagi_pages_array = pagesbis %}
{% set pagi_base_url = base_url %}
{% set pagi_http_param = 'page' %}
{% set pagi_max_near_by = 3 %}

[... snip ...]
 
{% include 'includes/pagination.twig' with [pagi_basepage, pagi_maxpage, pagi_base_url, pagi_http_param, pagi_max_near_by] %}
```