---
title: Run a simple server on your Mac for your static files
---

Python3 has a nice way of starting a server: `python3 -m http.server 8080`. This works super nice, but I always forget how to type this command. So I created a little function in `~/.bash_profile` on my Mac:

```bash
server () {
  local port=${1:-8080}
  if [[ $port -eq 80 ]]; then
    sudo python3 -m http.server $port
  else
    python3 -m http.server $port
  fi
}
```

Now I can type `server 80` or `server` to run a simple server in my current directory.

> If you don't have or want python3 you can use `python -m SimpleHTTPServer 8080` (it was too slow for me)
