OS
=========
We used [Ubuntu 14.04 32-Bit](http://releases.ubuntu.com/14.04/ubuntu-14.04.5-desktop-i386.iso)

Folder Structure
=========
* /workspace
  - uclinux
  - filesys
  - boot
  - toolchain
  - openocd

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

Setup OpenOCD
=========
```
	sudo apt-get install libtool automake libusb-1.0.0-dev texinfo libusb-dev
	cd ~/workspace
	git clone git://git.code.sf.net/p/openocd/code openocd
	cd openocd
	git submodule init && git submodule update && ./bootstrap && ./configure --enable-stlink  && make && sudo make install	
	sudo cp  /usr/local/share/openocd/contrib/99-openocd.rules /etc/udev/rules.d/
```
Note: If you're using ST-Link v2-1 you may need to change USB VID&PID settings in stlink-v2.cfg file before make
```
	hla_vid_pid 0x0483 0x3748 ==> hla_vid_pid 0x0483 0x374
```
Note: OpenOCD rule file name (99-openocd.rules) is not fixed! Check it before copy.

FLASH
=========	
Sample Flash command
```	
	openocd -f  board/stm32f429discovery.cfg \
	-c "init" \
	-c "reset init" \
	-c "flash probe 0" \
	-c "flash info 0" \
	-c "flash write_image erase ~/workspace/boot/u-boot/stm32429-disco/u-boot.bin 0x08000000" \
	-c "flash write_image erase ~/workspace/uclinux/arch/arm/boot/xipuImage.bin 0x08020000" \
	-c "flash write_image erase ~/workspace/filesys/romfs.bin 0x08120000" \
	-c "reset run" \
	-c "shutdown"
```
USART Connection	
=========

USART1:

pin PA9 -> TXD

pin PA10 -> RXD

USART3:

pin PC10 -> TXD

pin PC11 -> RXD

If you're using ST-Link v2-1 debugger you can use debugger's Virtual UART COM port (probably /dev/ttyACMx)
