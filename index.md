---
title: Hello
layout: default
---


# Hello #


{% for pagesSite in site.pages %}
{% if pagesSite.title != page.title and pagesSite.title is not empty  and pagesSite.title not null and pagesSite.title defined %}
<a href="{{ pagesSite.url }}">{{ pagesSite.title }}</a><br>
{% endif %}
{% endfor %}

<h2>{{ site.data.samplelist.docs_list_title }}</h2>
<ul>
   {% for item in site.data.samplelist.docs %}
      <li><a href="{{ item.url }}" alt="{{ item.title }}">{{ item.title }}</a></li>
   {% endfor %}
</ul>