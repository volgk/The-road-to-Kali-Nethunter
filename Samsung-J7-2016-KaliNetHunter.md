Porting a Kali Nethunter to Samsung J7 2016
===========================================

Here I will briefly describe the process of porting a Kali Nethunter to
Samsung J7 2016 (j7xelte) device.

If you want just to install Nethunter on your device, without repeating
the whole process, just skip the build steps.


## The items that led to Kali NetHunter for Samsung J7 2016 (SM-J710N)

1. [ADB](https://github.com/volgk/The-road-to-Kali-Nethunter/blob/master/Samsung-J7-2016-KaliNetHunter.md#1-adb)
1. Heimdall
1. TWRP
1. Custom ROM
1. SuperSU
1. BusyBox
1. Kali Nethunter kernel
1. Kali Nethunter build
1. Kali Nethunter install

---
## 1. ADB

**1. Download Android sdk command line tools for Linux**
* Link:		http://developer.android.com/sdk/index.html
* Crux port:	https://github.com/non-yellow-spot/vccrux/tree/3.5/android-sdk-platform-tools-bin
	
**2. Assuming adb is installed. Now you need to config**
* Enable USB debugging
	* Settings -> About phone -> Software information
	* Press 7 times the Build number
	* Go back to Settings
	* Developer options -> USB debugging
* Connect the device to USB and check if it's visible for adb

		$ adb devices
	 	List of devices attached
		5203b1e2600d94fb        no permissions; see [http://developer.android.com/tools/device.html]
	
* Write /etc/udev/rules.d/10-adb.rules to give permisions to device
	* Check the device information for 10-adb.rules file:
	
			$ lsusb
			Bus 001 Device 009: ID 04e8:6860 Samsung Electronics Co., Ltd Galaxy series, misc. (MTP mode)
	
	* Write /etc/udev/rules.d/10-adb.rules using id numbers
			
		  SUBSYSTEM=="usb", ATTR{idVendor}=="04e8", ATTR{idProduct}=="6860", ENV{GPHOTO2_DRIVER}="proprietary", ENV{ID_MEDIA_PLAYER}="1", MODE="0666", GROUP="plugdev"
		  
	* Change file permissions
		
			$ sudo chmod a+r /etc/udev/rules.d/10-adb.rules
		
	* Add a new group and add your user
	
			$ sudo groupadd plugdev
			$ sudo usermod -a -G plugdev $(whoami)
			$ newgrp plugdev
			$ groups
			**** **** ******* ****** plugdev
		  
	* Restart udev rules
	
			$ sudo udevadm control --reload-rules	
		
	* Replug the device
	* Allow USB Debugging on your device
		* Always allow from this computer -> OK
	* Restart adb server
	
			$ adb kill-server
			$ adb start-server
			$ adb devices
		  	List of devices attached
		  	5203b1e2600d94fb        device
		  
	* Trobleshoots with permissions:
		https://exceptionshub.com/android-debug-bridge-adb-device-no-permissions.html
---
## 2. Heimdall
* Download heimdall:
	* Link:	    https://glassechidna.com.au/heimdall/
	* Crux port:  https://github.com/non-yellow-spot/vccrux/tree/3.5/heimdall
* Connect the USB cable to device
* Assuming heimdall is instaled
	* For device visibility it must be boot in Download Mode
		* Power off
		* Press on the same time Down Volume Button + Power Button + Home Button 
		* After you see the blue screen with Warning message  press Up Volume Button to boot in 'Download Mode'
	* Connect the device with USB
	* Check if device is detected
			
			$ sudo heimdall detect
		  	Device detected
---
## 3. TWRP
* Download TWRP image specific for jxelte device
	* Link:	       		https://dl.twrp.me/j7xelte/twrp-3.2.1-0-j7xelte.img.html
	* Backup mirror:	https://drive.google.com/file/d/1WWcq1CgtJESn1rjvR9zfs8RY_9-kPDa1/view?usp=sharing
	* Md5sum:		b189622eb84f0f8bd3b8134ee8b88268
* Check adb devices
	
		$ adb devices
	        List of devices attached
	        5203b1e2600d94fb        device
		
* OEM must be unloked
	* Settings -> Developer options -> OEM Unlock

> WARNING:     If OEM was not unlocked you can't boot again !!!

* Boot to 'Download mode'
	* Power off
	* Press on the same time Down Volume Button + Power Button + Home Button 
	* After you see the blue screen with Warning message  press Up Volume Button to boot into 'Download Mode'
	* Connect device with USB
* Install TWRP
	* Check if device is detected
	
			$ sudo heimdall detect
			Device detected
		
	* Flash TWRP in recovery partition
		
			$ sudo heimdall flash --RECOVERY ~/twrp-3.2.1-9-j7xelte.img
			Heimdall v1.4.2
		  	Initialising connection...
		  	Detecting device...
		  	Claiming interface...
		  	Setting up interface...

		  	Initialising protocol...
		  	Protocol initialisation successful.

		  	Beginning session...

		  	Some devices may take up to 2 minutes to respond.
		  	Please be patient!

		  	Session begun.

		  	Downloading device's PIT file...
		  	PIT file download successful.

		  	Uploading RECOVERY
		  	100%
		  	RECOVERY upload successful

		  	Ending session...
		  	Rebooting device...
		  	Releasing device interface...
		  
* Open TWRP
	* With adb
		
			$ adb reboot recovery
		
	* Manual
		* Power off the device
		* Press Up Volume Button + Power Button + Home Button until
		  you see the logo.
---
## 4. Custom ROM

> We use a REFINED NOTE8 ROM by Mohit Mallick

* Download the ROM
	* Link:			https://forum.xda-developers.com/showpost.php?p=76847532&postcount=280
	* Backup mirror:	https://drive.google.com/open?id=1G-xK_6W-3WR9bxFRv5sUEBgn9vizu_4-
	* Md5sum:		9a6f5cbde550d4a1f084f44b72df3efe

>	NOTE: Please, use the XDA link, as the developer ask.
>	A mirror is needed only in case the original link is not available.
	
* Flash the ROM with TWRP
	* Move the ROM zip in Micro SD card and plug it in the device
	* If you plug the SD card after you open TWRP, do:
		* Mount -> Micro SD card
	* Install -> (Up A Level) -> external_sd -> ROM.zip -> Install Zip -> Swipe to confirm Flash
	
>	Since it's alredy rooted with Magistik, you don't need to install
>	SuperSU. Skip the 5 step. Else, in case you want to use your custom
>	ROM you need to root it with SuperSU.
---
## 5. SuperSU
* Download SuperSU
	* Link:			https://download.chainfire.eu/696/supersu/
	* Backup mirror:	https://drive.google.com/open?id=1SnfcYPrhvvN21XVlaIqYBQb7Ye0GBYvK
	* Md5sum:		332de336aee7337954202475eeaea453
* Move the SuperSU zip file in Micro SD card  and plug it in the device
	* If you plug the SD card after you open TWRP, do 
		* Mount -> Micro SD card
* Flash the SuperSU zip file with TWRP
	* Install -> (Up A Level) -> external_sd -> UPDATE-SuperSU-v2.46.zip -> Install Zip -> Swipe to confirm Flash
	* Reboot System
---
## 6. BusyBox
* Download from github the last version:
	* Releases:		https://github.com/meefik/busybox/releases
	* Backup mirror:	https://drive.google.com/open?id=1d4MjtWBYo51wK90X3mNH0vE4wbba9C4D
	* Md5sum:		e9c197251ee35ef354f6561c9efe6c06		
* Install with adb
	
		$ adb install busybox-1.31.1-46.apk
		
* Or install from Google Apps Store
---
## 7. Kali NetHunter kernel build
* Clone the repository
	
		$ git clone -b tw-nougat https://github.com/chinarulezzz/j7xelte-nethunter.git 
		$ cd j7xelte-nethunter
		
	* Backup mirror:	https://github.com/volgk/j7xelte-nethunter
* Download toolchain64:
			
		git clone https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9 -b  marshmallow-release toolchain64
		
* Modify the make wrapper conform your needs
  		
		$ vim make
		
* Build the kernel and the modules
	
		$ sudo ./make clean mrproper
		$ sudo ./make nethunter_j7xelte_defconfig
		$ sudo ./make -j$(nproc)

---
## 8. Kali Nethunter build
>	NOTE: If you want to install without manual building go to step 9
* Download kali-nethunter-project
	
		$ git clone https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-project
        	$ cd kali-nethunter-project/nethunter-installer
		
* Run ./bootstrap.sh script and answer the questions
		
		$ ./bootstrap.sh
	        Would you like to use the experimental devices branch? (y/N): n
	        Would you like to grab the full history of devices? (y/N): y
	        Would you like to use SSH authentication (faster, but requires a GitHub account with SSH keys)? (y/N): n
		$ cd devices
		
* Add your device to devices.cfg	
		
		\# Samsung J7 2016
		[j7xelte]
		author = "Stamatin Cristina"
		version = "1.0"
		kernelstring = "NetHunter Kernel for SM-J710F"
		arch = arm64
		devicenames = j7xelte SM-J710F
		
	* Link:	https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-devices
* Add the directory devices/nougat/j7xelte
	
		$ mkdir devices/nougat/j7xelte
		
* Where put the kernel, that we build early
	
		$ cp ~/j7xelte-nethunter/arch/arm64/boot/Image ~/kali-nethunter-project/nethunter-installer/devices/nougat/j7xelte/Image-dtb
		
* Where put the modules
	* Go to the j7xlete-nethunter directory to install the modules 
		
			$ cd ~/j7xelte-nethunter
			$ sudo ./make modules_install INSTALL_MOD_PATH=~/kali-nethunter-project/nethunter-installer/devices/nougat/j7xelte/

	* Move the lib/modules directory
	
			$ cd ~/kali-nethunter-project/nethunter-installer/devices/nougat/j7xelte/
			$ sudo mv lib/modules .
			$ rm -rf lib
			$ ls
			Image-dtb modules
			
* Build the NetHunter zip
	* See the help message
	
			$ cd ~/kali-nethunter-project/nethunter-installer
			$ python build.py -h
	* To try nethunter with your old kernel do
		
			$ python build.py -d j7xelte -n -fs minimal/full -nk
			
	* To try nethunter with the new kernel do
	
			$ python build.py -d j7xelte -n -fs minimal/full
	* To change just the kernel do 
		
			$ python build.py -d j7xelte -n -k
			
* Link:	https://www.kali.org/docs/nethunter/building-nethunter/
---
## 9. Install Kali NetHunter
* The order of items for succesfull installation:
	* Cusom ROM:		installed
	* Magistik/SuperSU:  	installed
	* BusyBox:		installed
* Now flash Kali NetHunter zip with TWRP
	* Minimal rootfs:
		* Link:		https://drive.google.com/open?id=19X5F7nmroNMHynOpHOAe4C8A3xeCpq2q
		* Md5sum:	4e60fa67740ce3b8f72b77fc6f6e6e94
	* Full rootfs:
		* Link:		https://drive.google.com/open?id=1sKE-fCJqoRFGr5SwHUCMTQqQRKP22dWY
		* Md5sum:	0c0bc2fa99395a5c0b487933f9c304aa
* If you want to return the old kernels:
	* Oxygen Kernel
		* Link:		https://drive.google.com/open?id=1-CXf5AxAMT64B6wFagq5N8gFBD03H0hL
		* Md5sum:	db9474e3af52bc5f4ba21257645e82e2
	* Polonium Kernel
		* Link:		https://drive.google.com/open?id=1aOm2k6droeFIS8895PyrE4Fcmj0uBieS
		* Md5sum: 	6b32d0804d1752a786959067e76e6f3b
* Compiled Nethunter Oxygen Kernel
	* Link:   https://drive.google.com/open?id=1nQ_I9obL1NyH2DL3a_BjLb_jXZEi7G_3
	* Md5sum: b7ac2352d081cf4d412efcf129c1e5cc
* Bugs
	* Wireless Attacks 
		* The Wireless Card of your device, with driver bcm43430
		   don't support the Monitor Mode, so Wireless Attaks are impossible.
		* A solution is to use a Wireless USB Adapter, but I can't check it yet because I don't have the right cable.
		* Link: https://www.kali.org/docs/nethunter/wireless-cards/
