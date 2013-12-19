<!--
PostId: 1756741514537983741
Title    : Upgrading to Kitkat on Nexus4 from rooted, custom 4.3 ROM
Labels   : Android
Format	 : markdown
-->
### Kitkat's here!
So Google finally posted kitkat factory images for Nexus4. Saw it on Reddit this morning and started the download before
I had my morning cuppa.

Hmm - and then to flash. Flashing image will nuke your device (including all photos etc) which I didn't want. As I was on
AOKP, a data wipe will be needed but there's no reason to kill my storage too. And while we're at it, why not also root 
it in the process.

It's been sometime since I've flashed and even then it's usually zips. I was running AOKP nightly
on my N4 so I'd have to do a full wipe. First step was backups...

1. Backup via titanium
2. Backup Nova desktop
3. Backup SMS and call logs
4. Nandroid backup from TWRP.

Then to move the backups to the PC - just in case... Moving Nandroid from TWRP was a little bit of an issue since they
implemented security. General advice is to do a `adb pull /sdcard/TWRP/BACKUPS` from recovery. Unfortunately, `adb` wasn't
detecting my device in recovery. Some more googleing and got [Nexus 4 Drivers for Windows](http://forum.xda-developers.com/showthread.php?t=1992345). Boot into recovery, and follow the driver installation directions exactly (pick android device and 'have driver').

I wanted to remain rooted - so download chainfire's SuperSU update zip and push it to the device with `adb push update-SuperSU.zip /sdcard/`

That took care of backing up Nandroids to the PC. Extract the factory image file `occam-jdq39-factory-345dc199.tgz` into 
a folder. Also open the `image-occam-jdq39.zip` inside and extract files from there into the same folder.

1. Reboot to bootloader
2. `fastboot flash bootloader <bootloader img>`
3. `fastboot reboot-bootloader`
4. `fastboot flash radio <radio img>`
5. `fastboot reboot-bootloader`
6. `fastboot flash boot boot.img`
7. `fastboot flash system system.img`
8. Reboot into recovery console.
9. Wipe data 
9. Flash the superSU zip.
9. Flash anythign else that's needed (Titanium backup for me)
9. Reboot! & 
10. Restore apps from TiBu
11. SMS and phone logs
12. Restore Nova desktop 

AND finally....

**Take a break! Have a kitkat!**

Now I just need the AOKP 4.4 nightlies....!
