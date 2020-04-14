---
title: Install Ubuntu Server 18.04.4 on encrypted with RAID 1, GRUB, and legacy BIOS
---

In this guide I explain how to install Ubuntu Server 18.04.4 on a bare metal server with **two discs**. We will use a **full disc encryption** with [md-crypt](https://en.wikipedia.org/wiki/Dm-crypt). I will put the discs in software RAID 1. My hosting provider does not support EUFI so I used **legacy BIOS** to run the server. This means you can't use discs larger than x TB. We will not use LUKS as it's another layer of complexity to the server.

## Mount the right ISO image to your server

To install a new OS on a (bare metal) server you need to mount an image to the server. Most providers allow you to mount an image via their customer portal or a [IPMI](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface) (Intelligent Platform Management Interface) such as ([DRAC](https://en.wikipedia.org/wiki/Dell_DRAC) for Dell, [iLO](https://en.wikipedia.org/wiki/HP_Integrated_Lights-Out) for HP.

Once you have access to your server via that interface you can mount an image to it. If you install Ubuntu Server you'll need the cdimage (not the live version). Sometimes this installer is called the "Alternative Ubuntu Server installer" or "Alternative installer".

Pick a version from [http://cdimage.ubuntu.com/releases/](http://cdimage.ubuntu.com/releases/). At the time of writing I picked `http://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.4-server-amd64.iso`. Link the .iso to your server via the control panel or the IPMI.

### Follow these steps to

### Using the GRUB loader with RAID 1

According to the [BIOS/GPT notes](https://help.ubuntu.com/community/Grub2/Installing#BIOS.2FGPT_Notes) if the BIOS is setup to boot the disk in Legacy/mbr mode, installing GRUB2 on a GPT (GUID Partition Table) disk requires a dedicated BIOS boot partition with a recommended size of at least 1 MiB. This partition can be created via GParted or other partitioning tools, or via the command line. It must be identified with a bios_grub flag. The necessary GPT modules are automatically included during installation when GRUB 2 detects a GPT scheme.

Read more:

- https://help.ubuntu.com/lts/serverguide/advanced-installation.html
- https://askubuntu.com/questions/459620/unable-to-install-grub-in-dev-sda-when-installing-grub
- https://askubuntu.com/questions/43036/how-do-i-install-grub-on-a-raid-system-installation
- https://help.ubuntu.com/search.html
- https://askubuntu.com/questions/1032766/grub-install-fails-on-fresh-18-04-alternate-server-raid-1-installation
- https://askubuntu.com/questions/43036/how-do-i-install-grub-on-a-raid-system-installation
- https://askubuntu.com/questions/1066028/install-ubuntu-18-04-desktop-with-raid-1-and-lvm-on-machine-with-uefi-bios (UEFI)
- https://askubuntu.com/questions/313564/why-encrypt-the-swap-partition
- https://askubuntu.com/questions/732274/is-swap-area-required-can-we-install-ubuntu-without-a-swap-area
- https://serverfault.com/questions/749274/is-it-possible-and-wise-to-put-the-grub-bios-partition-on-a-software-raid
- https://unix.stackexchange.com/questions/217268/booting-off-raided-2gb-drives-btrfs-handling-bios-boot-partition/218384#218384
- https://ubuntu.com/server/docs/installation-advanced
- https://askubuntu.com/questions/836010/ubuntu-server-16-04-1-software-raid10-44tb-boot-issues
- https://askubuntu.com/questions/495994/what-filesystem-should-boot-be

- https://www.tecmint.com/set-permanent-dns-nameservers-in-ubuntu-debian/
