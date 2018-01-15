Small Linux version with MMU less support. 
uClinux, μClinux and the logos versions are Trademarks of Arcturus Networks Inc. 
Copyright © 1998 - 2002 D. Jeff Dionne and Michael Durrant 
Copyright © 2001 - 2011 Arcturus Networks Inc. 

This is the source tree of the uClinux kernel that is part of the Linux
Cortex-M distribution.

Prerequisites
=========
Please read [Default Setup](https://github.com/mehmetalinbay/uclinux/blob/master/default_setup_README.md)

Download Kernel
=========
```
	cd ~/workspace
	git clone https://github.com/vf-tech/uclinux.git
```
Compile 
=========
```
	git checkout initial
	cd ~/workspace/uclinux
	cp config.robutest .config
```
or
```
	git checkout master
	cd ~/workspace/uclinux
	cp config.malinbay .config
```
Note: config.robutest is fork's default config file

Note: in config.malinbay USART1 & SPI5 enabled

```
	make menuconfig
	make ARCH=arm CROSS_COMPILE=arm-uclinuxeabi- -j4
```
Sample Output Message
```
	OBJCOPY arch/arm/boot/xipImage
	Kernel: arch/arm/boot/xipImage is ready (physical address: 0x08020040)
	Building modules, stage 2.
	MODPOST 0 modules
```		
Convert xipImage to uImage
=========
* xipImage must be converted to u-boot compataible using make_uboot_ximage
```
	cd arch/arm/boot/
	./make_uboot_ximage
```
Sample Output Message
```
	Image Name:   Linux-2.6.33-arm1
	Created:      Fri Dec  1 23:57:44 2017
	Image Type:   ARM Linux Kernel Image (uncompressed)
	Data Size:    889568 Bytes = 868.72 kB = 0.85 MB
	Load Address: 08020040
	Entry Point:  08020041
```

