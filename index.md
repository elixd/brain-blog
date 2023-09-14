---
layout: default
---

# Brain Blog

## Search
<input type="text" id="search-input" placeholder="Search for posts...">
<ul id="search-results"></ul>

## Latest Posts
<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <small>{{ post.date | date: "%B %d, %Y" }}</small>
  </li>
{% endfor %}
</ul>

<script>
  SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('search-results'),
    json: '/brain-blog/search.json',
    searchResultTemplate: '<li><a href="{url}" title="{title}">{title}</a></li>',
    noResultsText: 'No results found'
  });
</script>
