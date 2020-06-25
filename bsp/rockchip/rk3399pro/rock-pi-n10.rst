ROCKPI-N10
==========

This tutorial will show the details of Radxa ROCKPI-N10 board with VMARC RK3399pro SOM mainline support.

Hardware details and wiki `ROCKPI-N10 <https://wiki.radxa.com/RockpiN10>`_


Hardware Access
---------------

.. image:: /images/rk3399pro-rock-pi-n10.jpg

BSP Building
------------

Host require few tools/packages to build BSP, install them from `host <https://wiki.amarulasolutions.com/found/host/tools.html#host>`_

ATF
::

        $ git clone https://git.trustedfirmware.org/TF-A/trusted-firmware-a.git
        $ cd /path/to/trusted-firmware-a
        $ make distclean
        $ make CROSS_COMPILE=aarch64-linux-gnu- PLAT=rk3399 bl31

U-Boot
::
        
        $ git clone https://github.com/amarula/u-boot-amarula
        $ cd u-boot-amarula
        $ git checkout -b rock-pi origin/rock-pi
        $ export BL31=/path/to/arm-trusted-firmware/build/rk3399/release/bl31/bl31.elf
        $ make rock-pi-n10-rk3399pro_defconfig
        $ make

Linux
::

        $ git clone git://git.kernel.org/pub/scm/linux/kernel/git/next/linux-next.git
        $ cd linux-next
        $ make mrproper
        $ ARCH=arm64 make defconfig
        $ ARCH=arm64 make Image dtbs -j 4


Buildroot
::

	$ git clone git://git.buildroot.net/buildroot
	$ cd buildroot
	$ make rock_pi_n10_defconfig
	$ make


SD Boot

In the output/images directory, flash sdcard.img on to micro sd

::
	
	$ sudo dd if=output/images/sdcard.img of=/dev/sdX status=progress (X - find it from fdisk -l)
	$ sync

Put this micro-SD card onto your board in the slot and power the board. You should see something like:


