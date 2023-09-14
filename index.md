---
layout: default
---

# Brain Blog

<input type="text" id="search-input" placeholder="Search for posts...">
<ul id="search-results">
  <!-- search results will appear here -->
</ul>

<script>
  var search = new SimpleJekyllSearch({
    searchInput: document.getElementById('search-input'),
    resultsContainer: document.getElementById('search-results'),
    json: '/brain-blog/search.json',
    searchResultTemplate: '<li><a href="{url}">{title}</a></li>',
    noResultsText: 'No results found'
  });
</script>

# Latest Posts

<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a>
    <small>{{ post.date | date: "%B %d, %Y" }}</small>
  </li>
{% endfor %}
</ul>
