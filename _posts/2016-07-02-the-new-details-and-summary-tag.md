---
---

With this `details`-tag you can create foldable / collapsible content:

```html
<details>
  <summary>This is a summary of the collapsible content</summary>
  And this is the very long content..
  It can have <strong>HTML</strong>-tags in it as well!
</details>
```

This is the content when it *is* collapsed:

<img style="border: 1px solid gray; padding: 10px 15px;" src="/images/posts/collapsed.png" style="max-width: 336px;">

And this is the content when it *is not* collapsed:

<img style="border: 1px solid gray; padding: 10px 15px;" src="/images/posts/not-collapsed.png" style="max-width: 499px;">

It's handy for [hiding logs](https://x.com/ericclemmons/status/749223563790471169) in GitHub issues but you can still add them in a nice way for people who need them.

Browser support on desktop is not so great yet, but on mobile it is:

<img src="/images/posts/details-browser-support.png" style="max-width: 973px;">

See [caniuse.com/#feat=details](http://caniuse.com/#feat=details).
