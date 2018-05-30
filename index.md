---
title: Hello
layout: default
---
# Hello 

{% for  pagesSite in site.pages %}
    {% if pagesSite.title !== page.title %}
         <a href="{{ pagesSite.url }}">{{ pagesSite.title }}</a>
    {% endif %}
{% endfor %}