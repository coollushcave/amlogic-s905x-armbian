# amlogic-s905x-armbian

These instructions are for Amlogic CPUs for TV Boxes. 
 
Note: If you have previously run other distributions on the box such as coreelec the below installation will not work.  You will need to restore the original android firmware before attempting the install.  coreelec changes the boot environment in ways that are incompatible with these Armbian builds.
 
Download links:
    Weekly Community Rolling Builds:  https://www.armbian.com/amlogic-s9xx-tv-box/
    or build your own image using the Armbian build framework
 
Once you download your chosen build, you need to burn the image to an SD card.  Generally balenaEtcher is recommended as it does a verification of the burn.  Also be sure to use high quality SD cards.
 
Once you have the SD card with your chosen build, then you need to edit the boot configuration file on the SD card.  In the BOOT partition of the SD card there will be a file /boot/extlinux/extlinux.conf, that you need to edit.  There will also be a extlinux.conf.template file to use as a reference.  You will need to add a line into the extlinux.conf file for the Device Tree (dtb) file you will be using for your box.  Place this line before the APPEND line as shown in the .template file.
 
Basically you need to have the correct dtb for your box.  You may need to attempt to use different dtb files until you find the one that works the best for your box's hardware (there are a bunch of dtb files in /boot/dtb/amlogic/... to try depending on your cpu architecture and hardware).  It is unlikely that there will be a matching dtb file for your TV box.  The idea is to find the one that works best for your box.  This may mean that you try booting with different dtb files until you fine one that works good enough for your needs.  By searching the forums you will find information about what dtbs other users have found work best for different boxes.  Because you are booting from an SD card, you can easily try different dtb files.  The dtd files are named by cpu family.  So for example dtb files for the s905x2 cpu are named meson-g12a-*.  Below there is a table that shows the identifiers for each familiy (g12a for s905x2 in this case).
 
Next you need to copy the correct uboot for your box.  This is needed for how these builds boot on amlogic boxes.  There are four different u-boot files located in the /boot directory:  u-boot-s905, u-boot-s905x-s912, u-boot-s905x2-s922, u-boot-s905x3
You need to copy (note copy not move) the u-boot file that matches your cpu to a new file named u-boot.ext in the /boot directory
So for example with a TX3 mini box that has an s905w cpu you would copy u-boot-s905x-s912 to u-boot.ext: cp u-boot-s905x-s912 u-boot.ext
(See table below for more details on which u-boot to use for which cpu)
 
Once you have your SD card prepared you need to enable multiboot on the box.  There are different ways documented to do this, but the most common is the "toothpick" method.  The "toothpick" method means to hold the reset button while applying power to the box.  The reset button is often hidden and located at the back of the audio/video jack connector.  By pressing that button with a toothpick or other such pointed device you can enable multiboot.  What you need to do is have the box unplugged, have your prepared sd card inserted, then press and hold the button while inserting the power connector.  Then after a bit of time you can release the button.  (I don't know exactly how long you need to hold the button after power is applied, but if it doesn't work the first time try again holding for longer or shorter times).
 
You should now be booting into armbian/linux.  Note that the first boot takes longer as it is enlarging the root filesystem to utilize the entire SD card.
 
After you are satisfied that your box is working correctly for your needs you can optionally copy the installation from the SD card to internal emmc storage (assuming your box has emmc). (Note: Installing to emmc has some risks of bricking your box.  Don't do this unless you feel you understand how to reinstall your box's android firmware)  You install armbian to emmc by running the shell script in the /root directory: install-aml.sh. Note: It is not possible to install into emmc on boxes with the s905 cpu (s905x, s905w, s905x2, etc however should all be supported).  It is recommended that you make a backup of emmc first.  Also be prepared if anything goes horribly wrong with your emmc install to reinstall the android firmware using the Amlogic USB Burning Tool to unbrick your device.  If you have or can find an original android firmware on the internet and you can generally (but not always) recover a bricked box using the Amlogic tool and the original firmware file.
 
 
Mapping from CPU to uboot and dtb:
 
u-boot-s905
s905 - gxbb
 
u-boot-s905x2-s912
S905X - gxl
S905W - gxl
S905D - gxl
S905L - gxl
S805X - gxl
S912 - gxm
A311D - gxm
 
u-boot-s905x2-s922
S905X2 - g12a
S922 - g12b
 
u-boot-s905x3
S905X3 - sm1
 
Not supported or not tested
S805 -
S905W2 -
S905X4 -
S805X2 - s4
A113D - axg
A113X - axg
 
