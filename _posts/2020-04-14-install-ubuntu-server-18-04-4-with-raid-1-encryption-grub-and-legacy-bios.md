---
title: Install Ubuntu Server 18.04.4 on encrypted disks with RAID 1, GRUB, and legacy BIOS
---

In this guide I explain how to install Ubuntu Server 18.04.4 on a (bare metal) server with two disk in RAID 1 mode. You will **loose all data** on your server if you follow this guide. I will use a **full disk encryption** with [md-crypt](https://en.wikipedia.org/wiki/Dm-crypt). My hosting provider does not support EUFI so I used **legacy BIOS** to run the server. This means you can't use disks larger than x TB. We will not use LUKS as it's another layer of complexity to the server.

> Disclaimer: I'm not a server expert. I installed a few bare metal servers in my life. The biggest reason for this guide is to use it myself for my next server install. I couldn't find a good guide on installing Ubuntu Server with encrypted disks in RAID mode so I grabbed information from all around the internet. Please be careful when using this guide yourself.

Before you start it's wise to check if our requirements align. In this guide we encrypt the disks as much as possible. This means that **you need to enter a password on boot**. You can use tools like [Mandos](https://www.recompile.se/mandos), a system for allowing servers with encrypted root file systems to reboot unattended and/or remotely. I prefer Dropbear, a very small SSH program, that you can run via the initial ramdisk (initramfs). This means we are able to allow external connections via SSH before you need to enter your encryption password. By following this guide you will **delete all data** on your server. We also use legacy BIOS, change the steps I took (eg. BIOS boot partition) for legacy BIOS into something else if needed.

## Download the correct ISO image

To install a new OS on a (bare metal) server you need to mount an image to the server. Most providers allow you to mount an image via their customer portal or a [IPMI](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface) (Intelligent Platform Management Interface) such as [DRAC](https://en.wikipedia.org/wiki/Dell_DRAC) for Dell, [iLO](https://en.wikipedia.org/wiki/HP_Integrated_Lights-Out) for HP.

Once you have access to your server via that interface you can mount an image to it. If you install Ubuntu Server you'll need the cdimage (not the live version). Sometimes this installer is called the "Alternative Ubuntu Server installer" or "Alternative installer".

Pick a version from [http://cdimage.ubuntu.com/releases/](http://cdimage.ubuntu.com/releases/). At the time of writing I picked `http://cdimage.ubuntu.com/releases/18.04/release/ubuntu-18.04.4-server-amd64.iso`. Link the ISO to your server via the control panel or the IPMI.

## Mount the ISO image to your server

In my example I'm using the iLO Integrated Remote Console. This looks likely different for you depending on your hosting provider, but you get the gist:

<img loading="lazy" src="/images/posts/install-ubuntu/005-select-image-file.jpg" alt="Select the ISO image in our IPMI" class="shadow" />

Select the ISO you downloaded as Image File for CD/DVD-ROM:

<img loading="lazy" src="/images/posts/install-ubuntu/010-finder-image-file.jpg" alt="Select the ISO image from your desktop" class="shadow" />

### Legacy BIOS

I had to change the boot method to legacy BIOS because my hosting provider didn't support EUFI at the time. Note that other steps in this guide like partitioning is written for Legacy BIOS, not EUFI. You can do this on boot when you hit one of the keyboard keys to enter boot setup. In case of this screen it would have been F9:

<img loading="lazy" src="/images/posts/install-ubuntu/015-select-boot-menu.jpg" alt="Start boot menu" class="shadow" />

## Boot from ISO/CD

Make sure you boot from the CD. The ways you can boot into your machine is different for some manufacturers. In my case I hit F11 for the boot menu:

<img loading="lazy" src="/images/posts/install-ubuntu/015-select-boot-menu.jpg" alt="Start boot menu" class="shadow" />

Then I get a option to boot from the CD. In my case I need to press 1:

<img loading="lazy" src="/images/posts/install-ubuntu/020-boot-menu.jpg" alt="Boot menu" class="shadow" />

It starts booting from the CD, yea!

<img loading="lazy" src="/images/posts/install-ubuntu/025-booting-from-cd.jpg" alt="Booting from CD" class="shadow" />

## Install Ubuntu via the installer

Select your language and hit Enter:

<img loading="lazy" src="/images/posts/install-ubuntu/030-installer-select-language.jpg" alt="Ubuntu Server Installer select language" class="shadow" />

Select "Install Ubuntu Server" by hitting Enter:

<img loading="lazy" src="/images/posts/install-ubuntu/035-installer-start.jpg" alt="Ubuntu Server Installer home screen" class="shadow" />

In the next few screens you can select your language, keyword, and time zone. I omitted these screens because you probably have seen them already a few times. Just follow your gut. After these settings are done you see the installer loading:

<img loading="lazy" src="/images/posts/install-ubuntu/040-installer-loading.jpg" alt="Ubuntu Server Installer is loading files from disk" class="shadow" />

After loading it asked me to configure the network. You will need a get the IP address from your hosting provider. It's probably in the written somewhere in the dashboard. You might add an CIDR netmask like /24 to it. Ask your hosting provider or just try it without:

<img loading="lazy" src="/images/posts/install-ubuntu/045-installer-enter-ip.jpg" alt="Ubuntu Server Installer configure the network" class="shadow" />

Enter the gateway of your network, this is something your provider should offer you:

<img loading="lazy" src="/images/posts/install-ubuntu/050-installer-enter-gateway.jpg" alt="Ubuntu Server Installer enter gateway" class="shadow" />

Enter a name server. If you don't know what to use or your hosting provider doesn't have any you could use a public one like the privacy friendly [one from CloudFlare](https://www.cloudflare.com/learning/dns/what-is-1.1.1.1/): `1.1.1.1 1.0.0.1` (separated by a space, not comma!) If you need to connect to local servers on the local network, it's probably better to use the name servers of your hosting provider.

<img loading="lazy" src="/images/posts/install-ubuntu/055-installer-enter-name-server.jpg" alt="Ubuntu Server Installer enter name server" class="shadow" />

Enter a hostname. This is the name of your computer. You can change it later, but it's a bit cumbersome, so better pick a good name now:

<img loading="lazy" src="/images/posts/install-ubuntu/060-installer-enter-hostname.jpg" alt="Ubuntu Server Installer enter hostname" class="shadow" />

If your network requires a domain, you can enter it here (I usually leave it blank):

<img loading="lazy" src="/images/posts/install-ubuntu/065-installer-enter-domain.jpg" alt="Ubuntu Server Installer enter domain" class="shadow" />

Enter a name for the user/server. This is likely the name where you remotely login with.

<img loading="lazy" src="/images/posts/install-ubuntu/070-installer-enter-enter-name.jpg" alt="Ubuntu Server Installer enter name" class="shadow" />

Enter a user name (the current input is based on what you filled in in the previous step):

<img loading="lazy" src="/images/posts/install-ubuntu/075-installer-enter-enter-username.jpg" alt="Ubuntu Server Installer enter username" class="shadow" />

Enter a password, you can always change it later:

<img loading="lazy" src="/images/posts/install-ubuntu/080-installer-enter-enter-password.jpg" alt="Ubuntu Server Installer enter password" class="shadow" />

Just reenter the same password:

<img loading="lazy" src="/images/posts/install-ubuntu/085-installer-enter-enter-reenter-password.jpg" alt="Ubuntu Server Installer reenter password" class="shadow" />

It can happen that the installer ask you to unmount partitions. This does not always happen. Unmount those partitions otherwise we can't remove the existing partitions:

<img loading="lazy" src="/images/posts/install-ubuntu/090-installer-enter-unmount-partitions.jpg" alt="Ubuntu Server Installer unmount partitions" class="shadow" />

This screen can look a bit different based on this installation that you already have on your drives. Select "Manual" as partition method:

<img loading="lazy" src="/images/posts/install-ubuntu/095-installer-enter-select-partition-manual.jpg" alt="Ubuntu Server Installer select manual partition method" class="shadow" />

When you start with two disks you see them in the overview of partitions:

<img loading="lazy" src="/images/posts/install-ubuntu/100-installer-partition-start.jpg" alt="Ubuntu Server Installer select manual partition method" class="shadow" />

It could happen that the partitions do not show and when you select one of the disks it will as to create a new partition table on the drive. You can select "Yes":

<img loading="lazy" src="/images/posts/install-ubuntu/105-installer-partition-table.jpg" alt="Ubuntu Server Installer partition table" class="shadow" />

Hit the "FREE SPACE" and select "Create a new partition".

<img loading="lazy" src="/images/posts/install-ubuntu/110-installer-partition-create-new.jpg" alt="Ubuntu Server Installer partition create new" class="shadow" />

It will ask you for the size of the partition. As this is a partition used for the GRUB boot loader it can be very tiny.

I didn't figure out how to use the GRUB boot loader with a partition in RAID mode. So I followed [this advise](https://askubuntu.com/questions/43036/how-do-i-install-grub-on-a-raid-system-installation) and created on all disks an 1MB partition for the GRUB boot loader.

> According to the [BIOS/GPT notes](https://help.ubuntu.com/community/Grub2/Installing#BIOS.2FGPT_Notes) if the BIOS is setup to boot the disk in Legacy/mbr mode, installing GRUB2 on a GPT (GUID Partition Table) disk requires a dedicated BIOS boot partition with a recommended size of at least 1 MiB. This partition can be created via GParted or other partitioning tools, or via the command line. It must be identified with a bios_grub flag. The necessary GPT modules are automatically included during installation when GRUB 2 detects a GPT scheme - [source](https://help.ubuntu.com/community/Grub2/Installing#BIOS.2FGPT_Notes).

In the next screen select "Use as" and hit Enter. You will get a list of types on how to use this partition. Select "Reserved BIOS boot area" and hit Enter:

<img loading="lazy" src="/images/posts/install-ubuntu/115-installer-partition-use-as.jpg" alt="Ubuntu Server Installer partition use as" class="shadow" />

You can give the partition a name like "bios" or something. It's not required. For me the bootable flag was impossible to change, so I kept it to "off". It worked fine for me. Hit "Done setting up the partition".

<img loading="lazy" src="/images/posts/install-ubuntu/120-installer-partition-use-as-done.jpg" alt="Ubuntu Server Installer partition done setting up" class="shadow" />

Also add this partition to the other drives. Once you are done add another partition to both drives. One partition with 1GB of data used as "physical volume for RAID" and one with the rest of data also as "physical volume for RAID".

After these settings it will look more or less like this:

<img loading="lazy" src="/images/posts/install-ubuntu/125-installer-partition-overview.jpg" alt="Ubuntu Server Installer partition done setting up" class="shadow" />

Select "Configure software RAID":

<img loading="lazy" src="/images/posts/install-ubuntu/130-installer-partition-configure-software-raid.jpg" alt="Ubuntu Server Installer partition done setting up" class="shadow" />

Write the changes to the storage devices and configure RAID by selecting "Yes":

<img loading="lazy" src="/images/posts/install-ubuntu/135-installer-partition-overwrite.jpg" alt="Ubuntu Server installer partition overwrite" class="shadow" />

Select "Create MD device":

<img loading="lazy" src="/images/posts/install-ubuntu/140-installer-partition-select-create-md.jpg" alt="Ubuntu Server installer partition select create md" class="shadow" />

Select "RAID1":

<img loading="lazy" src="/images/posts/install-ubuntu/145-installer-partition-select-raid-1.jpg" alt="Ubuntu Server installer partition select raid 1" class="shadow" />

Enter the amount of active devices for the RAID array, for most people that's 2:

<img loading="lazy" src="/images/posts/install-ubuntu/150-installer-partition-set-2-devices-in-array.jpg" alt="Ubuntu Server installer partition set 2 devices in array" class="shadow" />

Enter the amount of spare devices for the RAID array, for most people that's 0:

<img loading="lazy" src="/images/posts/install-ubuntu/155-installer-partition-set-0-spare-devices.jpg" alt="Ubuntu Server installer partition set 0 spare devices" class="shadow" />

Select the more or less 1GB devices with your spacebar and hit enter when both are selected:

<img loading="lazy" src="/images/posts/install-ubuntu/160-installer-partition-select-1gb-partitions.jpg" alt="Ubuntu Server installer partition select 1gb partitions" class="shadow" />

Confirm by selecting "Yes":

<img loading="lazy" src="/images/posts/install-ubuntu/165-installer-partition-confirm.jpg" alt="Ubuntu Server installer partition confirm" class="shadow" />

Do the same for the other partition (the biggest one), leave the 1MB for what it is:

<img loading="lazy" src="/images/posts/install-ubuntu/170-installer-partition-select-rest-partitions.jpg" alt="Ubuntu Server installer partition select rest partitions" class="shadow" />

Select "Finish":

<img loading="lazy" src="/images/posts/install-ubuntu/175-installer-partition-finish.jpg" alt="Ubuntu Server installer partition finish" class="shadow" />

Select the first RAID device with 1GB:

<img loading="lazy" src="/images/posts/install-ubuntu/180-installer-partition-select-1gb-raid-device.jpg" alt="Ubuntu Server installer partition select 1gb raid device" class="shadow" />

Navigate to "Use as" and select it:

<img loading="lazy" src="/images/posts/install-ubuntu/185-installer-partition-select-use-as.jpg" alt="Ubuntu Server installer partition-select use as" class="shadow" />

Select "Ext4 journaling file system" (it's not super important which one you select, but Ext4 is common):

<img loading="lazy" src="/images/posts/install-ubuntu/190-installer-partition-select-ext4.jpg" alt="Ubuntu Server installer partition select ext4" class="shadow" />

Navigate to "Mount point":

<img loading="lazy" src="/images/posts/install-ubuntu/195-installer-partition-select-mount-point.jpg" alt="Ubuntu Server installer partition select mount point" class="shadow" />

Set "Mount point" to "/boot - static files of the boot loader":

<img loading="lazy" src="/images/posts/install-ubuntu/200-installer-partition-set-mount-point-to-boot.jpg" alt="Ubuntu Server installer partition set mount point to boot" class="shadow" />

Select "Done setting up the partition":

<img loading="lazy" src="/images/posts/install-ubuntu/205-installer-partition-done.jpg" alt="Ubuntu Server installer partition done" class="shadow" />

Go to the other RAID device (the big one):

<img loading="lazy" src="/images/posts/install-ubuntu/210-installer-partition-select-big-raid-device.jpg" alt="Ubuntu Server installer partition- elect big raid device" class="shadow" />

Navigate to "Use as" and select it:

<img loading="lazy" src="/images/posts/install-ubuntu/215-installer-partition-select-use-as.jpg" alt="Ubuntu Server installer partition select use as" class="shadow" />

Select "physical volume for encryption (if you want encryption):

<img loading="lazy" src="/images/posts/install-ubuntu/220-installer-partition-set-to-physical-volume-for-encryption.jpg" alt="Ubuntu Server installer partition set to physical- olume for encryption" class="shadow" />

Select "Done setting up the partition":

<img loading="lazy" src="/images/posts/install-ubuntu/225-installer-partition-done.jpg" alt="Ubuntu Server installer partition done" class="shadow" />

Select "Configure encrypted volumes":

<img loading="lazy" src="/images/posts/install-ubuntu/230-installer-partition-select-configure-encrypted-volumes.jpg" alt="Ubuntu Server installer partition select configure encrypted volumes" class="shadow" />

Confirm by selecting "Yes" (this can not be undone, if you need to make changes after you have to restart your installer I believe):

<img loading="lazy" src="/images/posts/install-ubuntu/235-installer-partition-confirm.jpg" alt="Ubuntu Server installer partition confirm" class="shadow" />

Select "Create encrypted volumes":

<img loading="lazy" src="/images/posts/install-ubuntu/240-installer-partition-create-encrypte-volume.jpg" alt="Ubuntu Server installer partition create encrypte volume" class="shadow" />

Select the big crypto volume with spacebar and hit Enter:

<img loading="lazy" src="/images/posts/install-ubuntu/245-installer-partition-select-big-crypto-volume.jpg" alt="Ubuntu Server installer partition select big crypto volume" class="shadow" />

Confirm by selecting "Finish":

<img loading="lazy" src="/images/posts/install-ubuntu/250-installer-partition-finish.jpg" alt="Ubuntu Server installer partition finish" class="shadow" />

Now type a password for your partition. This password is something you need to type every time your server restarts. You can do this remotely if you install something like [Dropbear](https://matt.ucc.asn.au/dropbear/dropbear.html) as a SSH client. If you do not need to enter the key on restart the whole encryption does not make much sense.

Enter a key that is long. You can't change this key without a clean install so please pick a good long key:

<img loading="lazy" src="/images/posts/install-ubuntu/255-installer-partition-enter-password.jpg" alt="Ubuntu Server installer partition enter password" class="shadow" />

Reenter the key (in this case this is very good UI, you really want to know you typed the correct key twice):

<img loading="lazy" src="/images/posts/install-ubuntu/260-installer-partition-reenter-password.jpg" alt="Ubuntu Server installer partition reenter password" class="shadow" />

Select the "Encrypted volume" that has been created:

<img loading="lazy" src="/images/posts/install-ubuntu/265-installer-partition-select-encrypted-volume.jpg" alt="Ubuntu Server installer partition select encrypted volume" class="shadow" />

Select "Mount point" again:

<img loading="lazy" src="/images/posts/install-ubuntu/270-installer-partition-select-mount.jpg" alt="Ubuntu Server installer partition select mount" class="shadow" />

Set the "Mount point" this time to "/ - the root file system" (this is where all your files live):

<img loading="lazy" src="/images/posts/install-ubuntu/275-installer-partition-set-mount-to-root.jpg" alt="Ubuntu Server installer partition set mount to root" class="shadow" />

Select "Done setting up the partition":

<img loading="lazy" src="/images/posts/install-ubuntu/280-installer-partition-done.jpg" alt="Ubuntu Server installer partition done" class="shadow" />

Finish by selecting "Finish partitioning and write changes to disk":

<img loading="lazy" src="/images/posts/install-ubuntu/285-installer-partition-finish-partitioning.jpg" alt="Ubuntu Server installer partition finish partitioning" class="shadow" />

Confirm by selecting "Yes":

<img loading="lazy" src="/images/posts/install-ubuntu/290-installer-partition-confirm.jpg" alt="Ubuntu Server installer partition confirm" class="shadow" />

The partitions are created now:

<img loading="lazy" src="/images/posts/install-ubuntu/295-installer-partition-progress.jpg" alt="Ubuntu Server installer partition progress" class="shadow" />

It then will start installing the Ubuntu Server to your machine:

<img loading="lazy" src="/images/posts/install-ubuntu/300-installer-installing.jpg" alt="Ubuntu Server installer installing" class="shadow" />

It could ask you for a proxy for the package manager (leave this blank if you have no idea what this is):

<img loading="lazy" src="/images/posts/install-ubuntu/305-installer-proxy-question.jpg" alt="Ubuntu Server installer proxy question" class="shadow" />

It asks you about automated updates. I normally select "Install security updates automatically":

<img loading="lazy" src="/images/posts/install-ubuntu/310-installer-prompt-security-updates.jpg" alt="Ubuntu Server installer prompt security updates" class="shadow" />

It then asks you about additional software to install. I only select OpenSSH server at this point so I can SSH into the machine once it's booted. Pick whatever your want there:

<img loading="lazy" src="/images/posts/install-ubuntu/315-installer-select-openssh-server.jpg" alt="Ubuntu Server installer select openssh server" class="shadow" />

You can see that the GRUB boot loader is being installed on both disks:

<img loading="lazy" src="/images/posts/install-ubuntu/320-installer-grub-installing-to-both-disks.jpg" alt="Ubuntu Server installer grub installing to both disks" class="shadow" />

At the end of the installation make sure to remove the ISO image you attached to your server earlier:

<img loading="lazy" src="/images/posts/install-ubuntu/325-installer-remove-attached-iso-image.jpg" alt="Ubuntu Server installer remove-attached iso image" class="shadow" />

Once your image is unmounted/removed you can select "Continue" to finish the installation and reboot the machine:

<img loading="lazy" src="/images/posts/install-ubuntu/330-installer-hit-reboot.jpg" alt="Ubuntu Server installer hit reboot" class="shadow" />

It will show a brief message that it's rebooting (it could look different on your machine):

<img loading="lazy" src="/images/posts/install-ubuntu/340-installer-rebooting.jpg" alt="Ubuntu Server installer rebooting" class="shadow" />

It's reset and now starting again:

<img loading="lazy" src="/images/posts/install-ubuntu/350-booting.jpg" alt="Ubuntu Server booting" class="shadow" />

When you see this prompt "Please unlock disk md1_crypt" you can enter your encryption password and finally enter your machine:

<img loading="lazy" src="/images/posts/install-ubuntu/355-unlock.jpg" alt="Ubuntu Server unlock" class="shadow" />

Wow, you did it! I bet this was a very long process. I hope everything went well and if you run into issues, please use [askubuntu.com](https://askubuntu.com/) to figure out what is going wrong. I also found the search engine on [help.ubuntu.com](https://help.ubuntu.com/) very helpful.

I created this guide by recording my screen while installing the server. When done I took screenshots of every important screen and cropped them to remove the noise around it. It took me almost a full day, but I'm happy to make encryption a bit easier for the world. Let me know if there are errors in this guide, happy to fix those.

## Read more

I used the resources below to install my server:

- [help.ubuntu.com/…/advanced-installation.html](https://help.ubuntu.com/lts/serverguide/advanced-installation.html)
- [askubuntu.com/…/unable-to-install-grub-in-dev-sda-when-installing-grub](https://askubuntu.com/questions/459620/unable-to-install-grub-in-dev-sda-when-installing-grub)
- [askubuntu.com/…/how-do-i-install-grub-on-a-raid-system-installation](https://askubuntu.com/questions/43036/how-do-i-install-grub-on-a-raid-system-installation)
- [help.ubuntu.com/search.html](https://help.ubuntu.com/search.html)
- [askubuntu.com/…/grub-install-fails-on-fresh-18-04-alternate-server-raid-1-installation](https://askubuntu.com/questions/1032766/grub-install-fails-on-fresh-18-04-alternate-server-raid-1-installation)
- [askubuntu.com/…/how-do-i-install-grub-on-a-raid-system-installation](https://askubuntu.com/questions/43036/how-do-i-install-grub-on-a-raid-system-installation)
- [askubuntu.com/…/install-ubuntu-18-04-desktop-with-raid-1-and-lvm-on-machine-with-uefi-bios](https://askubuntu.com/questions/1066028/install-ubuntu-18-04-desktop-with-raid-1-and-lvm-on-machine-with-uefi-bios) (UEFI)
- [askubuntu.com/…/why-encrypt-the-swap-partition](https://askubuntu.com/questions/313564/why-encrypt-the-swap-partition)
- [askubuntu.com/…/is-swap-area-required-can-we-install-ubuntu-without-a-swap-area](https://askubuntu.com/questions/732274/is-swap-area-required-can-we-install-ubuntu-without-a-swap-area)
- [serverfault.com/…/is-it-possible-and-wise-to-put-the-grub-bios-partition-on-a-software-raid](https://serverfault.com/questions/749274/is-it-possible-and-wise-to-put-the-grub-bios-partition-on-a-software-raid)
- [unix.stackexchange.com/…/booting-off-raided-2gb-drives-btrfs-handling-bios-boot-partition](https://unix.stackexchange.com/questions/217268/booting-off-raided-2gb-drives-btrfs-handling-bios-boot-partition/218384#218384)
- [ubuntu.com/server/docs/installation-advanced](https://ubuntu.com/server/docs/installation-advanced)
- [askubuntu.com/…/ubuntu-server-16-04-1-software-raid10-44tb-boot-issues](https://askubuntu.com/questions/836010/ubuntu-server-16-04-1-software-raid10-44tb-boot-issues)
- [askubuntu.com/…/what-filesystem-should-boot-be](https://askubuntu.com/questions/495994/what-filesystem-should-boot-be)
- [servethehome.com/installing-ubuntu-server-software-raid-1](https://www.servethehome.com/installing-ubuntu-server-software-raid-1/)
- [smarthomebeginner.com/ubuntu-server-partition-scheme-guide](https://www.smarthomebeginner.com/ubuntu-server-partition-scheme-guide/)

Thanks for reading, I hope it did help you a bit.
