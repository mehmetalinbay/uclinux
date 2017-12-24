Small Linux version with MMU less support. 
uClinux, μClinux and the logos versions are Trademarks of Arcturus Networks Inc. 
Copyright © 1998 - 2002 D. Jeff Dionne and Michael Durrant 
Copyright © 2001 - 2011 Arcturus Networks Inc. 

This is the source tree of the uClinux kernel that is part of the Linux
Cortex-M distribution.

Folder Structure
=========
* /workspace
  - uclinux
  - filesys
  - toolchain

```
	cd home/$USER
	mkdir workspace
	cd workspace
```

Dependicies
=========
The builder requires that various tools and packages be available for use in
the build procedure:
```
	sudo apt-get install libncurses-dev libglib2.0-dev libfdt-dev libpixman-1-dev zlib1g-dev git
```

Setup Toolchain
=========
* Set ARM/uClinux Toolchain:
  - Download [arm-2010q1-189-arm-uclinuxeabi-i686-pc-linux-gnu.tar.bz2](https://sourcery.mentor.com/public/gnu_toolchain/arm-uclinuxeabi/arm-2010q1-189-arm-uclinuxeabi-i686-pc-linux-gnu.tar.bz2) from Mentor Graphics
  - only arm-2010q1 is known to work; don't use SourceryG++ arm-2011.03
```
	cd ~/workspace
	mkdir toolchain
	cd toolchain
	wget https://sourcery.mentor.com/public/gnu_toolchain/arm-uclinuxeabi/arm-2010q1-189-arm-uclinuxeabi-i686-pc-linux-gnu.tar.bz2
	tar jxvf arm-2010q1-189-arm-uclinuxeabi-i686-pc-linux-gnu.tar.bz2
```
* To set toolchain path permanently
```
	cd (goto home/$USER directory)
	gedit .bashrc
```
* add following lines to bashrc
```
	PATH=~/workspace/toolchain/arm-2010q1/bin:$PATH (beware of your PATH)
	export PATH
```
* Restart Ubuntu
* Check toolchain PATH

Download Kernel
=========
```
	cd ~/workspace
	git clone https://github.com/mehmetalinbay/uclinux.git
```
Compile 
=========
```
	cd ~/workspace/uclinux
	cp config.robutest .config
```
or
```
	cd ~/workspace/uclinux
	cp config.malinbay .config
```
Note: config.robutest is fork default
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

