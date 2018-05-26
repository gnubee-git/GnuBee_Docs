---
layout: default
title: Background information for GnuBee PC 1 and 2
---

## Hardware and software

The GnuBee Personal Cloud One and Two come pre-installed with LibreCMC. While this operating system provides basic functionality, there are some problems with this copy of LibreCMC, such as not being able to install additional packages. Due to limited time, LibreCMC is also not being actively supported for the device. Instead we strongly recommend installing either OpenMediaVault or Debian.

Choose OpenMediaVault if you would simply like a fully-functioning NAS device for end users. Keep in mind that the configuration interface is still quite limited.

Choose Debian if you would prefer to tinker with the device and configure your own software.

While OpenWRT (now merged with the LEDE project) is not yet recommended, the upcoming release of OpenWRT 18.06 (June) is expected to provide significantly improved support and is likely to become the recommended option for end users.

Assuming that you have GnuBee PC1 without any upgraded firmware nor installed linux distribution you have two choices using the built-in webserver or using a USB-stick.

The GnuBee PC 1 has a 32MB soldered on Flash-chip. It contains 6 sections. Their names (u-boot,
u-boot-env, factory,firmware,kernel,rootfs,rootfs_data) should be self-explanatory.

**Upgrading the firmware** will replace the content of the firmware, kernel, rootfs and rootfs_data sections.

Besides the 6 interfaces for 2,5 SATA drives there is a slot for micro SD-cards (upto 64 GB tested).

All the firmware discussed here, have the following common characteristics:

* They mount a read/write overlay partition, where changes are preserverd over reboots.  This allows you to add more packages, download scripts, change password, etc.
* By default there is no root password.
* After the startup you may access the device
  * either with a USB-to-UART cable or
  * via a WebServer on 192.168.10.1 using the **black** ethernet RJ-45 connector

Once you have installed the firmware for Debian however, this pictures changes. So do not forget to read [this section](#debian_background).

### Using [https://lede-project.org/](LEDE) or [libreCMC](https://www.librecmc.org/)

Wikpedia characterizes them as follows:

* [LEDE](https://en.wikipedia.org/wiki/LEDE) The Linux Embedded Development Environment (LEDE) project is founded as a fork of the OpenWrt project and shares many of the same goals. It was created in May 2016 by a group of core OpenWrt contributors due to disagreements on OpenWrt internal processes.[5] The schism was nominally reconciled a year later in May 2017 pending approval of the LEDE developers. The proposed remerger will preserve the OpenWrt branding but use many of the LEDE processes and rules. The remerge proposal vote was passed by LEDE developers in June 2017.

* [LibreCMC](https://en.wikipedia.org/wiki/LibreCMC) LibreCMC is a GNU/Linux-libre distribution for computers with minimal resources, such as the Ben Nanonote, ath9k-based Wi-Fi routers, and other hardware with emphasis on free software. The project's current goal is to aim for compliance with the GNU Free System Distribution Guidelines (GNU FSDG) and ensure that the project continues to meet these requirements set forth by the Free Software Foundation (FSF).

Once you have installed/upgraded the firmware please look at the documentation of each distribution, as there are not many GnuBee specialities. And their built-in webserver for administration is easy to use.

You can use the GnuBee PC1 as a router/firewall even without adding an SD-card or hard disk.

### Using Debian or OpenMediaVault<a name="debian_background"></a>

Once you have installed the Debian specific firmware, the default network setup changes:

* Uses the **blue** ethernet RJ-45 connector and gets IP address via DHCP
* U-Boot loads a LEDE based kernel, which looks for EXT4 formatted partition(s) labelled `GNUBEE-ROOT`
* Assumes that this partition contains a valid Debian system
* Pivots (Replaces the early / by the first  `GNUBEE-ROOT` labelled EXT4 partition)
* If no `GNUBEE-ROOT` found (eg. because you removed the disk drives/SD card), else
** boots into a rescue kernel, where you can
** access to a lot of tools (fdisk, fsck.ext4, tune2fs, ifconfig, etc)
** currently (Sept 2017) you are unable to install additional packages using opkg

### debugging the bootstrap processes

Use the USB-2-UART as explained in [USB_to_UART](/USB_to_UART/README.html)
