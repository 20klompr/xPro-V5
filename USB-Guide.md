# USB Cable

**The CNC xPRO V5 requires a USB-A to USB-C cable, it is recommended to use a cable with shielding and a ferrite.  USB-C to USB-C cables will not work.**  

# USB Drivers
### Windows Driver Installation
If you are unable to connect to the xPRO via USB to a gcode sender, please download the CP210x USB to UART Bridge VCP Drivers here:
- [https://www.silabs.com/documents/public/software/CP210x_Universal_Windows_Driver.zip](https://www.silabs.com/documents/public/software/CP210x_Universal_Windows_Driver.zip)

Please extract the file and run:
- **```P210xVCPInstaller_x64``` if you have a 64 bit computer**
- **```CP210xVCPInstaller_x86``` if you have a 32 bit computer**

### Mac OSX Driver Installation

Release notes and Legacy Drivers for **MacOS 10.5, 10.6, 10.7, 10.8, 10.9, 10.10 and 10.11:**
- [Release Notes](http://www.silabs.com/Support%20Documents/Software/Mac_OSX_VCP_Driver_10_6_Release_Notes.txt)
- [http://www.silabs.com/Support%20Documents/Software/Mac_OSX_VCP_Driver_10_6.zip](http://www.silabs.com/Support%20Documents/Software/Mac_OSX_VCP_Driver_10_6.zip)

Release notes and Drivers for **MacOS 10.11 or greater:**
- [Release Notes](https://www.silabs.com/documents/public/release-notes/Mac_OSX_VCP_Driver_Release_Notes.txt)
- [https://www.silabs.com/documents/public/software/Mac_OSX_VCP_Driver.zip](https://www.silabs.com/documents/public/software/Mac_OSX_VCP_Driver.zip)

### Linux Drivers

- [Linux 2.6.x VCP Revision History](https://www.silabs.com/documents/public/release-notes/Linux_CP210x_VCP_2.6.x_Release_Notes.txt)
- [Linux 3.x.x/4.x.x/5.x.x VCP Driver](https://github.com/Spark-Concepts/xPro-V5/tree/main/Drivers)

### Driver source links: 

_for all legacy OS software and driver packages see:_
   - _[SiLabs Legacy OS Drivers](https://www.silabs.com/community/interface/knowledge-base.entry.html/2017/01/10/legacy_os_softwarea-bgvU)_

_for the most current drivers see:_ 
   - _[SiLabs VCP Drivers](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers#software)_

# GCode Senders

**We recommend using CNCjs Desktop to control the CNC xPro V5, however, the other Gcode Senders listed below will work as well**    
***

## CNCJS

To make the xPro V5 reboot on connection (recommended for USB use), select the "Enable hardware flow control" option.

**Download it here:** 
- 64bit computer: 
[https://github.com/cncjs/cncjs/releases/download/v1.9.22/cncjs-app-1.9.22-win-x64.exe](https://github.com/cncjs/cncjs/releases/download/v1.9.22/cncjs-app-1.9.22-win-x64.exe)
- 32Bit computer: 
[https://github.com/cncjs/cncjs/releases/download/v1.9.22/cncjs-app-1.9.22-win-ia32.exe](https://github.com/cncjs/cncjs/releases/download/v1.9.22/cncjs-app-1.9.22-win-ia32.exe) 
- Mac and Linux users, or for latest revisions, please visit: [https://github.com/cncjs/cncjs/releases](https://github.com/cncjs/cncjs/releases)

<img src="https://cloud.githubusercontent.com/assets/447801/24392019/aa2d725e-13c4-11e7-9538-fd5f746a2130.png" width="400">

* [Website](https://cnc.js.org/) 

## Universal Gcode Sender

<img src="http://winder.github.io/ugs_website/img/platform/screenshot.png" width="400">

* [Website](http://winder.github.io/ugs_website/)

## LaserGRBL

* [Website](http://lasergrbl.com/en/)

## LaserWeb4

* [Github](https://github.com/LaserWeb/LaserWeb4)
* [Website](https://cncpro.yurl.ch/)

## Candle

![](https://github.com/Denvi/Candle/raw/master/screenshots/screenshot_heightmap_original.png)

* [Website](https://github.com/Denvi/Candle)

## Grbl-Plotter

<img src="https://github.com/svenhb/GRBL-Plotter/raw/master/doc/GRBLPlotter_GUI.png" width="400">

* [Website](https://github.com/svenhb/GRBL-Plotter)
