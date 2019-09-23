# jem_misc
Misc things needed to get LineageOS on an Amazon Kindle Fire HD 8.9" (jem)

This is all public info but it is getting old and hard to find. I gathered it all in one place.

You are entirely responsible for anything bad that happens when following this guide but it should work fine.

## Step 0 - Setup

* Make sure you have an Amazon Kindle Fire HD 8.9" (jem) as this guide is only valid for that exact device
* Make sure battery is fully charged
* Download the following files from this branch:

  * [kfhd8-freedom-boot-8.4.6.img](https://github.com/jsheradin/jem_misc/blob/master/kfhd8-freedom-boot-8.4.6.img) MD5: 8374cf88e75abda8c374044a1f0daa5f
  * [jem_recovery_160403.img](https://github.com/jsheradin/jem_misc/blob/master/jem_recovery_160403.img) MD5: 4e0dae2b0794294efd4316424afd2956
  * [kfhd8-u-boot-prod-8.1.4.bin](https://github.com/jsheradin/jem_misc/blob/master/kfhd8-u-boot-prod-8.1.4.bin) MD5: a56f24c0c01aaea4bf408bc710faadaa
  * [stack](https://github.com/jsheradin/otter2_misc/raw/master/stack) MD5: 3cee2b7f3233fc3a1e10373677b8c1a9
  * [superuser.zip](https://github.com/jsheradin/jem_misc/raw/master/superuser.zip) MD5: a124270d1e687045e71c0431277c184c
  * [kindlehd2013_root.zip](https://github.com/jsheradin/jem_misc/raw/master/kindlehd2013_root.zip) MD5: 8c344b6bf9fcf9d6a4796a91fc95004e

* Download a custom ROM and (optionally) OpenGapps
  * [My LineageOS 11 ROM](https://github.com/jsheradin/android_device_amazon_jem/releases)
  * [OpenGapps](https://opengapps.org/) (ARM > 4.4 if using my LOS11 ROM)
* Obtain a fastboot cable with the following pinout

  ![fastboot](https://raw.githubusercontent.com/jsheradin/otter2_misc/master/fastbootcable.jpg)
* Have working ADB and Fastboot on a computer

## Step 1 - Stack override
* Root the device
  * Extract the contents of *kindlehd2013_root.zip* to a folder
  * Extract the file *su* from *superuser.zip>armeabi* to the same folder, overwriting
  * With a shell in that folder, run the following commands
  
        adb push su /data/local/tmp/
        adb push rootme.sh /data/local/tmp/
        adb push exploit /data/local/tmp/
        adb shell chmod 755 /data/local/tmp/rootme.sh
        adb shell chmod 755 /data/local/tmp/exploit
        adb shell /data/local/tmp/exploit -c "/data/local/tmp/rootme.sh"

* Apply stack override
    
      adb push stack /sdcard/
      adb shell su -c "dd if=/sdcard/stack of=/dev/block/platform/omap/omap_hsmmc.1/by-name/system bs=6519488 seek=1"

## Step 2 - Test

* Connect the tablet to a computer using the fastboot cable
* Reboot the tablet (you should see a fastboot screen)
* Test fastboot by running the following command:

      fastboot getvar product
* It should return something like:

      product: Jem-PVT-Prod-04
      Finished. Total time: 0.000s
* If the product string does not begin with *Jem*, **do not proceed**

## Step 3 - Flash files

* With a shell in the directory containing the files, run the following commands:

      fastboot flash bootloader kfhd8-u-boot-prod-8.1.4.bin
      fastboot flash recovery kfhd8-twrp-2.8.7.0-recovery.img
      fastboot flash boot kfhd8-freedom-boot-8.4.6.img
      
 * If all commands ran successfully, unplug the tablet
 
 ## Step 4 - Enter recovery
 
 * With the tablet unplugged, hold the power button until the device powers off
 * Hold the power button until you see a boot screen with *Kindle* written in orange
 * Hold Vol-Up (Closest to headphone jack) until you see a boot screen with *Kindle* written in blue
 * Device should then boot into TWRP
 
 ## Step 5 - Flash ROM
 
 * In TWRP
   * Wipe > Advanced > select everything > confirm
   * Wipe > Format data > confirm
   * Wipe > Factory reset > confirm
 * Repeat step 3 to enter recovery
 * Connect device to computer using normal micro-USB cable
 * In TWRP
   * Advanced > ADB Sideload > Swipe to start sideload
 * Run the following command (replace the lineage zip with whatever custom ROM you use):
 
       adb sideload lineage-XX-XXXXXXXX-UNOFFICIAL.zip
 * In TWRP
   * Back arrow > ADB Sideload > Swipe to start sideload
 * Run the following command:
 
       adb sideload open_gapps-arm-4.4-XXXX-XXXXXXXX.zip
  * Unplug tablet and reboot
  * First boot may take a long time
  * Enjoy
  
## Credit:
* All original devs
* transi1 for twrp3
* fattire for bootloader exploit
* verygreen for work on 2nd bootloader
* Hashcode for most of it
* Original guide (https://forum.xda-developers.com/showthread.php?t=2128175)
* Root method (https://forum.xda-developers.com/kindle-fire-hd/8-9-inch-help/success-rooting-8-5-1-t2943620)
* tripmonger
