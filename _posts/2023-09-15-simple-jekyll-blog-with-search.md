---
---
This onw will provide search with search snippet

```
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

    // Custom tokenizer for substrings
    // ... [same tokenizer code as before]

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
        var query = "*" + this.value + "*";
        var results = idx.search(query);
        displayResults(results, this.value);
    });

    function displayResults(results, query) {
        var searchResults = document.getElementById('search-results');
        if (results.length) {
            var output = '';
            results.forEach(function(result) {
                var item = docs.find(i => i.url === result.ref);
                var snippet = item.content;

                // Create a snippet around the search term for context
                var start = snippet.indexOf(query) - 30;
                start = start < 0 ? 0 : start;
                var end = start + query.length + 60;
                snippet = snippet.substring(start, end) + '...';

                output += '<li><a href="' + baseUrl + item.url + '">' + item.title + '</a><br><small>' + snippet + '</small></li>';
            });
            searchResults.innerHTML = output;
        } else {
            searchResults.innerHTML = '<li>No results found</li>';
        }
    }
});
</script>
```


This script now provides a snippet of content around the searched term. The snippet is a simple one, taking 30 characters before the search term and 60 characters after it. You can adjust these numbers as needed.

However, this is a naive approach, and there are edge cases it won't handle perfectly, such as when the search term appears multiple times in the content. For a more advanced search experience, you'd typically utilize a dedicated search solution like Algolia or Elasticsearch that provides this functionality out of the box. But for a simple blog, the above should suffice.
