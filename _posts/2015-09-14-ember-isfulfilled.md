---
title: Ember isFulfilled
layout: post
---

For the Member Get Member Company we build an admin in Ember with us of the [ember-cli-admin](https://github.com/ember-admin/ember-cli-admin) addon.
We want to show to the users of the admin if the current user does not have offers. We can just make a simple `if`-statement,
but isn't it nicer to have the text to be always right? We use the `isFulfilled`-flag for that.

{% raw %}

```handlebars
{{#if model.offers}}
  {{#each model.offers as |offer|}}
    <li>{{offer.id}}</li>
  {{/each}}
{{else if model.offers.isFulfilled}}
  <p>This user has absolutely no offers</p>
{{else}}
  <p>Loading offers...</p>
{{/if}}
```

{% endraw %}

Ember 2.0.0
