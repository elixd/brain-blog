---
layout: home
---


## Search Posts

<input type="text" id="search-input" placeholder="Enter search term..." style="width:100%; padding: 10px; margin: 20px 0; border: 1px solid #ddd; border-radius: 4px;">
<ul id="search-results" style="list-style-type: none; padding-left: 0;"></ul>

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
