---
title: How to block traffic from a specific IP address with Docker enabled 
layout: post
---

Using Docker can sometimes lead to an issue where all traffic routes through [Docker](https://www.docker.com/) via [iptables](https://en.wikipedia.org/wiki/Iptables), bypassing other layers such as [NGINX](https://www.nginx.com/). To mitigate this, you'll need to introduce a specific rule in iptables targeting the `DOCKER-USER` chain.

Suppose you aim to block all incoming traffic from the IP address `1.2.3.4`. Here's how you can achieve that:

## 1. View Current iptables Rules

Begin by checking the existing rules. This will give you an overview and confirm the presence of the "Chain DOCKER-USER":

```bash
sudo iptables -L -v -n
```

## 2. Add a Rule to Block the IP

Once you've verified the "Chain DOCKER-USER", proceed to add a rule that blocks the desired IP address. To block traffic from `1.2.3.4`, use the following command:

```bash
sudo iptables -I DOCKER-USER -s 1.2.3.4 -j DROP
```

After executing the above command, iptables will begin blocking traffic originating from the IP address `1.2.3.4`.

> It's important that this DROP row is above other rows in iptables.
