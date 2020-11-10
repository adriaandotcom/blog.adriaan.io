---
title: How to copy Let's Encrypt account including all certificates to a new server 
layout: post
---

You can copy the entire dir /etc/letsencrypt/ and restore it on your new server.

1. Make sure to be logged in to your old server

1. Run `cd ~/ && sudo tar zpcvf 2020-11-10-letsencrypt-backup.tar.gz /etc/letsencrypt/`

1. Copy this file (`2020-11-10-letsencrypt-backup.tar.gz`) from the home directory in your old server to your new server.

   In short run `cd ~/ && scp 2020-11-10-letsencrypt-backup.tar.gz user@100.150.10.200:/home/app/`.

   If you don't know how to use `scp` read further. Make sure you have an ssh key in `~/.ssh/id_rsa.pub`, if this file is not present, create one with `ssh-keygen`. After this command you should have a key in `~/.ssh/id_rsa.pub`. Add this key (of your old server) to `~/.ssh/authorized_keys` in your new server. Then restart ssh with `sudo service sshd restart`. After this you can copy files like this: `cd ~/ && scp 2020-11-10-letsencrypt-backup.tar.gz user@100.150.10.200:/home/app/` where `100.150.10.200` is the IP of your new server and `user` the ssh user.

1. On the new server you can decompress the backup file: `sudo tar zxvf 2020-11-10-letsencrypt-backup.tar.gz -C /`

And you have all the certificates, renewal confs, etc. on your new server.

And will the new installation know how to update the files?

Certbot will use the information saved on renewal conf files `/etc/letsencrypt/renewal/*` so if the paths to your webroot etc. are the same, you should have no issues, if the paths have changed then you should modify them on the renewal conf files for all your domains, but well all this depends on how you issued your certificates (using certonly, webroot, apache plugin, nginx plugin, etc.)

Thanks to [sahsanu on community.letsencrypt.org](https://community.letsencrypt.org/t/move-to-another-server/77985/6) for providing most of this information.
