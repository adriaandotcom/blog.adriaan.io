---
title: Valdiate email addresses with MX records in Node.js
layout: post
---

This might help others in their search for some email validator based on [MX DNS records](https://en.wikipedia.org/wiki/MX_record).

In Node.js it's important to specify the `dnsServers`, because otherwise it will only check the local resolver. In my case this was causing issues.

```js
const { Resolver } = require("dns").promises;

const isValidRegexEmail = (email) =>
  /[^@\s]+@[^@\s]+\.[^@\s]+/.test(email) && !/\s/.test(email.trim());

const dnsServers = [
  "1.1.1.1", // Cloudflare
  "1.0.0.1", // Cloudflare
  "8.8.8.8", // Google
  "8.8.4.4", // Google
  "208.67.222.222", // OpenDNS
  "208.67.220.220", // OpenDNS
];

const resolverOptions = {
  timeout: 3000,
  tries: Math.min(4, dnsServers.length),
};

const resolveDNS = async ({ type, value }) => {
  try {
    const resolver = new Resolver(resolverOptions);
    resolver.setServers(dnsServers);

    let addresses;
    switch (type) {
      case "mx":
        addresses = await resolver.resolveMx(value);
        break;
      case "lookup":
        addresses = await resolver.resolve(value);
        break;
      case "ns":
        addresses = await resolver.resolveNs(value);
        break;
      default:
        throw new Error(`Unknown DNS resolve type: ${type}`);
    }

    if (Array.isArray(addresses) && addresses.length > 0) {
      return { isValid: true, addresses };
    } else {
      return { isValid: false, addresses: [] };
    }
  } catch (error) {
    if (["ECONNREFUSED", "ENOTFOUND"].includes(error.code)) {
      return { isValid: false, addresses: [] };
    } else {
      return { isValid: false, addresses: [], error: error };
    }
  }
};

const isValidMxEmail = async (emailAddress = "") => {
  if (typeof emailAddress !== "string") return false;

  emailAddress = emailAddress.toLowerCase();

  if (!isValidRegexEmail(emailAddress)) return false;

  const [, domain] = emailAddress.split("@");
  const { isValid, addresses = [] } = await resolveDNS({
    type: "mx",
    value: domain,
  });

  const hasAddress = addresses.every(({ exchange }) => !!exchange);
  if (!hasAddress) return false;

  return isValid;
};

isValidMxEmail('henk@example.com').then(console.log); // false
```
