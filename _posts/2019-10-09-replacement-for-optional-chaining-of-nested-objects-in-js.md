---
title: Replacement for optional chaining for nested JS objects
---

Sometimes you have to get a variable from a nested object like this

```js
const customer = {
  sources: {
    data: [{
      type: 'card',
      last4: '1234'
    }]
  }
}
```

If you want to get the value `1234`, you could do this:

```js
const { last4 } = customer.sources.data[0]
```

But if you don't know if the whole object will be there (like with this Stripe response), you'll need to check for every variable:

```js
let last4
if (customer
  && customer.sources
  && customer.sources.data
  && customer.sources.data[0]
  && customer.sources.data[0].last4) {
    last4 = customer.sources.data[0].last4
}
```

Or you could do a `try...catch`:

```js
let last4
try {
  last4 = customer.sources.data[0].last4
} catch (error) {
  // do nothing
}
```

But I like to have a little function that does this for me:

```js
// Copy this function
const get = (object, selector) => {
  if (typeof object !== 'object' || typeof selector !== 'string') return
  return selector.replace(/\[/g, '.[').split('.').reduce((prev, curr) => {
    if (prev === undefined) return prev
    if (curr.startsWith('[')) return prev[curr.slice(1, -1)]
    else return prev[curr]
  }, object)
}

// So you can do
const { last4 } = get(customer, 'sources.data[0]')
```

There is a relevant [proposal by tc39](https://github.com/tc39/proposal-optional-chaining) (thanks [@TehShrike](https://twitter.com/TehShrike/status/1058093341861179393)).

> Inspired by the Ember [`get`-function](https://emberjs.com/api/ember/3.5/functions/@ember%2Fobject/get)
