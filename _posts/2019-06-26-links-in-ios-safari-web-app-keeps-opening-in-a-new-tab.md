---
title: Links in iOS Safari WebApp keeps opening in a new tab
---

When you use the `<meta name="apple-mobile-web-app-capable" content="yes">` tag in your HTML the links in your app with open in a new tab in Safari. This is something you usually don't want.

Put this as the first script in your `<head>`:

```js
if (('standalone' in navigator) && navigator.standalone) {
  document.addEventListener('click', function(e) {
    var curnode = e.target
    while (!(/^(a|html)$/i).test(curnode.nodeName)) {
      curnode = curnode.parentNode
    }
    if ('href' in curnode
      && (chref = curnode.href).replace(document.location.href, '').indexOf('#')
      && (!(/^[a-z\+\.\-]+:/i).test(chref)
      || chref.indexOf(document.location.protocol + '//' + document.location.host) === 0)
    ) {
      e.preventDefault()
      document.location.href = curnode.href
    }
  }, false)
}
```

It opens all you own links in the same app window and other links in a new tab in Safari.

> This code will not work with `javascript:history.go(-1)`.

Source: [gist.github.com/irae/1042167](https://gist.github.com/irae/1042167), [gist.github.com/kylebarrow/1042026](https://gist.github.com/kylebarrow/1042026)
