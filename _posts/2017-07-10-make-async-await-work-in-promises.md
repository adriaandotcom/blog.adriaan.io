I have some issues with getting [`async`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) and [`await`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await) working inside of `Promise`'s.

When using this code:

```js
function returnSomething(name) {
  return new Promise((resolve, reject) => {
    const somethingElse = await returnSomethingElse();
    return resolve(somethingElse);
  });
}
```

I got this error

```
/app/app.js:63
      const something = await returnSomethingElse();
                              ^^^^^^^^^^^^^^^^^^^
SyntaxError: Unexpected identifier
```

I knew I needed to add `async` into the mix, so I was changing the function to

```js
async function returnSomething(name) { // <--- this line
  return new Promise((resolve, reject) => {
    const somethingElse = await returnSomethingElse();
    return resolve(somethingElse);
  });
}
```

But still no luck, then I realised I didn't have an async function. The arrow function in the `new Promise` is not `async`! So I change the code to this and it worked:

```js
function returnSomething(name) {
  return new Promise(async (resolve, reject) => { // <--- this line
    const somethingElse = await returnSomethingElse();
    return resolve(somethingElse);
  });
}
```

But now we have a function with a Promise inside of it which is `async`. That seems like a bit much, don't you think?

Let's clean this up a bit. Because the function is already `async` it means it's behaving like a Promise. No need for `new Promise` inside of the function.

```js
async function returnSomething(name) {
  const somethingElse = await returnSomethingElse();
  return resolve(somethingElse);
}
```

If `returnSomethingElse` rejects, we will catch the error in the parent function of `returnSomething`:

```js
(async () => {
  try {
    const something = await returnSomething();
    console.log(something);
  } catch (e) {
    console.error(e);
  }
})();
```
