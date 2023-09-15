---
---

# {{ site.title }}

{% for post in site.posts %}
- **[{{ post.title }}]({{ site.baseurl }}{{ post.url }})**  
  _Posted on {{ post.date | date: "%b %-d, %Y" }}_  
  {{ post.excerpt }}
{% endfor %}

## Search Posts

<input type="text" id="search-input" placeholder="Enter search term...">
<ul id="search-results"></ul>

<script src="https://cdn.jsdelivr.net/npm/lunr@2.3.9/lunr.min.js"></script>
<script>
document.addEventListener("DOMContentLoaded", function() {
  var idx;
  var docs;
  var baseUrl = "{{ site.baseurl }}";
  
  // Download the data
  fetch(baseUrl + '/search.json')
    .then(response => response.json())
    .then(data => {
      docs = data;
      idx = lunr(function() {
        this.ref('url');
        this.field('title');
        this.field('content');
        data.forEach(function(doc) {
          this.add(doc);
        }, this);
      });
    });

// Handle search
document.getElementById('search-input').addEventListener("keyup", function() {
    var query = this.value;
    var results = idx.search(query + "*");  // <--- Add the wildcard here
    displayResults(results);
});

  function displayResults(results) {
    var searchResults = document.getElementById('search-results');
    if (results.length) {
      var output = '';
      results.forEach(function(result) {
        var item = docs.find(i => i.url === result.ref);
        output += '<li><a href="' + baseUrl + item.url + '">' + item.title + '</a></li>';
      });
      searchResults.innerHTML = output;
    } else {
      searchResults.innerHTML = '<li>No results found</li>';
    }
  }
});
</script>
