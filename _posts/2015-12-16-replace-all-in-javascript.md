---
title: Replace all in JavaScript
layout: post
---

The JavaScript `.replace()` function will only replace the first instance if you use just two strings like this:

```js
var text = "This is the blog of Adriaan, the worst blog in the universe";
text.replace("blog", "life"); // 'This is the life of Adriaan, the worst blog in the universe'
```

So to replace all occurrences of `blog` you can use:

```js
text.replace(/blog/g, "life");
```

But that is [30% slower than](https://jsperf.com/replace-vs-split-join-vs-replaceall/23) this:

```js
text.split("blog").join("life");
```
