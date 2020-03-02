Porting a Kali Nethunter to Samsung J7 2016
===========================================

Here I will briefly describe the process of porting a Kali Nethunter to
Samsung J7 2016 (j7xelte) device.

If you want just to install Nethunter on your device, without repeating
the whole process, just skip the build steps.


## Prepare

The items that led to Kali NetHunter for Samsung J7 2016 (SM-J710N)

I.    Adb
II.   Heimdall
III.  TWRP
IV.   Custom ROM
V.    SuperSU
VI.   BusyBox
VII.  Kali Nethunter kernel
VIII. Kali Nethunter build
IV.   Kali Nethunter install

## Here goes!

I. ADB

  1. Download Android sdk command line tools for Linux
	Link:	    http://developer.android.com/sdk/index.html
	Crux port:  https://github.com/non-yellow-spot/vccrux/tree/3.5/android-sdk-platform-tools-bin
  2. Assuming adb is installed. Now you need to config:
	2.1. Enable USB debugging
		- Settings -> About phone -> Software information
		- Press 7 times the Build number
		- Go back to Settings
		- Developer options -> USB debugging
	2.2. Connect the device to USB and check if it's visible for adb
		$ adb devices
		  List of devices attached
	          5203b1e2600d94fb        no permissions; see [http://developer.android.com/tools/device.html]
	2.3. Write /etc/udev/rules.d/10-adb.rules to give permisions to device
		- Check the device information for 10-adb.rules file:
			$ lsusb
			  Bus 001 Device 009: ID 04e8:6860 Samsung Electronics Co., Ltd Galaxy series, misc. (MTP mode)
		- Write /etc/udev/rules.d/10-adb.rules using id numbers
			SUBSYSTEM=="usb", ATTR{idVendor}=="04e8", ATTR{idProduct}=="6860", ENV{GPHOTO2_DRIVER}="proprietary", ENV{ID_MEDIA_PLAYER}="1", MODE="0666", GROUP="plugdev"
	2.4. Change file permissions
		$ sudo chmod a+r /etc/udev/rules.d/10-adb.rules
	2.5. Add a new group and add your user
		$ sudo groupadd plugdev
		$ sudo usermod -a -G plugdev $(whoami)
		$ newgrp plugdev
		$ groups
		  **** **** ******* ****** plugdev
	2.6. Restart udev rules
		$ sudo udevadm control --reload-rules
	2.7. Replug the device
	2.8. Allow USB Debugging on your device
		- Always allow from this computer -> OK
	2.9. Restart adb server
		$ adb kill-server
		$ adb start-server
		$ adb devices
		  List of devices attached
		  5203b1e2600d94fb        device
	2.10. Trobleshooting with permissions:
		https://exceptionshub.com/android-debug-bridge-adb-device-no-permissions.html


II. Heimdall

  1. Download heimdall:
	Link:	    https://glassechidna.com.au/heimdall/
	Crux port:  https://github.com/non-yellow-spot/vccrux/tree/3.5/heimdall
  2. Connect the USB cable to device
  3. Assuming heimdall is instaled
	3.1. For device visibility it must be in Download Mode(III.4)
	3.2. Check if device is detected
		$ sudo heimdall detect
		  Device detected


III. TWRP

  1. Download TWRP image specific for jxelte device
	Link:	        https://dl.twrp.me/j7xelte/twrp-3.2.1-0-j7xelte.img.html
	Backup mirror:	https://drive.google.com/file/d/1WWcq1CgtJESn1rjvR9zfs8RY_9-kPDa1/view?usp=sharing
	Md5sum:		b189622eb84f0f8bd3b8134ee8b88268
  2. Check adb devices
	2.1. $ adb devices
	       List of devices attached
	       5203b1e2600d94fb        device
  3. OEM must be unloked
	3.1. Settings -> Developer options -> OEM Unlock

	WARNING:     If OEM was not unlocked you can't boot again !!!

  4. Boot to 'Download mode'
	4.1. Power off
	4.2. Press on the same time Down Volume Button + Power Button + Home Button 
	4.3. After you see the blue screen with Warning message  press 
	     Up Volume Button to boot into 'Download Mode'
	4.4. Connect device with USB
  5. Install TWRP
	5.1. Check if device is detected
		$ sudo heimdall detect
		  Device detected
	5.2. Flash TWRP in recovery partition
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
  6. Open TWRP
	6.1. With adb
		$ adb reboot recovery
	6.2. Manual
		- Power off the device
		- Press Up Volume Button + Power Button + Home Button until
		  you see the logo.


IV. Custom ROM

	We use a REFINED NOTE8 ROM by Mohit Mallick

  1. Download the ROM
	Link:		https://forum.xda-developers.com/showpost.php?p=76847532&postcount=280
	Backup mirror:	https://drive.google.com/open?id=1G-xK_6W-3WR9bxFRv5sUEBgn9vizu_4-
	Md5sum:		9a6f5cbde550d4a1f084f44b72df3efe

	NOTE: Please, use the XDA link, as the developer ask.
	A mirror is needed only in case the original link is not available.
	
  2. Flash the ROM with TWRP (See items V.2, V.3)
	Since it's alredy rooted with Magistik, you don't need to install
	SuperSU. Skip the V step. Else, in case you want to use your custom
	ROM you need to root it with SuperSU.


V. SuperSU

  1. Download SuperSU
	Link:		https://download.chainfire.eu/696/supersu/
	Backup mirror:	https://drive.google.com/open?id=1SnfcYPrhvvN21XVlaIqYBQb7Ye0GBYvK
	Md5sum:		332de336aee7337954202475eeaea453
  2. Move the SuperSU zip file in Micro SD card  and plug it in the device
	2.1. If you plug the SD card after you open TWRP, do 
		- Mount -> Micro SD card
  3. Flash the SuperSU zip file with TWRP
	3.1. Open TWRP -> Install -> (Up A Level) -> external_sd -> UPDATE-SuperSU-v2.46.zip -> Install Zip -> Swipe to confirm Flash
	3.2. Reboot System


VI. BusyBox

  1. Download from github the last version:
	Releases:	https://github.com/meefik/busybox/releases
	Backup mirror:	https://drive.google.com/open?id=1d4MjtWBYo51wK90X3mNH0vE4wbba9C4D
	Md5sum:		e9c197251ee35ef354f6561c9efe6c06		
  2. Install with adb
	$ adb install busybox-1.31.1-46.apk
  3. Or install from Google Apps Store


VII. Kali NetHunter kernel

  1. Clone the repository
	Backup mirror:	https://github.com/volgk/j7xelte-nethunter
	$ git clone -b tw-nougat https://github.com/chinarulezzz/j7xelte-nethunter.git 
	$ cd j7xelte-nethunter
  2. Modify the make wrapper conform your needs
  	$ vim make
  3. Build the kernel and the modules
	$ sudo ./make clean mrproper
	$ sudo ./make nethunter_j7xelte_defconfig
	$ sudo ./make -j$(nproc)


VIII. Kali Nethunter build

	NOTE: If you want to install without manual building go to IX step.

  1. Download kali-nethunter-project
	$ git clone https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-project
        $ cd kali-nethunter-project/nethunter-installer
  2. Run ./bootstrap.sh script and answer the questions
	2.1. $ ./bootstrap.sh
	       Would you like to use the experimental devices branch? (y/N): n
	       Would you like to grab the full history of devices? (y/N): y
	       Would you like to use SSH authentication (faster, but requires a GitHub account with SSH keys)? (y/N): n
  3. Add your device to devices.cfg
	3.1. Write devices/devices.cfg	
		# Samsung J7 2016
		[j7xelte]
		author = "Stamatin Cristina"
		version = "1.0"
		kernelstring = "NetHunter Kernel for SM-J710F"
		arch = arm64
		devicenames = j7xelte SM-J710F
	3.2. Link for more information
		https://gitlab.com/kalilinux/nethunter/build-scripts/kali-nethunter-devices
  4. Add the directory devices/nougat/j7xelte
	4.1. $ mkdir devices/nougat/j7xelte
  5. Where put the kernel, that we build early
	5.1. $ cp ~/j7xelte-nethunter/arch/arm64/boot/Image ~/kali-nethunter-project/nethunter-installer/devices/nougat/j7xelte/Image-dtb
  6. Where put the modules
	6.1. Go to the j7xlete-nethunter directory to install the modules 
		$ cd ~/j7xelte-nethunter
		$ sudo ./make modules_install INSTALL_MOD_PATH=~/kali-nethunter-project/nethunter-installer/devices/nougat/j7xelte/
	6.2. Move the lib/modules directory
		$ cd ~/kali-nethunter-project/nethunter-installer/devices/nougat/j7xelte/
		$ sudo mv lib/modules .
		$ rm -rf lib
  7. Build the nethunter zip
	7.1. See the help message
		$ cd ~/kali-nethunter-project/nethunter-installer
		$ python build.py -h
	7.2. To try nethunter with your old kernel do
		$ python build.py -d j7xelte -n -fs minimal/full -nk
	7.3. To try nethunter with the new kernel do
		$ python build.py -d j7xelte -n -fs minimal/full
	7.4. To change just the kernel do 
		$ python build.py -d j7xelte -n -k
  8. Link for more information 
	8.1. https://www.kali.org/docs/nethunter/building-nethunter/


IX. Install Kali NetHunter
  1. The order of items for succesfull installation:
	1.1. Cusom ROM:		installed
	1.2. Magistik/SuperSU:  installed
	1.3. BusyBox:		installed
  2. Now flash Kali NetHunter zip with TWRP
	2.1. Minimal rootfs:
		Link:   https://drive.google.com/open?id=19X5F7nmroNMHynOpHOAe4C8A3xeCpq2q
		Md5sum: 4e60fa67740ce3b8f72b77fc6f6e6e94
	2.2. Full rootfs:
		Link:	https://drive.google.com/open?id=1sKE-fCJqoRFGr5SwHUCMTQqQRKP22dWY
		Md5sum:	0c0bc2fa99395a5c0b487933f9c304aa
  3. If you want to return the old kernels:
	3.1. Oxygen Kernel
		Link:   https://drive.google.com/open?id=1-CXf5AxAMT64B6wFagq5N8gFBD03H0hL
		Md5sum: db9474e3af52bc5f4ba21257645e82e2
	3.2. Polonium Kernel
		Link:	https://drive.google.com/open?id=1aOm2k6droeFIS8895PyrE4Fcmj0uBieS
		Md5sum: 6b32d0804d1752a786959067e76e6f3b
  4. Compiled Nethunter Oxygen Kernel
		Link:   https://drive.google.com/open?id=1nQ_I9obL1NyH2DL3a_BjLb_jXZEi7G_3
		Md5sum: b7ac2352d081cf4d412efcf129c1e5cc
  5. Bugs
	5.1. Wireless Attacks 
		- The Wireless Card of your device, with driver bcm43430
		   don't support the Monitor Mode.
		- There is the list of supported cards and the bypass of the
		  power problems:
			https://www.kali.org/docs/nethunter/wireless-cards/
