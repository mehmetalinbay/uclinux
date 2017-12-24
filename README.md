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

