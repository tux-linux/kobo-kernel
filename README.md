# kobo-kernel

A set of enhanced kernels for various Kobo devices.
<br>
## Warning
This could render your Kobo completely unbootable and unrecoverable. Use at your own risk. In devices with internal SD cards, recovery should be possible, but in those with eMMCs, the only way out is serial port access to U-Boot and flashing the old kernel with fastboot on your computer. And well, if U-Boot was overwritten with the kernel (i.e. you missed a number in the `dd` command), you're screwed up. I'm not responsible, nor any Kobo developers, of any damage done to your device due to custom kernel flashing. You have been warned.

### Supported devices [WIP]
Kobo Glo HD
<br>
### How do I use this?
You'll need to flash the kernel, preferably with `dd`. Steps:
1. Dump the kernel (uImage) and everything that's inside the drivers/ folder in the onboard storage;
<br>
2. Telnet/SSH and type this in:
```
dd if=/mnt/onboard/uImage of=/dev/mmcblk0 bs=512 seek=2048
```
3. After it's finished, you'll want to get the modules installed. Make a backup of the existing ones:
```
cd / && tar cJf ./drivers.tar.xz drivers/
```
4. Install the modules. Do a `ls /drivers` and if necessary, `ls` the subdirectories to find which modules belongs where. E.g., you'll want to put `g_ether.ko` in `/drivers/<board/cpu id>/usb/gadget`. Just `cp`, example:
```
cp /mnt/onboard/g_ether.ko /drivers/mx6sl-ntx/usb/gadget
```
5. Once you're sure that everything is there and you didn't miss a step:
```
sync && reboot
```
Your Kobo should now boot with the new kernel.

### Sources
Official sources can be found here, in the hw/ folder: https://github.com/kobolabs/Kobo-Reader