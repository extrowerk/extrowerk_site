---
layout:     post
title:      "Exploring Kindle Paperwhite"
subtitle:   "Experiences with Embedded ARM Systems"
date:       2015-01-09 12:00:00
author:     "z"
header-img: "img/ebook_between_paper_books_mini.jpg"
---

###How to start
First, you need the latest firmware update [firmware rev 5.4.4.2](http://www.amazon.com/gp/help/customer/display.html/ref=hp_left_sib?ie=UTF8&nodeId=201064850 "Amazon Kindle Paperwhite 1 firmware update") and the [KindleTool](https://github.com/yifanlu/KindleTool "KindleTool Github Site").
Download booth of them, and extract the rootfs:

`./kindletool extract update_kindle_5.4.4.2.bin`

You'll get the rootfs, the uImage kernel and some payload data.

###Exploring the rootfs:

HW and SW developed by [lab126](http://www.lab126.com/ "Official webpage") in Cupertino, USA, assembled in China.

####Architecture codenames:
* ADS
* Mario
* Nell
* Turing
* TuringWW
* NellSL and NellWW
* Luigi
* Shasta
* Yoshi

####Hardware:
* Processor	: ARMv7 Processor rev 5 (v7l)
* BogoMIPS	: 799.53
* Features	: swp half thumb fastmult vfp edsp neon vfpv3 
* CPU implementer	: 0x41
* CPU architecture: 7
* CPU variant	: 0x2
* CPU part	: 0xc08
* CPU revision	: 5
* Hardware	: Amazon.com MX50 YOSHIME Board
* Revision	: 50011
* Serial		: "B0241603xxxxxxxx"
* BoardId		: "00A16071xxxxxxxx"
* RAM : 256 MB
* WLAN : Atheros AR6003

####fstab:

	# <file system>   <mount pt>    <type> <options>                                         <dump>  <pass>
	/dev/mmcblk0p1    /             ext3   suid,exec,auto,nouser,async,rw,noatime,nodiratime 0      1
	proc              /proc         proc   defaults                                          0      0
	sysfs             /sys          sysfs  defaults                                          0      0
	devpts            /dev/pts      devpts defaults,gid=5,mode=620                           0      0
	usbfs             /proc/bus/usb usbfs  defaults                                          0      0
	/dev/loop/0       /mnt/base-us  vfat   defaults,noatime,nodiratime,utf8,noexec,uid=0,gid=0,umask=0,shortname=mixed              0      0
	/dev/mmcblk1p1    /mnt/base-mmc vfat   defaults,noatime,nodiratime,utf8,noexec,shortname=mixed              0      0
	fsp#/mnt/base-us  /mnt/us       fuse   allow_other,umask=0,uid=0,gid=0,rw,max_write=65536,max_readahead=65536,noatime,noexec,nosuid,nodev,nonempty     0      0
	fsp#/mnt/base-mmc /mnt/mmc      fuse   rw,max_write=65536,max_readahead=65536,noatime,exec,nosuid,nodev,nonempty     0      0

####Interesting binaries:

* [Linux kindle 2.6.31-rt11-lab126 #1 Mon Apr 28 12:33:59 PDT 2014 armv7l GNU/Linux](http://kernelnewbies.org/Linux_2_6_31 "Linux kernel")
* [Awesome WM](http://awesome.naquadah.org/ "Awesome Window Manager")
* [X Window System](http://en.wikipedia.org/wiki/X_Window_System "X11")
* [Pango for laying out and rendering of text](http://www.pango.org/ "Pango official site")
* [MESA/OpenGL](http://www.mesa3d.org/ "MESA 3D")
* [GTK](http://www.gtk.org/ "Gnome ToolKit")
* [Webkit](http://https://www.webkit.org/ "Webkit Engine")

####dmesg through SSH with [usbnet](http://www.mobileread.com/forums/showthread.php?t=225030 "Mobilread forum ")
	Linux version 2.6.31-rt11-lab126 (jenkins-official@sjc10-jbuild09) (collect2: ld returned 1 exit status) #1 Mon Apr 28 12:33:59 PDT 2014
	CPU: ARMv7 Processor [412fc085] revision 5 (ARMv7), cr=10c53c7f
	CPU: VIPT nonaliasing data cache, VIPT nonaliasing instruction cache
	Machine: Amazon.com MX50 YOSHIME Board
	Board ID and Serial Number driver for Lab126 boards version 1.0
	MX50 Board id - 00A16071xxxxxxxx
	Memory policy: ECC disabled, Data cache writeback
	On node 0 totalpages: 65536
	free_area_init_node: node 0, pgdat c046e89c, node_mem_map c0498000
		DMA zone: 192 pages used for memmap
		DMA zone: 0 pages reserved
		DMA zone: 24384 pages, LIFO batch:3
		Normal zone: 320 pages used for memmap
		Normal zone: 40640 pages, LIFO batch:7
	Built 1 zonelists in Zone order, mobility grouping on.  Total pages: 65024
	Kernel command line: consoleblank=0 rootwait ro ip=off root=/dev/mmcblk0p1 quiet eink=fslepdc video=mxcepdcfb:E60,bpp=8,x_mem=4M mem=256M user_debug=0x1 console=ttymxc0,115200
	PID hash table entries: 1024 (order: 10, 4096 bytes)
	Dentry cache hash table entries: 32768 (order: 5, 131072 bytes)
	Inode-cache hash table entries: 16384 (order: 4, 65536 bytes)
	Memory: 256MB = 256MB total
	Memory: 255012KB available (3296K code, 359K data, 1004K init, 0K highmem)
	NR_IRQS:368
	MXC IRQ initialized
	cko2_set_rate, new divider=0
	MXC_Early serial console at MMIO 0x53fbc000 (options '115200')
	console [ttymxc0] enabled
	Console: colour dummy device 80x30
	Calibrating delay loop... 799.53 BogoMIPS (lpj=3997696)
	Mount-cache hash table entries: 512
	CPU: Testing write buffer coherency: ok
	regulator: core version 0.5
	NET: Registered protocol family 16
	i.MX IRAM pool: 128 KB@0xd0840000
	MX50 LPDDR2 MfgID: 0x1 [Samsung]
	CPU is i.MX50 Revision 1.1
	MXC GPIO hardware
	Using SDMA I.API
	MXC DMA API initialized
	bio: create slab <bio-0> at 0
	CSPI: mxc_spi-0 probed
	CSPI: mxc_spi-1 probed
	CSPI: mxc_spi-2 probed
	MXC I2C driver
	MXC I2C driver
	MXC I2C driver
	PMIC Light driver loading...
	mc13892 Rev 2.4 FinVer a detected
	Initializing regulators for mx50 yoshi.
	regulator: SW1: 600 <--> 1375 mV 
	regulator: SW2: 900 <--> 1850 mV 
	regulator: SW3: 900 <--> 1850 mV 
	regulator: SW4: 1100 <--> 1850 mV 
	regulator: SWBST: 0 mV 
	regulator: VIOHI: 0 mV 
	regulator: VPLL: 1050 <--> 1800 mV 
	regulator: VDIG: 1200 mV 
	regulator: VSD: 1800 <--> 3150 mV 
	regulator: VUSB2: 2400 <--> 2775 mV 
	regulator: VVIDEO: 2775 mV 
	regulator: VAUDIO: 2300 <--> 3000 mV 
	regulator: VCAM: 2500 <--> 3000 mV fast normal 
	regulator: VGEN1: 3000 mV 
	regulator: VGEN2: 1200 <--> 3150 mV 
	regulator: VGEN3: 1800 mV 
	regulator: VUSB: 0 mV 
	regulator: GPO1: 0 mV 
	regulator: GPO2: 0 mV 
	regulator: GPO3: 0 mV 
	regulator: GPO4: 0 mV 
	PMIC ADC start probe
	PMIC Light successfully loaded
	Device spi3.0 probed
	NET: Registered protocol family 2
	IP route cache hash table entries: 2048 (order: 1, 8192 bytes)
	TCP established hash table entries: 8192 (order: 4, 65536 bytes)
	TCP bind hash table entries: 8192 (order: 3, 32768 bytes)
	TCP: Hash tables configured (established 8192 bind 8192)
	TCP reno registered
	NET: Registered protocol family 1
	LPMode driver module loaded
	Static Power Management for Freescale i.MX5
	PM driver module loaded
	sdram autogating driver module loaded
	Bus freq driver module loaded
	mxc_dvfs_core_probe
	DVFS driver module loaded
	i.MXC CPU frequency driver
	squashfs: version 4.0 (2009/01/31) Phillip Lougher
	Registering unionfs 2.5.10 (for 2.6.31.14)
	msgmni has been set to 498
	alg: No test for stdrng (krng)
	io scheduler noop registered
	io scheduler anticipatory registered
	io scheduler deadline registered
	io scheduler cfq registered (default)
	regulator: DISPLAY: 0 mV 
	regulator: GVDD: 20000 mV 
	regulator: GVEE: -22000 mV 
	regulator: VCOM: 0 <--> 2749 mV 
	regulator: VNEG: -15000 mV 
	regulator: VPOS: 15000 mV 
	regulator: TMST: 0 mV 
	papyrus 1-0068: PMIC PAPYRUS for eInk display
	Amazon MX35 Yoshi Power Button Driver
	Serial: MXC Internal UART driver
	mxcintuart.0: ttymxc0 at MMIO 0x53fbc000 (irq = 31) is a Freescale MXC
	console handover: boot [ttymxc0] -> real [ttymxc0]
	mxcintuart.1: ttymxc1 at MMIO 0x53fc0000 (irq = 32) is a Freescale MXC
	mxcintuart.2: ttymxc2 at MMIO 0x5000c000 (irq = 33) is a Freescale MXC
	mxcintuart.3: ttymxc3 at MMIO 0x53ff0000 (irq = 13) is a Freescale MXC
	loop: module loaded
	mxc_rtc mxc_rtc.0: rtc core: registered mxc_rtc as rtc0
	Probing mxc_rtc done
	mc13892 rtc probe start
	pmic_rtc pmic_rtc.1: rtc core: registered pmic_rtc as rtc1
	mc13892 rtc probe succeed
	i2c /dev entries driver
	MXC WatchDog Driver 2.0
	MXC Watchdog # 0 Timer: initial timeout 127 sec
	MXC Watchdog: Started 10000 millisecond watchdog refresh
	PMIC Character device: successfully loaded
	pmic_battery: probe of pmic_battery.1 failed with error -1
	sdhci: Secure Digital Host Controller Interface driver
	sdhci: Copyright(c) Pierre Ossman
	mxsdhci: MXC Secure Digital Host Controller Interface driver
	mxsdhci: MXC SDHCI Controller Driver. 
	mmc0: SDHCI detect irq 273 irq 2 INTERNAL DMA
	mxsdhci: MXC SDHCI Controller Driver. 
	mmc1: SDHCI detect irq 0 irq 3 INTERNAL DMA
	Registered led device: pmic_ledsr
	Registered led device: pmic_ledsg
	Registered led device: pmic_ledsb
	nf_conntrack version 0.5.0 (4096 buckets, 16384 max)
	ip_tables: (C) 2000-2006 Netfilter Core Team
	TCP cubic registered
	NET: Registered protocol family 17
	RPC: Registered udp transport module.
	RPC: Registered tcp transport module.
	kernel: I perf:kernel:kernel_loaded=0.48 seconds:
	VFP support v0.3: implementor 41 architecture 3 part 30 variant c rev 2
	(...)
	mxc_rtc mxc_rtc.0: setting system clock to 2015-01-09 12:14:16 UTC (1420805656)
	Freeing init memory: 1004K
	(...)
	mmc0: new high speed SDIO card at address 0001
	emmc: I def:mmcpartinfo:vendor=unknown, ddr=0, host=mmc1, manufacturer_id=254:
	mmc1: new high speed MMC card at address 0001
	mmcblk0: mmc1:0001 MMC02G 1.83 GiB 
		 mmcblk0: p1 p2 p3 p4
	INFO:Loaded module /lib/modules/eink_fb_waveform.ko  (40028 bytes)
	mxc_epdc_fb_init_hw: 06_05_00ac_3c_2b4601_07_dc_00000dd8_85_12.wbf
	INFO:Loaded module /lib/modules/mxc_epdc_fb.ko default_panel_hw_init=1 default_update_mode=1 (51640 bytes)
	INFO:eink initialized... (786432 bytes)
	INFO:!!! Checking MBR /dev/mmcblk0 !!!!  
	INFO:partition 2, start sector is 782336
	INFO:partition 3, start sector is 913408
	INFO:partition 4, start sector is 1044480
	INFO:maximizing partition 2797568 sectors
	INFO:*** Partition table verified for /dev/mmcblk0 ***
	INFO:Setup loop device /dev/loop0 for /dev/mmcblk0p4 + 8192
	INFO:Checking for updates... (auto-pilot mode)
	INFO:No update*.bin found; no update needed.
	INFO:no updates found.
	kjournald starting.  Commit interval 5 seconds
	EXT3-fs: mounted filesystem with writeback data mode.
	ARC USB OTG/Gadget/Device Controller driver (26 Mar 2012)
	kernel: I perf:usb:usb_gadget_loaded=8.19 seconds:
	g_file_storage gadget: File-backed Storage Gadget, version: 7 August 2007
	g_file_storage gadget: Number of LUNs=1
	fsl-usb2-udc: bind to driver g_file_storage 
	fuse init (API version 7.12)
	kjournald starting.  Commit interval 5 seconds
	EXT3 FS on mmcblk0p3, internal journal
	EXT3-fs: mounted filesystem with writeback data mode.
	cypress reset
	cyttsp_bl_app_valid: App found; normal boot
	cyttsp: I mfgcmd :cmd_cmpl::
	cyttsp_set_sysinfo_mode: hst_mode:0x90, mfg_stat:0x44, mfg_cmd:0x0 
	cyttsp_set_sysinfo_mode:SI2:tts_ver=0x15D5 app_id=0x2054 app_ver=0x020A c_id=0x000011
	cyttsp_set_operational_mode: hstmode:0x0 tt_mode:0x0 tt_stat:0x0 
	cyttsp_power_on: Power state is ACTIVE
	input: cyttsp as /devices/platform/cyttsp.0/input/input0
	cyttsp: I cyttsp_proc_write::command=unlock:
	Loading Atheros ar6003 driver
	SDIO: Enabling device mmc0:0001:1...
	SDIO: Enabled device mmc0:0001:1
	kernel: I perf:wifi:wifi_driver_loaded=29.55 seconds:
	ar6003 Driver returning from ar6000_init_module
	ar6k_wlan mmc0:0001:1: firmware: requesting /opt/ar6k/target/AR6003/hw2.1.1/bin/active_calibration
	MAC from kernel xx:xx:xx:xx:xx:F0
	ar6k_wlan mmc0:0001:1: firmware: requesting /opt/ar6k/target/AR6003/hw2.1.1/bin/otp.bin
	ar6k_wlan mmc0:0001:1: firmware: requesting /opt/ar6k/target/AR6003/hw2.1.1/bin/athwlan.bin
	ar6k_wlan mmc0:0001:1: firmware: requesting /opt/ar6k/target/AR6003/hw2.1.1/bin/data.patch.hw3_0.bin
	ar6003 driver: Completed loading the module ar6000_avail_ev
	mxc_rtc: saved=0x9f5778 boot=0x9f57af
	boot: I def:rbt:reset=user_reboot,version=232331:
	unregistered gadget driver 'g_file_storage'
	ARC USB OTG/Gadget/Device Controller driver unregistered 
	ARC USB OTG/Gadget/Device Controller driver (26 Mar 2012)
	kernel: I perf:usb:usb_gadget_loaded=102.58 seconds:
	usb0: MAC ee:19:00:00:00:00
	usb0: HOST MAC ee:49:00:00:00:00
	g_ether gadget: Ethernet Gadget, version: Memorial Day 2008
	g_ether gadget: g_ether ready
	fsl-usb2-udc: bind to driver g_ether 
	[root@kindle root]# YAY!

###Virtualization:
I was not able yet to start the kernel in [QEMU](http://wiki.qemu.org/Main_Page "QEMU machine emulator and virtualizer"), but i will update this post if i achieve something on this field.

###Opinion:
lab126 made a nice work, but i would like to see a new e-book reader generation without X11, but with Wayland/MIR, with faster A4 HiDPI e-Paper panel and an SD-Card Slot. (Sandisk recently announced the world's first SD card with a capacity of 512 gigabytes—half a terabyte of storage.) Oh, and with nice PDF rendering, please!
