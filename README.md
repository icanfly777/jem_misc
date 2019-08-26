# jem_misc
Misc things needed to get LineageOS on an Amazon Kindle Fire HD 8.9" (jem)

This is all public info but it is getting old and hard to find. I gathered it all in one place.

You are entirely responsible for anything bad that happens when following this guide but it should work fine.

## Step 0 - Setup

* Make sure you have an Amazon Kindle Fire HD 8.9" (jem) as this guide is only valid for that exact device
* Make sure battery is fully charged
* Download the following files from this branch:

  [kfhd8-freedom-boot-8.4.6.img](https://github.com/jsheradin/jem_misc/blob/master/kfhd8-freedom-boot-8.4.6.img) MD5: 8374cf88e75abda8c374044a1f0daa5f

  [kfhd8-twrp-2.8.7.0-recovery.img](https://github.com/jsheradin/jem_misc/blob/master/kfhd8-twrp-2.8.7.0-recovery.img) MD5: 9ff016b4b0ca6e71e05996502d49ea4c

  [kfhd8-u-boot-prod-8.1.4.bin](https://github.com/jsheradin/jem_misc/blob/master/kfhd8-u-boot-prod-8.1.4.bin) MD5: a56f24c0c01aaea4bf408bc710faadaa

* Download a custom ROM and (optionally) OpenGapps
* Obtain a fastboot cable with the following pinout

  ![fastboot](https://raw.githubusercontent.com/jsheradin/otter2_misc/master/fastbootcable.jpg)
* Have working ADB and Fastboot on a computer

## Step 1 - Test

* Connect the tablet to a computer using the fastboot cable
* Reboot the tablet (you should see a fastboot screen)
* Test fastboot by running the following command:

      fastboot getvar product
* It should return something like:

      product: Jem-PVT-Prod-04
      Finished. Total time: 0.000s
* If the product string does not begin with *Jem*, **do not proceed**

## Step 2 - Flash files

* With a shell in the directory containing the above files, run the following commands:

      fastboot flash bootloader kfhd8-u-boot-prod-8.1.4.bin
      fastboot flash recovery kfhd8-twrp-2.8.7.0-recovery.img
      fastboot flash boot kfhd8-freedom-boot-8.4.6.img
      
 * If all commands ran successfully, unplug the tablet
 
 ## Step 3 - Enter recovery
 
 * With the tablet unplugged, hold the power button until the device powers off
 * Hold the power button until you see a boot screen with *Kindle* written in orange
 * Repeatedly press Vol-Up (Closest to headphone jack) until you see a boot screen with *Kindle* written in blue
 * Device should then boot into TWRP
 
 ## Step 4 - Flash ROM
 
 * In TWRP
   * Wipe > Advanced > select everything > confirm
   * Wipe > Format data > confirm
   * Wipe > Factory reset > confirm
 * Repeat step 3 to enter recovery
 * Connect device to computer using normal micro-USB cable
 * In TWRP
   * Advanced > ADB Sideload > Swipe to start sideload
 * Run the following commands (replace the lineage zip with whatever custom ROM you use):
 
       adb sideload lineage-XX-XXXXXXXX-UNOFFICIAL.zip
       adb sideload open_gapps-arm-4.4-XXXX-XXXXXXXX.zip
  * Unplug tablet and reboot
  * First boot may take a long time
  * Enjoy
  
Credit:
* All original devs
* Hashcode for the original guide (https://forum.xda-developers.com/showthread.php?t=2128175)
