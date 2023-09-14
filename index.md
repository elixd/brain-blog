---
layout: home
---

Welcome to my blog2!

<!-- This is for the search bar -->
<input type="text" id="search-input" placeholder="Search for posts...">
<ul id="results-container"></ul>

<!-- This script makes the search work -->
<script src="https://cdn.jsdelivr.net/npm/simple-jekyll-search@1.7.1/dest/simple-jekyll-search.min.js"></script>
<script>
SimpleJekyllSearch({
  searchInput: document.getElementById('search-input'),
  resultsContainer: document.getElementById('results-container'),
  json: '{{ site.baseurl }}/search.json',
  searchResultTemplate: '<li><a href="{url}" title="{desc}">{title}</a></li>',
  noResultsText: 'No results found'
})
</script>
