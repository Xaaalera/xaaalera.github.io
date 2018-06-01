---
title: Hello
layout: default
---


# Hello #


{% for pagesSite in site.pages %}
{% if pagesSite.url == '/sitemap.xml'  %}
<a href="{{ pagesSite.url }}">Карта сайта</a><br>
{% else if  pagesSite.title is not empty %}
<a href="{{ pagesSite.url }}">{{ pagesSite.title }}</a><br>
{% endif %}
{% endfor %}
