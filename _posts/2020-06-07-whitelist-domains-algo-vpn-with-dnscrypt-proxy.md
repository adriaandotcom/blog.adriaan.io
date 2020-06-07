---
title: Allow (whitelist) domains with Algo VPN in DNSCrypt Proxy
---

[DNSCrypt Proxy](https://www.dnscrypt.org/) is one of the tools build into the [Algo VPN ansible scripts](https://github.com/trailofbits/algo). It's great for blocking ads and trackers. I run a privacy friendly analytics tool called Simple Analytics. For developing my tool I need to make sure it's never blocked in my VPN.

> I use the term whitelist because it's being used within DNSCrypt and because people search with this keyword. I would prefer to call it allowed, allowlist, ignorelist, or something like that.

First navigate to your DNSCrypt settings:

```
cd /etc/dnscrypt-proxy
ls -la
```

You will find a file in there called `dnscrypt-proxy.toml`, open this file:

```
sudo vi dnscrypt-proxy.toml
```

It's a very long file so search for `whitelist` by hitting the <kbd>/</kbd> key. Type `whitelist` and hit <kbd>enter</kbd>. You will see the first hit of whitelist, use <kbd>n</kbd> to navigate to this line:

```
# whitelist_file = 'whitelist.txt'
```

Remove the `# ` before `whitelist_file`. In vim you can do this by hitting <kbd>i</kbd> for insert and just use your <kbd>backspace</kbd> to remove the `# `. Once you're done you hit <kbd>ESC</kbd> and type `:wq` <kbd>enter</kbd> (write quit).

> If something goes wrong you can always hit <kbd>esc</kbd> and <kbd>u</kbd> to undo.

Now DNSCrypt will look for a `whitelist.txt` file in this folder `/etc/dnscrypt-proxy`. Create this file now:

```
sudo touch /etc/dnscrypt-proxy/whitelist.txt
sudo vi /etc/dnscrypt-proxy/whitelist.txt
```

The last command opens the `whitelist.txt` file in vim. Just hit <kbd>i</kbd> and type the domains you want to allow and skip the blocklist/blacklist. One domain per line and without any subdomain. For me `whitelist.txt` looks like:

```
simpleanalytics.io
```

Once you're done you hit <kbd>esc</kbd> and type `:wq` <kbd>enter</kbd>.

You now created a list which will allow certain domains to bypass your blocklist. We now need to restart DNSCrypt:

```
sudo service dnscrypt-proxy restart
```

And you're done. You should be able to visit your allowed domains.
