Nordic ESB RF Firmware Repository
===

This repository contains the code to build the firmware for the Nordic RF devices that support the Nordic proprietary Enhanced Shock Burst (ESB) protocol.

Installing the Build Environment
---

The best way to do this is to go to the Nordic website and follow their instructions on installing the required environment.  This will require you to go to http://developer.nordicsemi.com/nRF5_SDK/nRF5_SDK_v14.x.x/.  When the page opens, click on nRF5_SDK_14.2.0_17b948a.zip to download and then expand the zip file.

Next, you'll need to install the ARM version of the gcc compiler.  You will first need to go to the ARM web site at https://developer.arm.com/open-source/gnu-toolchain/gnu-rm/downloads and download the correct version for your system.  Please be aware that there may be a problem with the latest version of the compiler.  The last version that I know of that will build the sources in this repository with out andy errors is release 7.3.1.  You can try later version, but be aware that you may get errors and that you may have to uninstall that version and go to an earlier version.

Installing Source Code
---
Once you have the build environment installed, both gcc and the Nordic SDK, you will need to install the sources from this GitHub repository.

The first thing that you will need to do is to create the following directory:
```
cd <dev_dir>/nRF5_SDK_14.2.0_17b948a/
mkdir development
cd development
```
Once you have that done, installing the sources is a simple process of going to this repository and selecting 'Clone or download', which instructs git to copy the path for into your clipboard.  Then go back to the terminal window where you created the directory above and enter the following git command (this assumes that you have git installed):
```
git clone https://github.com/JimInCA/nordic_esp.git
```
And that's it.  You should now have the sources installed on your system and they should be ready to build.

Updating Nordic source files
---

You'll first need to make a modification to one of the Nordic source files in order to get our sources to build.  You will need to open the following file and update as described:
```
<dev_dir>/nRF5_SDK_14.2.0_17b948a/components/proprietary_rf/esb/nrf_esb.h
```
Add the following #define:
```
#include "sdk_config.h"
```
Please be aware that you may also have to update the following file so that it points to the correct gcc version that you are using:
```
<dev_dir>/nRF5_SDK_14.2.0_17b948a/components/toolchain/gcc/Makefile.posix
```

Building our Source Code
---

Once you have everything installed and updated, you're ready for build our firmware.

To build the firmware, go to the following directory:
```
<dev_dir>/Nordic/nRF5_SDK_14.2.0_17b948a/development/nordic_esb/esb_transceiver/build/pca10031/armgcc
```
Building the source code is a simple matter of entering the following command at the prompt:
```
make all
```
The firmware binary will be placed in the '.\armgcc\_build' directory.


Installing the firmware
---

To install the firmware, all you will need to do is to have the dongle installed on your computer and enter the following command:
```
make flash
```
This should install the firmware on the dongle.  At least, I hope this will work.  I may have forgotten that the JLINK software needs to be installed, but I did this so long ago, that I can' remember.
