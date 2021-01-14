---
title: "Vue 2: this.$refs is undefined under v-if portion"
layout: post
---

`this.$refs` is undefined when the element with the `ref` attribute is not visable at the moment you use the `this.$refs` object. This costs me some time to figure out.

Let's say you have an app that looks like this:

```html
<div id="app">
  <p><button @click="openBox">Toggle</button></p>
  <div v-if="open">
    <input type="text" ref="inputField" />
   </div>
</div>

<script>
var app = new Vue({
  el: "#app",
  data: {
    open: false
  },
  methods: {
    openBox: function() {
      this.open = true;
      this.$refs.inputField.focus();
    }
  }
});
</script>
```

It has a button that opens a div that contains an input field with a `ref` attribute. Looks great, but...

When running this and click the button you will get this error: `TypeError: this.$refs.inputField is undefined`.

Why? 

Because the element is not rendered in the DOM yet.

You can solve it by using the `$nextTick` function:

```html
<div id="app">
  <p><button @click="openBox">Toggle</button></p>
  <div v-if="open">
    <input type="text" ref="inputField" />
   </div>
</div>

<script>
var app = new Vue({
  el: "#app",
  data: {
    open: false
  },
  methods: {
    openBox: function() {
      this.open = true;
      this.$nextTick(
        function () {
          this.$refs.inputField.focus();
        }.bind(this)
      );
    }
  }
});
</script>
```

Make sure to place the `this.open = true` outside of that `$nextTick` function. `$nextTick` waits for the DOM to be rendered and then runts the callback function you specify.
