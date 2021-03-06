# Remarkable tablet system tools 
Please be aware that you can potentially break your remarkable tablet. Even if unlikely even bricking it behind repair.
Nobody is responsible for the actions you are taking with this scripts.

The uuu (Universial Update Utility) tool was created by NXP and made avilable under the BSD license. 
Please check here for details:
https://github.com/NXPmicro/mfgtools

This is a community effort. Please do not contact the rM-team. They are helpful but cannot provide support for third-party acitvities. 

## Preperation
### Clone this repository
```bash
git clone https://github.com/ddvk/remarkable-uuuflash.git 
cd remarkable-uuuflash
```
### Set the rM into recovery mode
* Attach the rM tablet via USB to the host machine
* Power-off the rM tablet
* Set it into upload mode, by pressing the middle (home) button and then the power button for > 3 seconds. The display will not change, there will be no feedback! 
* To control if the mode was set type:
```bash
dmesg
```
One of the last messages should be something (usb address might be different) like:
```
 hid-generic 0003:15A2:0063.0008: hiddev1,hidraw3: USB HID v1.10 Device [Freescale SemiConductor Inc  SE Blank MEGREZ] on usb-0000:00:1a.0-1.3/input0
```
Now you are ready to start the following actions from within this repository
## Actions

Depending on your system you might have to run the uuu tool with sudo rights, or as a better alternative create some udev rules.

### Boot into a recovery serial console
To boot into recovery use:
```bash
./uuu recover.uuu 
```
After the loading of the recovery image you should be able to login via a serial client
On Linux you can use
```bash
minicom -D /dev/ttyACM0
```
or
```bash
screen /dev/ttyACM0
```
Please notice, that the name of the device might be different depending on your system. 
On Windows you can use `putty` to establish a serial connection
If this does not work, check with `dmesg` the current status, there should be a line like:
```bash
cdc_acm 1-1.3:1.2: ttyACM0: USB ACM device
```
Note that the name of the serial device might differ from distro to distro. Try again, with the correct name, if no device is shown under dmesg. Something might have gone wrong. In that case please report your problems. Please be aware that the access to the serial device might require root rights, depending on your system. 

A login prompt will appear:
```bash 
Frankenboot rmrestore /dev/ttyGS0

rmrestore login:
```
To login use `root` as user.
#### Mount flash memory
The entire visible system is the initramfs within the rM RAM. Thus, the flash memory partitions of the real system have to be mounted, if you want to access it.
* Mount the internal flash memory partitions
```bash 
mount /dev/mmcblk1p2 /mnt/
mount /dev/mmcblk1p7 /mnt/home
mount /dev/mmcblk1p1 /mnt/var/lib/uboot
```
* For convince, one can chroot into the real system.
```bash 
chroot /mnt
```
* You can now change settings or reset passwords, etc. After you finished, type 
```bash 
exit #if you used the chroot
reboot
``` 
to restart the rM tablet and boot into the normal operation mode.

### Semi Upgrade (overwriting the root with version 2.1.1.3)
Use
```bash
./uuu upgrade.uuu
```
To be documented in detail

### Reflash ()
Use
```bash
./uuu reflash.uuu
```
To be documented in detail

### Sources
- remarkable / imx_usb_tool (initramfs)
- uuu tool
- uboot (modified to ignore env variables)
- zImages-fsl
