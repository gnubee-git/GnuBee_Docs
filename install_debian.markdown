---
layout: default
title: Install Debian
---

## Debian

The Debian installation is documented [here](install_debian.html) as several points have to be considered.
You should be familiar with Debian or be familiar using linux system commands on the command line.


### download and install the firmware supporting Debian

Download **https://github.com/gnubee-git/gnubee-git.github.io/raw/master/debian/openwrt-ramips-mt7621-gb-pc1-squashfs-sysupgrade_2017-03-01.bin** (offical Kernel 3.10.14 from Soc vendor, support install to SD card)

or 

https://github.com/gnubee-git/gnubee-git.github.io/raw/master/debian/librecmc-ramips-mt7621-gb-pc1-squashfs-sysupgrade_2017-11-28.bin (Kernel 4.4.87-gnu, with patches from Linux community, do not support install to SD card, because SD driver only work as a module) 


Note: The firmware named "librecmc-ramips-mt7621-gb-pc1-squashfs-sysupgrade_2017-07-22.bin" has a bug with the network setup and is therefore NOT recommended! [Details](https://github.com/gnubee-git/GnuBee_Docs/issues/18)

Sha256sums:
c2794439b241bd0f71ccfd84f337662b44e3d2b6b6a30593d2d03d8b4136ad  openwrt-ramips-mt7621-gb-pc1-squashfs-sysupgrade_2017-03-01.bin
e2fe4cd4f70dc04372d5a7dcf1443d6895cd3e797a65c1d9ec889d79db181493  librecmc-ramips-mt7621-gb-pc1-squashfs-sysupgrade_2017-11-28.bin


### Upgrade your firmware using the downloaded file

The exact procedure is explained **[here](https://github.com/gnubee-git/GnuBee_Docs/wiki/Install-firmware)**

Reboot by entering the command `reboot`.

### Access the newly installed kernel via SSH or the USB-2-UART

You can either activate the SSH access via built-in webserver on [http://192.168.10.1](http://192.168.10.1) on eth0.1 (bridge-lan), connect the second Ethernet (eth0.2) to a switch and obtain an automatic address in your existing LAN, or use  or via  USB-2-UART, as explained in [USB_to_UART](/USB_to_UART/README.html)

If you want to install Debian on a hard disk yo must format (at least) one partitions with the ext4 file system. Think a while on which partition scheme you want to use.

You can use `fdisk` paritioning tool or partition the HD on another system. For my 1 TB hard disk I decide to create the following partitions using a GPT partition table:

    /dev/sda1 1 GB ext4 named boot
    /dev/sda2 10 GB ext4 named data
    /dev/sda3 rest ext4 named data

Afterward run in the shell run the following commands

     wget --no-check-certificate https://raw.githubusercontent.com/gnubee-git/GnuBee_Docs/master/GB-PCx/scripts/jessie_3.10.14/debian-jessie-install
     sh debian-jessie-install

You will be asked where (what device) to install debian on. In my case I used /dev/sda2. You find the complete log [here](logs/install_debian.log).

Once  if finishes type `reboot` and the new debian system should come up. You find the boot log [here](logs/boot_debian.log).

If you want to access the new Debian system via ssh, you must connect now the **blue** RJ-45 to your network and consult your DHCP server to see, which IP-address was assigned to it. In my case it was 192.168.0.73 and running `ssh root@192.168.073` got me to the login.

Once Debian is up you may log in as user root with the password GnuBee. You should change it. 

### Install kernel modules

You must install more kernel modules running the following commands

    wget https://raw.githubusercontent.com/gnubee-git/gnubee-git.github.io/master/debian/debian-modules-install
    sh debian-modules-install

### Install more packages

Now you are ready to install more packages. There are quite a few I cannot live without, there I ran

    apt-get update
    apt-get install vim-nox etckeeper htop iotop parted smartmontools
