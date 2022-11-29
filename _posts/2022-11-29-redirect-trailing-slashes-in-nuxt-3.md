---
title: Remove and redirect trailing slashes from URLs in Nuxt 3
layout: post
---

Create this file in your middleware folder called `middleware/trailing-slash.global.js`:

```javascript
export default function ({ path, query, hash }) {
  if (path === "/" || !path.endsWith("/")) return;

  const nextPath = path.replace(/\/+$/, "") || "/";
  const nextRoute = { path: nextPath, query, hash };

  // 308 Permanent Redirect
  return navigateTo(nextRoute, { redirectCode: 308 });
}
```
