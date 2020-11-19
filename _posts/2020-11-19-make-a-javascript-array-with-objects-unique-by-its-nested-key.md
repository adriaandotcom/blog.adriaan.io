---
title: Make a JavaScript array with objects unique by its (nested) key
layout: post
---

Comparing an array with objects in JavaScript can be a bit annoying.

Let's say you have an array with objects like this:

```js
const array = [
  { name: "Adriaan", address: { city: "Amsterdam" } },
  { name: "Adriaan", address: { city: "Chiang Mai" } },
  { name: "Maria", address: { city: "Nazareth" } },
  { name: "Tim", address: { city: "Amsterdam" } }
]
```

You can use this little snippet to get the unique rows from above list:

```js
const unique = (array, property) => {
  const compare =
    typeof property === "function"
      ? property
      : (left, right) => left[property] == right[property];

  const newArray = [];

  array.forEach((right) => {
    const run = (left) => compare.call(this, left, right);
    var i = newArray.findIndex(run);
    if (i === -1) newArray.push(right);
  });

  return newArray;
};
```

For example when you want to get the objects with a unique name you can use:

```js
unique(array, "name") 

// [
//   { name: "Adriaan", address: { city: "Amsterdam" } },
//   { name: "Maria", address: { city: "Nazareth" } },
//   { name: "Tim", address: { city: "Amsterdam" } }
// ]
```

Or when you want to make the array unique by city (which is nested):


```js
const compage = (left, right) => left.address.city == right.address.city
unique(array, compare)

// [
//   { name: "Adriaan", address: { city: "Amsterdam" } },
//   { name: "Adriaan", address: { city: "Chiang Mai" } },
//   { name: "Maria", address: { city: "Nazareth" } }
// ]
```
