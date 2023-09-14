---
layout: null
---
[
{% for post in site.posts %}
  {
    "title": "{{ post.title | escape }}",
    "content": "{{ post.content | strip_html | escape }}",
    "url": "{{ site.baseurl }}{{ post.url }}"
  }{% unless forloop.last %},{% endunless %}
{% endfor %}
]
