Orangepi Zero Plus2
###################

This tutorial will show the details of Orangepi Zero Plus2 board mainline support and other needed details, for more information about `hardware <https://linux-sunxi.org/Xunlong_Orange_Pi_Zero_Plus_2>`_ and `linux-sunxi <https://linux-sunxi.org/Xunlong_Orange_Pi_Zero_Plus_2>`_

Hardware Access
***************
.. image:: /images/opi_zero_plus2.jpeg

Serial debug and Power connections

BSP Build
*********
Manual Build
============
Image building need host to ready with all necessary tools ready, refer here

Below are the details of Image build for Orangepi Zero Plus2 board.

ATF
---
::

        $ git clone https://github.com/apritzel/arm-trusted-firmware.git
        $ cd arm-trusted-firmware
        $ make PLAT=sun50iw1p1 DEBUG=1 bl31
        $ export BL31=/path/to/arm-trusted-firmware/build/sun50iw1p1/release/bl31.bin
        
U-Boot
------
::

        $ git clone git://git.denx.de/u-boot.git
        $ cd u-boot
        $ make orangepi_zero_plus2_defconfig
        $ make

Linux
-----
::

        $ git clone git://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git
        $ cd linux-next
        $ make mrproper
        $ ARCH=arm64 make defconfig
        $ ARCH=arm64 make -j 4 Image dtbs

Buildroot
=========
It's easy to build entire system using buildroot and mainline supported orangepi zero plus2 already. See read this `readme.txt <https://git.buildroot.net/buildroot/tree/board/orangepi/orangepi-zero-plus2/readme.txt>`_ for more info.

::

        $ git clone git://git.busybox.net/buildroot
        $ cd buildroot
        $ make orangepi_zero_plus2_defconfig
        $ make

Booting
*******
