---
layout: home
title: Brain Blog
---

## Search Posts

<input type="text" id="search-input" placeholder="Search for posts...">
<ul id="search-results"></ul>

<!-- Lunr.js Library for Full-Text Search -->
<script src="https://cdn.jsdelivr.net/npm/lunr@2.3.9/lunr.min.js"></script>

<!-- Search Functionality Script -->
<script>
  document.addEventListener("DOMContentLoaded", function() {
    // Declare necessary variables
    var idx;  // Lunr index
    var docs;  // Documents (posts) to search
    var baseUrl = "{{ site.baseurl }}";  // Base URL for the site

    // Fetch the search data from search.json and initialize the search index
    fetch(baseUrl + '/search.json')
      .then(response => response.json())
      .then(data => {
        docs = data;
        idx = lunr(function() {
          this.ref('url');
          this.field('title', { boost: 10 });
          this.field('content');
          this.pipeline.remove(lunr.stemmer);
          this.searchPipeline.remove(lunr.stemmer);
          data.forEach(function(doc) {
            this.add(doc);
          }, this);
        });
      });

    // Event listener for the search input
    document.getElementById('search-input').addEventListener("keyup", function() {
      var query = this.value;  // Search query
      var results = idx.search(query);  // Perform search
      displayResults(results);
    });

    // Display the search results
    function displayResults(results) {
      var searchResults = document.getElementById('search-results');
      if (results.length) {
        var output = '';
        results.forEach(function(result) {
          var item = docs.find(i => i.url === result.ref);
          output += '<li><a href="' + baseUrl + item.url + '">' + item.title + '</a><small>' + item.content.substring(0, 150) + '...</small></li>';
        });
        searchResults.innerHTML = output;
      } else {
        searchResults.innerHTML = '<li>No results found</li>';
      }
    }
  });
</script>