::

	U-Boot TPL 2020.07-rc4 (Jun 16 2020 - 19:29:04)
	Channel 0: LPDDR4, 50MHz
	BW=32 Col=10 Bk=8 CS0 Row=15 CS1 Row=15 CS=2 Die BW=16 Size=2048MB
	Channel 1: LPDDR4, 50MHz
	BW=32 Col=10 Bk=8 CS0 Row=15 CS1 Row=15 CS=2 Die BW=16 Size=2048MB
	256B stride
	256B stride
	lpddr4_set_rate: change freq to 400000000 mhz 0, 1
	lpddr4_set_rate: change freq to 800000000 mhz 1, 0
	Trying to boot from BOOTROM
	Returning to boot ROM...

	U-Boot SPL 2020.07-rc4 (Jun 16 2020 - 19:29:04 +0530)
	Trying to boot from MMC1


	U-Boot 2020.07-rc4 (Jun 16 2020 - 19:29:04 +0530)

	SoC: Rockchip rk3399
	Reset cause: POR
	Model: Radxa ROCK Pi N10
	DRAM:  3.9 GiB
	PMIC:  RK808 
	MMC:   mmc@fe310000: 2, mmc@fe320000: 1, sdhci@fe330000: 0
	Loading Environment from MMC... Card did not respond to voltage select!
	*** Warning - No block device, using default environment

	In:    serial
	Out:   serial
	Err:   serial
	Model: Radxa ROCK Pi N10
	Net:   eth0: ethernet@fe300000
	Hit any key to stop autoboot:  0 
	Card did not respond to voltage select!
	switch to partitions #0, OK
	mmc1 is current device
	Scanning mmc 1:3...
	Found /extlinux/extlinux.conf
	Retrieving file: /extlinux/extlinux.conf
	156 bytes read in 5 ms (30.3 KiB/s)
	1:      RK3399_ROCKPI_N10 linux
	Retrieving file: /Image
	26288640 bytes read in 1122 ms (22.3 MiB/s)
	append: earlycon=uart8250,mmio32,0xff1a0000 root=/dev/mmcblk1p4 rw rootwait
	Retrieving file: /rk3399pro-rock-pi-n10.dtb
	54196 bytes read in 7 ms (7.4 MiB/s)
	## Flattened Device Tree blob at 01f00000
	   Booting using the fdt blob at 0x1f00000
	   Loading Device Tree to 00000000f2503000, end 00000000f25133b3 ... OK

	Starting kernel ...

	[    0.000000] Linux version 5.7.2 (suniel@suniel-P5WE0) (gcc version 8.4.0 (Buildroot 2020.08-git-00273-g8f70124), GNU ld (GNU Binutils) 2.32) #2 SMP PREEMPT Thu Ju0
	[    0.000000] Machine model: Radxa ROCK Pi N10
	[    0.000000] earlycon: uart8250 at MMIO32 0x00000000ff1a0000 (options '')
	[    0.000000] printk: bootconsole [uart8250] enabled
	[    0.000000] efi: UEFI not found.
	[    0.000000] cma: Reserved 32 MiB at 0x000000007e000000
	[    0.000000] NUMA: No NUMA configuration found
	[    0.000000] NUMA: Faking a node at [mem 0x0000000000200000-0x000000007fffffff]
	[    0.000000] NUMA: NODE_DATA [mem 0x7dbc5100-0x7dbc6fff]
	[    0.000000] Zone ranges:
	[    0.000000]   DMA      [mem 0x0000000000200000-0x000000003fffffff]
	[    0.000000]   DMA32    [mem 0x0000000040000000-0x000000007fffffff]
	[    0.000000]   Normal   empty
	[    0.000000] Movable zone start for each node
	[    0.000000] Early memory node ranges
	[    0.000000]   node   0: [mem 0x0000000000200000-0x00000000083fffff]
	[    0.000000]   node   0: [mem 0x000000000a200000-0x000000007fffffff]
	[    0.000000] Initmem setup node 0 [mem 0x0000000000200000-0x000000007fffffff]
	[    0.000000] psci: probing for conduit method from DT.
	[    0.000000] psci: PSCIv1.0 detected in firmware.
	[    0.000000] psci: Using standard PSCI v0.2 function IDs
	[    0.000000] psci: Trusted OS migration not required
	[    0.000000] psci: SMC Calling Convention v1.0
	[    0.000000] percpu: Embedded 23 pages/cpu s53784 r8192 d32232 u94208
	[    0.000000] Detected VIPT I-cache on CPU0
	[    0.000000] CPU features: detected: ARM erratum 845719
	[    0.000000] CPU features: detected: GIC system register CPU interface
	[    0.000000] Built 1 zonelists, mobility grouping on.  Total pages: 507912
	[    0.000000] Policy zone: DMA32
	[    0.000000] Kernel command line: earlycon=uart8250,mmio32,0xff1a0000 root=/dev/mmcblk0p4 rw rootwait
	[    0.000000] Dentry cache hash table entries: 262144 (order: 9, 2097152 bytes, linear)
	[    0.000000] Inode-cache hash table entries: 131072 (order: 8, 1048576 bytes, linear)
	[    0.000000] mem auto-init: stack:off, heap alloc:off, heap free:off
	[    0.000000] software IO TLB: mapped [mem 0x3bfff000-0x3ffff000] (64MB)
	[    0.000000] Memory: 1895696K/2064384K available (13692K kernel code, 2088K rwdata, 7332K rodata, 5568K init, 483K bss, 135920K reserved, 32768K cma-reserved)
	[    0.000000] SLUB: HWalign=64, Order=0-3, MinObjects=0, CPUs=6, Nodes=1
	[    0.000000] rcu: Preemptible hierarchical RCU implementation.
	[    0.000000] rcu:     RCU event tracing is enabled.
	[    0.000000] rcu:     RCU restricting CPUs from NR_CPUS=256 to nr_cpu_ids=6.
	[    0.000000]  Tasks RCU enabled.
	[    0.000000] rcu: RCU calculated value of scheduler-enlistment delay is 25 jiffies.
	[    0.000000] rcu: Adjusting geometry for rcu_fanout_leaf=16, nr_cpu_ids=6
	[    0.000000] NR_IRQS: 64, nr_irqs: 64, preallocated irqs: 0
	[    0.000000] GICv3: GIC: Using split EOI/Deactivate mode
	[    0.000000] GICv3: 256 SPIs implemented
	[    0.000000] GICv3: 0 Extended SPIs implemented
	[    0.000000] GICv3: Distributor has no Range Selector support
	[    0.000000] GICv3: 16 PPIs implemented
	[    0.000000] GICv3: CPU0: found redistributor 0 region 0:0x00000000fef00000
	[    0.000000] ITS [mem 0xfee20000-0xfee3ffff]
	[    0.000000] ITS@0x00000000fee20000: allocated 65536 Devices @7d080000 (flat, esz 8, psz 64K, shr 0)
	[    0.000000] ITS: using cache flushing for cmd queue
	[    0.000000] GICv3: using LPI property table @0x000000007d040000
	[    0.000000] GIC: using cache flushing for LPI property table
	[    0.000000] GICv3: CPU0: using allocated LPI pending table @0x000000007d050000
	[    0.000000] GICv3: GIC: PPI partition interrupt-partition-0[0] { /cpus/cpu@0[0] /cpus/cpu@1[1] /cpus/cpu@2[2] /cpus/cpu@3[3] }
	[    0.000000] GICv3: GIC: PPI partition interrupt-partition-1[1] { /cpus/cpu@100[4] /cpus/cpu@101[5] }
	[    0.000000] random: get_random_bytes called from start_kernel+0x314/0x4e4 with crng_init=0
	[    0.000000] arch_timer: cp15 timer(s) running at 24.00MHz (phys).
	[    0.000000] clocksource: arch_sys_counter: mask: 0xffffffffffffff max_cycles: 0x588fe9dc0, max_idle_ns: 440795202592 ns
	[    0.000005] sched_clock: 56 bits at 24MHz, resolution 41ns, wraps every 4398046511097ns
	[    0.002997] Console: colour dummy device 80x25
	[    0.003425] printk: console [tty0] enabled
	[    0.003824] printk: bootconsole [uart8250] disabled
	[    0.004402] Calibrating delay loop (skipped), value calculated using timer frequency.. 48.00 BogoMIPS (lpj=96000)
	[    0.004431] pid_max: default: 32768 minimum: 301
	[    0.004549] LSM: Security Framework initializing
	[    0.004635] Mount-cache hash table entries: 4096 (order: 3, 32768 bytes, linear)
	[    0.004663] Mountpoint-cache hash table entries: 4096 (order: 3, 32768 bytes, linear)
	[    0.006515] rcu: Hierarchical SRCU implementation.
	[    0.006791] Platform MSI: interrupt-controller@fee20000 domain created
	[    0.007179] PCI/MSI: /interrupt-controller@fee00000/interrupt-controller@fee20000 domain created
	[    0.007426] fsl-mc MSI: /interrupt-controller@fee00000/interrupt-controller@fee20000 domain created
	[    0.011708] EFI services will not be available.
	[    0.012060] smp: Bringing up secondary CPUs ...
	[    0.012619] Detected VIPT I-cache on CPU1
	[    0.012653] GICv3: CPU1: found redistributor 1 region 0:0x00000000fef20000
	[    0.012667] GICv3: CPU1: using allocated LPI pending table @0x000000007d060000
	[    0.012712] CPU1: Booted secondary processor 0x0000000001 [0x410fd034]
	[    0.013280] Detected VIPT I-cache on CPU2
	[    0.013304] GICv3: CPU2: found redistributor 2 region 0:0x00000000fef40000
	[    0.013315] GICv3: CPU2: using allocated LPI pending table @0x000000007d070000
	[    0.013343] CPU2: Booted secondary processor 0x0000000002 [0x410fd034]
	[    0.013890] Detected VIPT I-cache on CPU3
	[    0.013912] GICv3: CPU3: found redistributor 3 region 0:0x00000000fef60000
	[    0.013922] GICv3: CPU3: using allocated LPI pending table @0x000000007d100000
	[    0.013948] CPU3: Booted secondary processor 0x0000000003 [0x410fd034]
	[    0.017862] CPU features: detected: EL2 vector hardening
	[    0.018267] ARM_SMCCC_ARCH_WORKAROUND_1 missing from firmware
	[    0.018601] CPU features: detected: ARM erratum 1319367
	[    0.018878] Detected PIPT I-cache on CPU4
	[    0.020121] GICv3: CPU4: found redistributor 100 region 0:0x00000000fef80000
	[    0.020602] GICv3: CPU4: using allocated LPI pending table @0x000000007d110000
	[    0.021850] CPU4: Booted secondary processor 0x0000000100 [0x410fd082]
	[    0.030887] Detected PIPT I-cache on CPU5
	[    0.032074] GICv3: CPU5: found redistributor 101 region 0:0x00000000fefa0000
	[    0.032508] GICv3: CPU5: using allocated LPI pending table @0x000000007d120000
	[    0.033607] CPU5: Booted secondary processor 0x0000000101 [0x410fd082]
	[    0.039150] smp: Brought up 1 node, 6 CPUs
	[    0.039500] SMP: Total of 6 processors activated.
	[    0.039534] CPU features: detected: 32-bit EL0 Support
	[    0.039568] CPU features: detected: CRC32 instructions
	[    0.219105] CPU: All CPU(s) started at EL2
	[    0.219954] alternatives: patching kernel code
	[    0.224388] devtmpfs: initialized
	[    0.236654] KASLR disabled due to lack of seed
	[    0.237771] clocksource: jiffies: mask: 0xffffffff max_cycles: 0xffffffff, max_idle_ns: 7645041785100000 ns
	[    0.237860] futex hash table entries: 2048 (order: 5, 131072 bytes, linear)
	[    0.239760] pinctrl core: initialized pinctrl subsystem
	[    0.242448] thermal_sys: Registered thermal governor 'step_wise'
	[    0.242487] thermal_sys: Registered thermal governor 'power_allocator'
	[    0.243158] DMI not present or invalid.
	[    0.244246] NET: Registered protocol family 16
	[    0.266992] DMA: preallocated 256 KiB pool for atomic allocations
	[    0.267120] audit: initializing netlink subsys (disabled)
	[    0.267566] audit: type=2000 audit(0.264:1): state=initialized audit_enabled=0 res=1
	[    0.270233] cpuidle: using governor menu
	[    0.270853] hw-breakpoint: found 6 breakpoint and 4 watchpoint registers.
	[    0.272760] ASID allocator initialised with 65536 entries
	[    0.275273] Serial: AMBA PL011 UART driver
	[    0.321360] HugeTLB registered 1.00 GiB page size, pre-allocated 0 pages
	[    0.321482] HugeTLB registered 32.0 MiB page size, pre-allocated 0 pages
	[    0.321502] HugeTLB registered 2.00 MiB page size, pre-allocated 0 pages
	[    0.321521] HugeTLB registered 64.0 KiB page size, pre-allocated 0 pages
	[    0.325832] cryptd: max_cpu_qlen set to 1000
	[    0.332792] ACPI: Interpreter disabled.
	[    0.334823] vcc5v0_sys: supplied by vcc12v_dcin
	[    0.337154] iommu: Default domain type: Translated 
	[    0.339159] vgaarb: loaded
	[    0.339668] SCSI subsystem initialized
	[    0.341372] usbcore: registered new interface driver usbfs
	[    0.341429] usbcore: registered new interface driver hub
	[    0.341601] usbcore: registered new device driver usb
	[    0.343009] pps_core: LinuxPPS API ver. 1 registered
	[    0.343034] pps_core: Software ver. 5.3.6 - Copyright 2005-2007 Rodolfo Giometti <giometti@linux.it>
	[    0.343072] PTP clock support registered
	[    0.343252] EDAC MC: Ver: 3.0.0
	[    0.345952] FPGA manager framework
	[    0.346145] Advanced Linux Sound Architecture Driver Initialized.
	[    0.348816] clocksource: Switched to clocksource arch_sys_counter
	[    0.349599] VFS: Disk quotas dquot_6.6.0
	[    0.349691] VFS: Dquot-cache hash table entries: 512 (order 0, 4096 bytes)
	[    0.349977] pnp: PnP ACPI: disabled
	[    0.358168] NET: Registered protocol family 2
	[    0.358927] tcp_listen_portaddr_hash hash table entries: 1024 (order: 2, 16384 bytes, linear)
	[    0.359002] TCP established hash table entries: 16384 (order: 5, 131072 bytes, linear)
	[    0.359205] TCP bind hash table entries: 16384 (order: 6, 262144 bytes, linear)
	[    0.359510] TCP: Hash tables configured (established 16384 bind 16384)
	[    0.359745] UDP hash table entries: 1024 (order: 3, 32768 bytes, linear)
	[    0.359826] UDP-Lite hash table entries: 1024 (order: 3, 32768 bytes, linear)
	[    0.360084] NET: Registered protocol family 1
	[    0.362774] RPC: Registered named UNIX socket transport module.
	[    0.362801] RPC: Registered udp transport module.
	[    0.362816] RPC: Registered tcp transport module.
	[    0.362830] RPC: Registered tcp NFSv4.1 backchannel transport module.
	[    0.362858] PCI: CLS 0 bytes, default 64
	[    0.373854] hw perfevents: enabled with armv8_cortex_a53 PMU driver, 7 counters available
	[    0.376143] hw perfevents: enabled with armv8_cortex_a72 PMU driver, 7 counters available
	[    0.377226] kvm [1]: IPA Size Limit: 40bits
	[    0.378930] kvm [1]: vgic-v2@fff20000
	[    0.379281] kvm [1]: GIC system register CPU interface enabled
	[    0.381652] kvm [1]: vgic interrupt IRQ10
	[    0.383661] kvm [1]: Hyp mode initialized successfully
	[    0.397406] Initialise system trusted keyrings
	[    0.399627] workingset: timestamp_bits=44 max_order=19 bucket_order=0
	[    0.409013] squashfs: version 4.0 (2009/01/31) Phillip Lougher
	[    0.412132] NFS: Registering the id_resolver key type
	[    0.412241] Key type id_resolver registered
	[    0.412256] Key type id_legacy registered
	[    0.412278] nfs4filelayout_init: NFSv4 File Layout Driver Registering...
	[    0.412550] 9p: Installing v9fs 9p2000 file system support
	[    0.432812] Key type asymmetric registered
	[    0.432862] Asymmetric key parser 'x509' registered
	[    0.432985] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 245)
	[    0.433009] io scheduler mq-deadline registered
	[    0.433027] io scheduler kyber registered
	[    0.453203] EINJ: ACPI disabled.
	[    0.461982] dma-pl330 ff6d0000.dma-controller: Loaded driver for PL330 DMAC-241330
	[    0.462059] dma-pl330 ff6d0000.dma-controller:       DBUFF-32x8bytes Num_Chans-6 Num_Peri-12 Num_Events-12
	[    0.464015] dma-pl330 ff6e0000.dma-controller: Loaded driver for PL330 DMAC-241330
	[    0.464052] dma-pl330 ff6e0000.dma-controller:       DBUFF-128x8bytes Num_Chans-8 Num_Peri-20 Num_Events-16
	[    0.477788] Serial: 8250/16550 driver, 4 ports, IRQ sharing enabled
	[    0.481443] ff180000.serial: ttyS0 at MMIO 0xff180000 (irq = 29, base_baud = 1500000) is a 16550A
	[    0.482932] ff1a0000.serial: ttyS2 at MMIO 0xff1a0000 (irq = 30, base_baud = 1500000) is a 16550A
	[    1.702465] printk: console [ttyS2] enabled
	[    1.709482] SuperH (H)SCI(F) driver initialized
	[    1.715422] msm_serial: driver initialized
	[    1.723059] cacheinfo: Unable to detect cache hierarchy for CPU 0
	[    1.743450] loop: module loaded
	[    1.749003] megasas: 07.713.01.00-rc1
	[    1.760492] libphy: Fixed MDIO Bus: probed
	[    1.766153] tun: Universal TUN/TAP device driver, 1.6
	[    1.773692] thunder_xcv, ver 1.0
	[    1.777413] thunder_bgx, ver 1.0
	[    1.781082] nicpf, ver 1.0
	[    1.785704] hclge is initializing
	[    1.789772] hns3: Hisilicon Ethernet Network Driver for Hip08 Family - version
	[    1.797904] hns3: Copyright (c) 2017 Huawei Corporation.
	[    1.803981] e1000: Intel(R) PRO/1000 Network Driver - version 7.3.21-k8-NAPI
	[    1.811889] e1000: Copyright (c) 1999-2006 Intel Corporation.
	[    1.818413] e1000e: Intel(R) PRO/1000 Network Driver - 3.2.6-k
	[    1.824955] e1000e: Copyright(c) 1999 - 2015 Intel Corporation.
	[    1.831632] igb: Intel(R) Gigabit Ethernet Network Driver - version 5.6.0-k
	[    1.839436] igb: Copyright (c) 2007-2014 Intel Corporation.
	[    1.845738] igbvf: Intel(R) Gigabit Virtual Function Network Driver - version 2.4.0-k
	[    1.854514] igbvf: Copyright (c) 2009 - 2012 Intel Corporation.
	[    1.861661] sky2: driver version 1.30
	[    1.867271] VFIO - User Level meta-driver version: 0.3
	[    1.877341] ehci_hcd: USB 2.0 'Enhanced' Host Controller (EHCI) Driver
	[    1.884704] ehci-pci: EHCI PCI platform driver
	[    1.889737] ehci-platform: EHCI generic platform driver
	[    1.895840] ehci-orion: EHCI orion driver
	[    1.900485] ehci-exynos: EHCI Exynos driver
	[    1.905297] ohci_hcd: USB 1.1 'Open' Host Controller (OHCI) Driver
	[    1.912265] ohci-pci: OHCI PCI platform driver
	[    1.917289] ohci-platform: OHCI generic platform driver
	[    1.923309] ohci-exynos: OHCI Exynos driver
	[    1.928803] usbcore: registered new interface driver usb-storage
	[    1.939821] i2c /dev entries driver
	[    1.949562] rk808 0-0020: chip id: 0x8090
	[    1.961877] rk808-regulator rk808-regulator: there is no dvs0 gpio
	[    1.967072] random: fast init done
	[    1.968943] rk808-regulator rk808-regulator: there is no dvs1 gpio
	[    1.979802] DCDC_REG1: supplied by vcc5v0_sys
	[    1.987843] DCDC_REG2: supplied by vcc5v0_sys
	[    1.995219] DCDC_REG3: supplied by vcc5v0_sys
	[    2.001906] DCDC_REG4: supplied by vcc5v0_sys
	[    2.008641] DCDC_REG5: supplied by vcc5v0_sys
	[    2.013848] vcc_buck5: Bringing 3300000uV into 2200000-2200000uV
	[    2.023184] LDO_REG1: supplied by vcc_buck5
	[    2.030387] LDO_REG2: supplied by vcc_buck5
	[    2.037481] LDO_REG3: supplied by vcc_buck5
	[    2.044832] LDO_REG4: supplied by vcc_buck5
	[    2.050032] vcca_1v8: Bringing 1800000uV into 1850000-1850000uV
	[    2.059615] LDO_REG5: supplied by vcc_buck5
	[    2.064821] vdd1v5_dvp: Bringing 1800000uV into 1500000-1500000uV
	[    2.074575] LDO_REG6: supplied by vcc_buck5
	[    2.081638] LDO_REG7: supplied by vcc5v0_sys
	[    2.088719] LDO_REG8: supplied by vcc5v0_sys
	[    2.095745] LDO_REG9: supplied by vcc5v0_sys
	[    2.102782] SWITCH_REG1: supplied by vcc5v0_sys
	[    2.108399] SWITCH_REG2: supplied by vcc3v3_sys
	[    2.130110] energy_model: pd0: hertz/watts ratio non-monotonically decreasing: em_cap_state 1 >= em_cap_state0
	[    2.145681] cpufreq: cpufreq_online: CPU4: Running at unlisted freq: 12000 KHz
	[    2.154394] cpufreq: cpufreq_online: CPU4: Unlisted initial frequency changed to: 408000 KHz
	[    2.165782] sdhci: Secure Digital Host Controller Interface driver
	[    2.172707] sdhci: Copyright(c) Pierre Ossman
	[    2.177796] Synopsys Designware Multimedia Card Interface Driver
	[    2.185245] dwmmc_rockchip fe320000.mmc: IDMAC supports 32-bit address mode.
	[    2.193175] dwmmc_rockchip fe320000.mmc: Using internal DMA controller.
	[    2.200574] dwmmc_rockchip fe320000.mmc: Version ID is 270a
	[    2.206827] dwmmc_rockchip fe320000.mmc: DW MMC controller at irq 25,32 bit host data width,256 deep fifo
	[    2.217994] dwmmc_rockchip fe320000.mmc: Got CD GPIO
	[    2.237965] mmc_host mmc0: Bus speed (slot 0) = 400000Hz (slot req 400000Hz, actual 400000HZ div = 0)
	[    2.261712] sdhci-pltfm: SDHCI platform and OF driver helper
	[    2.268765] mmc1: CQHCI version 5.10
	[    2.298460] mmc1: SDHCI controller on fe330000.sdhci [fe330000.sdhci] using ADMA
	[    2.307986] ledtrig-cpu: registered to indicate activity on CPUs
	[    2.309063] mmc_host mmc0: Bus speed (slot 0) = 50000000Hz (slot req 50000000Hz, actual 50000000HZ div = 0)
	[    2.315429] usbcore: registered new interface driver usbhid
	[    2.325995] mmc0: new high speed SDHC card at address 0001
	[    2.331860] usbhid: USB HID core driver
	[    2.338562] mmcblk0: mmc0:0001 EB1QT 29.8 GiB 
	[    2.345053] NET: Registered protocol family 17
	[    2.352301] 9pnet: Installing 9P2000 support
	[    2.353868] GPT:Primary header thinks Alt. header is not at the end of the disk.
	[    2.357113] Key type dns_resolver registered
	[    2.365347] GPT:589856 != 62521343
	[    2.365348] GPT:Alternate GPT header not at the end of the disk.
	[    2.365350] GPT:589856 != 62521343
	[    2.370379] registered taskstats version 1
	[    2.373919] GPT: Use GNU Parted to correct GPT errors.
	[    2.373936]  mmcblk0: p1 p2 p3 p4
	[    2.380643] Loading compiled-in X.509 certificates
	[    2.420197] ALSA device list:
	[    2.423536]   No soundcards found.
	[    2.427498] dw-apb-uart ff1a0000.serial: forbid DMA for kernel console
	[    2.443331] EXT4-fs (mmcblk0p4): mounted filesystem with ordered data mode. Opts: (null)
	[    2.452444] VFS: Mounted root (ext4 filesystem) on device 179:4.
	[    2.460160] devtmpfs: mounted
	[    2.465654] Freeing unused kernel memory: 5568K
	[    2.470804] Run /sbin/init as init process
	[    2.497297] mmc1: Command Queue Engine enabled
	[    2.502282] mmc1: new HS400 Enhanced strobe MMC card at address 0001
	[    2.509710] mmcblk1: mmc1:0001 D9D16G 14.5 GiB 
	[    2.514884] mmcblk1boot0: mmc1:0001 D9D16G partition 1 4.00 MiB
	[    2.521595] mmcblk1boot1: mmc1:0001 D9D16G partition 2 4.00 MiB
	[    2.528647] mmcblk1rpmb: mmc1:0001 D9D16G partition 3 4.00 MiB, chardev (234:0)
	[    2.540885]  mmcblk1: p1 p2 p3 p4 p5 p6 p7 p8 p9 p10 p11 p12 p13 p14
	[    2.549382] EXT4-fs (mmcblk0p4): re-mounted. Opts: (null)
	Starting syslogd: OK
	Starting klogd: OK
	Running sysctl: OK
	Saving random seed: [    2.605267] random: dd: uninitialized urandom read (512 bytes read)
	OK
	Starting system message bus: [    2.649896] random: dbus-uuidgen: uninitialized urandom read (12 bytes read)
	[    2.657864] random: dbus-uuidgen: uninitialized urandom read (8 bytes read)
	dbus-daemon[237]: Failed to start message bus: Could not get UID and GID for username "dbus"
	done
	Starting network: OK
	Starting dhcpcd...
	[    2.777277] 8021q: 802.1Q VLAN Support v1.8
	[    2.838810] cfg80211: Loading compiled-in X.509 certificates for regulatory database
	[    2.898377] cfg80211: Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
	[    2.905855] platform regulatory.0: Direct firmware load for regulatory.db failed with error -2
	no valid interfaces found
	[    2.915550] platform regulatory.0: Falling back to sysfs fallback for: regulatory.db
	no interfaces have a carrier
	forked to background, child pid 261
	Starting ntpd: [    3.106005] NET: Registered protocol family 10
	[    3.111902] Segment Routing with IPv6
	OK
	[    6.120757] random: crng init done
	[    6.124591] random: 2 urandom warning(s) missed due to ratelimiting
	ssh-keygen: generating new host keys: RSA DSA ECDSA ED25519 
	Starting sshd: Privilege separation user sshd does not exist
	OK
	Starting HPA's tftpd: done

	Welcome to ROCKPI-N10..!!
	rockpi-n10 login:

use root for login.
