Porting Kali Nethunter to LG K7 (X210DS)
========================================

<img src="https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/images/LG_Kali_NetHunter.jpg" width="300" align="right" style="float" />

## The items that led to Kali Nethunter for LG

<p align="left">
	
1. [ADB](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/LG-X210DS-KaliNetHunter.md#adb)
1. [TWRP](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/LG-X210DS-KaliNetHunter.md#twrp)
1. [SuperSU](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/LG-X210DS-KaliNetHunter.md#supersu)
1. [BusyBox](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/LG-X210DS-KaliNetHunter.md#busybox)
1. [Kali Nethunter build](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/LG-X210DS-KaliNetHunter.md#kali-nethunter-build)
1. [Kali Nethunter install](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/LG-X210DS-KaliNetHunter.md#install-kali-nethunter)
1. [SP Flash Tool and Custom ROM](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/LG-X210DS-KaliNetHunter.md#sp-flash-tool-and-custom-rom)

</p>

---

## ADB

* Assuming adb and sdk tools are installed
	* If not:
		* Link:	http://developer.android.com/sdk/index.html
*  Configure your machine and device for sdk tools
	* Enable USB debugging
		* Settings -> About phone
		* Press 7 times the Build number
		* Go back to Settings
		* Developer mode -> USB debugging
		* Developer mode -> Verify apps over USB
	* Connect the device to USB and check if it is visible for adb
		
			$ adb devices
			GUU85DVONB8PAQSC	no permissions; see [http://developer.android.com/tools/device.html]

	* See the device informations:
		
			$ lsusb
			Bus 002 Device 112: ID 0e8d:201d MediaTek Inc. K7(X210DS)
	* Write id numbers  in /etc/udev/rules.d/10-adb.rules
		
			SUBSYSTEM=="usb", ATTR{idVendor}=="0e8d", ATTR{idProduct}=="201d", ENV{GPHOTO2_DRIVER}="proprietary", ENV{ID_MEDIA_PLAYER}="1", MODE="0666", GROUP="plugdev"

	* Change file permissions
			
			$ sudo chmod a+r /etc/udev/rules.d/10-adb.rules

	* Add a new group and add your user in that group
		
			$ sudo groupadd plugdev
			$ sudo usermod -aG plugdev $(whoami)
			$ newgrp plugdev
			$ groups
			..... ..... ...... ... plugdev

	* Restart udev rules
		
			$ sudo udevadm control --reload-rules

	* Replug the device with USB
	* Allow USB Debugging on your device
		* Always allow from this computer -> OK
	*  Restart adb server
	
			$ adb kill-server
			$ adb devices
			List of devices attached
			GUU85DVONB8PAQSC        device
	* Troleshoots with permissions
		* Link:	https://exceptionshub.com/android-debug-bridge-adb-device-no-permissions.html
---
## TWRP
* Download TWRP image specific for LG K7
	* Backup mirror:	https://drive.google.com/open?id=1QEGxB0hF6guvaRZtuKn4SKgwDkZZ-2A3	
	* Md5sum:			11d31af279b31c6238663172cae1a308		
* Unlock OEM
	* Unlock OEM in Developers mode
		* Settings -> Developers mode -> Unlock OEM
	* After that, unlock OEM in fastboot mode
		* Go in fastboot mode:
			* With adb:
				
					$ adb reboot bootloader

			* Manually:
				* Power off
				* Press in the same time 
				  Volume Up Button + Power Button until you see
				  the message "Select Boot Mode:"
				* With Volume Up Button you can select, and
				  with Volume Down Button you confirm the
				  selected item
				* Select [ Fastboot Mode ]
		* Check if the device is visible for fastboot (sdk tool)
			
				$ sudo fasboot devices
				GUU85DVONB8PAQSC        fastboot

		* Unlock OEM
			
				$ sudo fastboot oem unlock

			press Volume Up Button without touch the USB cable

				OKAY [ 79.139s]
				Finished. Total time: 79.191s

* Flash TWRP image in recovery partition
	
		$ sudo fasboot flash recovery ~/twrp_LGx210.img
		  Sending 'recovery' (12030 KB)                      OKAY [  0.487s]
		  Writing 'recovery'                                 OKAY [  1.609s]
		  Finished. Total time: 2.104s

* Take off the battery and putt it back
* Open TWRP
	* With adb
		
			$ adb reboot recovery

	* Manualy
		* Power off
 		* Press in the same time Volume Up Button + Power Button until
		  you'll see the message "Select Boot Mode"
		* Select [ Recovery Mode ]
---
## SuperSU
* Download SuperSU
	* Link:		    https://download.chainfire.eu/696/supersu/
	* Backup mirror:    https://drive.google.com/open?id=1SnfcYPrhvvN21XVlaIqYBQb7Ye0GBYvK
	* Md5sum:	    332de336aee7337954202475eeaea453
* Move SuperSU zip in the Micro SD card
	* If you put the Micro SD card in device after you open TWRP:
		* Mount -> Miscro SD card
* Flash SuperSU zip 
	* Install -> (Up A Level) -> external_sd -> UPDATE-SuperSU-v2.46.zip -> Install Zip -> Swipe to confirm Flash
* Reboot System
* If you have this error 
		
		Failed to mount '/data' (Invalid argument)
		...done
		Unable to mount storage
		
	* Wipe -> Advanced Wipe -> Data -> Repair or Change File System -> Change File System -> EXT2 -> Swipe to Change
	* Reflash SuperSU zip file
---
##  BusyBox
* Download the last version from github
	* Rekeases:	    https://github.com/meefik/busybox/releases
	* Backup mirror:    https://drive.google.com/open?id=1d4MjtWBYo51wK90X3mNH0vE4wbba9C4D
	* Md5sum:	    e9c197251ee35ef354f6561c9efe6c06
* Install with adb
	
		$ adb install ~/busybox-1.31.1-46.apk

* Or install from Google Apps Store
---
## Kali NetHunter build

>   NOTE: If you want to install Kali NetHunter without building go to next step.

* Download kali-nethunter-project
	
		$ git clone https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-project
		$ cd kali-nethunter-project/nethunter-installer

* Help message
		
			$ python build.py -h

>   NOTE:   Full rootfs is to much for this device. So use minimal rootfs.

>	NOTE:	If build NetHunter with boot animation and wallpaper, you cannot boot. So build without.
	
* Build NetHunter
		
		$ python build.py -g armhf -l -fs minimal -nb

* Link:	https://www.kali.org/docs/nethunter/building-nethunter/
---
## Install Kali NetHunter
* The order of items for succesful installation:
	* SuperSU:  installed
	* BusyBox:  installed
* Now flash Kali NetHunter zip file with TWRP
	* Link:	https://drive.google.com/open?id=145z0h5bkm3EQgCHx_185cEowIU2KU-Rz
	* Md5sum:	f1769dd5af172ddb475b3ad63386d6fa
---
## SP Flash Tool and Custom ROM

> NOTE:	This tool is usefull when you make something wrong, and the right option is to start from begin.

* Download SP Flash Tools for Linux 
	* Link:		https://spflashtool.com/download/
	* Crux port:	https://github.com/non-yellow-spot/vccrux/tree/3.6/sp-flash-tool-bin
* Assuming SP Flash is installed. Open it
	
	$ ./flash_tool

* Download Custom ROM and unpack it	    
	* Backup mirror:	https://drive.google.com/open?id=1GKMQKCn9ss3NBN7HfBjkz08UAeQuh3u4  
	* Md5sum:			b580e9a47fe85d6778b6843fa387737a
	
	* Backup mirror:	https://drive.google.com/open?id=15CpiL2avVypQ_4XZZhNMVwdrnH2j8K5u
	* Md5sum:			aa7613e727ed02b9631e7f9850a53026
* In 'Scatter-loading File' chose the *_scatter.txt file from unpacked ROM
* Unmark 'preloader'
* Be sure is on 'Download Only' mode
* Press Download 
* Make sure your device is Poewr off and unpluget from USB cable
* Take pressed Volume Down Button and replug the device until you'll see that download begin.
>   NOTE:   Be careful, don't tuch the USB cable and don't intrerrupt the download process.
* Reboot.
* Tutorial link:	https://androidmtk.com/flash-stock-rom-using-smart-phone-flash-tool
