---
title: End to the endless if's to get a JavaScript value in (nested) objects
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
  try {
    if (typeof object !== 'object') return
    return eval(`object.${selector}`)
  } catch (error) {
    return
  }
}

// So you can do
const { last4 } = get(customer, 'sources.data[0]')
```

> Inspired by the Ember [`get`-function](https://emberjs.com/api/ember/3.5/functions/@ember%2Fobject/get)
