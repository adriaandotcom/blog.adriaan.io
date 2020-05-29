---
title: Open external links in a new tab in JavaScript
---

When you have links on your page but you want to open them in a new tab (when visitors are navigating to another website).

You can use this function:

```js
window.addEventListener("DOMContentLoaded", function externalLinks() {
  var anchors = document.getElementsByTagName("a");
  for (var i = 0; i < anchors.length; i++) {
    if (anchors[i].hostname !== window.location.hostname) {
      anchors[i].setAttribute("target", "_blank");
      anchors[i].setAttribute("rel", "noopener");
    }
  }
});
```

We include `rel="noopener"` to the links so your visitors are less vunrenable to attacks like [reverse tabnabbing](https://web.dev/external-anchors-use-rel-noopener/).
