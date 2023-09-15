---
---

# {{ site.title }}

{% for post in site.posts %}
- **[{{ post.title }}]({{ post.url }})**  
  _Posted on {{ post.date | date: "%b %-d, %Y" }}_  
  {{ post.excerpt }}
{% endfor %}

[Search posts](/search)
