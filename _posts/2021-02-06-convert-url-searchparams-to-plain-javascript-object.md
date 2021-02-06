---
title: Convert URL searchParams to plain JS Object
layout: post
---

In Node.js the [`url.parse` API](https://nodejs.org/api/url.html#url_url_parse_urlstring_parsequerystring_slashesdenotehost) is deprecated. But one of the great things was getting an plain JavaScript object from your query params:

```js
const { query } = url.parse("https://example.com/?search=term&page=1", true);

console.log(query); // [Object: null prototype] { search: 'term', page: '1' }
```

> Want to know what `[Object: null prototype]` is? Check [this StackOverflow answer](https://stackoverflow.com/a/56298574/747044).

Fortunately this is also possible with the WHATWG URL API (`new URL`).

```js
const { searchParams } = new URL("https://example.com/?search=term&page=1");
const query = Object.fromEntries(searchParams);

console.log(query); // { search: 'term', page: '1' }
```

WHATWG URL API combined with [`Object.fromEntries`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/fromEntries) really helps here.
