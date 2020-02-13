---
title: Make array with objects unique on multiple keys in javascript
---

Let's say you have an array with objects:

```js
const browsers = [
  {
    os: "OS X",
    os_version: "Catalina",
    browser: "chrome",
    browser_version: "30.0"
  },
  {
    os: "Windows",
    os_version: "7",
    browser: "chrome",
    browser_version: "40.0"
  },
  {
    os: "Windows",
    os_version: "7",
    browser: "chrome",
    browser_version: "50.0"
  }
];
```

Let's assume you only want to have browsers with unique `os` and `os_version`. Returning the first 2 browsers.

If you want to make this `browsers` array unique you can use this function:

```js
const makeUnique = (array = [], keys = []) => {
  if (!keys.length || !array.length) return [];

  return array.reduce((list, item) => {
    const hasItem = list.find(listItem =>
      keys.every(key => listItem[key] === item[key])
    );
    if (!hasItem) list.push(item);
    return list;
  }, []);
};
```

It returns the first match it finds, so in this case `browser_version: '40.0'` instead of `browser_version: '50.0'`.

You can use it like this:

```js
makeUnique(browsers, ["os", "os_version"]);
```

It will return something like this:


```js
[
  {
    os: 'OS X',
    os_version: 'Catalina',
    browser: 'chrome',
    browser_version: '30.0'
  },
  {
    os: 'Windows',
    os_version: '7',
    browser: 'chrome',
    browser_version: '40.0'
  }
]
```
