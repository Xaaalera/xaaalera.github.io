---
title: Hello
layout: default
---
# Hello 

{% for  pagesSite in site.pages %}
    {% if pagesSite.title == page.title %}
      {% continue %}
      {% endif %}
      
         <a href="{{ pagesSite.url }}">{{ pagesSite.title }}</a>
         
         
{% endfor %}