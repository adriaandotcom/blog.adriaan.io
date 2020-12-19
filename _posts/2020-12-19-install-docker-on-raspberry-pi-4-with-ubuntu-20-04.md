---
title: Install Docker on Raspberry Pi 4 with Ubuntu 20.04
layout: post
---

In the [documentation of Docker](https://docs.docker.com/engine/install/ubuntu/) it says to install the OS version with `lsb_release -cs`. For me this returned `focal`, but Docker does not have the release files for that version it seems.

I got errors like `E: Package 'docker-ce' has no installation candidate`.

Just change `lsb_release -cs` to `bionic` (for `armhf`):

```bash
sudo add-apt-repository \
   "deb [arch=armhf] https://download.docker.com/linux/ubuntu \
   bionic \
   stable"
```

I installed the 32 bit version so then you need `armhf`, but it might be different for you. Just check the command `apt policy` to see what your system is using. It returns something like `... Packages`. Where `...` is your type of installation.

After that you can just install Docker

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
