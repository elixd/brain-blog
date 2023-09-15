---
layout: default
---

# {{ site.title }}

{% for post in site.posts %}
- **[{{ post.title }}]({{ site.baseurl }}{{ post.url }})**  
  _Posted on {{ post.date | date: "%b %-d, %Y" }}_  
  {{ post.excerpt }}
{% endfor %}

[Search posts]({{ site.baseurl }}/search)
