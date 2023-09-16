---
---
This onw will provide search with search snippet

```
test
test
test
```


This script now provides a snippet of content around the searched term. The snippet is a simple one, taking 30 characters before the search term and 60 characters after it. You can adjust these numbers as needed.

However, this is a naive approach, and there are edge cases it won't handle perfectly, such as when the search term appears multiple times in the content. For a more advanced search experience, you'd typically utilize a dedicated search solution like Algolia or Elasticsearch that provides this functionality out of the box. But for a simple blog, the above should suffice.
