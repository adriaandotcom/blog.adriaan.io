---
title: Promise with a timeout
layout: post
---

Sometimes you don't want to wait for a JavaScript promise to resolve if it takes too long, just like this sentence.

The [`Promise.race`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/race) is a perfect solution for this. This method returns a promise that fulfills or rejects as soon as one of the promises in an iterable fulfills or rejects, with the value or reason from that promise. 

```js
const timeoutPromise = async ({
  promise,
  timeout = 1000,
  failureMessage
}) => {
  let timeoutHandle;

  const error = new Error(failureMessage || `Timeout of ${timeout}ms exceeded`);
  const timeoutPromise = new Promise((resolve, reject) => {
    timeoutHandle = setTimeout(() => reject(error), timeout);
  });

  const result = await Promise.race([promise, timeoutPromise]);

  clearTimeout(timeoutHandle);

  return result;
};

const sleep = new Promise((resolve) => setTimeout(resolve, 20));

// This will throw an error
await timeoutPromise({ promise: sleep(), timeout: 10 });
```

Inspired by [spin.atomicobject.com](https://spin.atomicobject.com/2020/01/16/timeout-promises-nodejs/).
