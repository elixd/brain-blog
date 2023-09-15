---
layout: home
title: Brain Blog
---

## Search Posts

<input type="text" id="search-input" placeholder="Search for posts...">
<ul id="search-results"></ul>

<script src="https://cdn.jsdelivr.net/npm/lunr@2.3.9/lunr.min.js"></script>
<script>
document.addEventListener("DOMContentLoaded", function() {
  var idx;
  var docs;
  var baseUrl = "{{ site.baseurl }}";
  
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

  document.getElementById('search-input').addEventListener("keyup", function() {
    var query = this.value;
    var results = idx.search(query);
    displayResults(results);
  });

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
