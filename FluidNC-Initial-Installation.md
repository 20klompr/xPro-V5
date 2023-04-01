## Upgrading Firmware
To update firmware, but do not install the file system.

Go to the [FluidNC project page](https://github.com/bdring/FluidNC) on Github
Click on the [releases link](https://github.com/bdring/FluidNC/releases) on the right side of the page.
Click on the release that you want. You should generally use the latest non "Pre" release.
Download the zip file for your operating system (win64 for Windows and posix for Linux and Mac) from the Assets section of the release. You do not need to download the source code to use pre-compiled files.
Unzip it in a folder on your computer. Do not try to run from inside the zip file. Make sure it is fully extracted before you start. On some operating systems, like Windows, the folder should be on local drives, not a networked folder.
Connect the ESP32 via USB. It is best to remove all other USB/Serial devices while installing it because it might try the wrong one.
Run either install-wifi.bat or install-bt.bat (.sh on other OS's). Make sure you are running the script while in that that folder.
If you are doing a first time install, run install-fs.bat (or .sh) to install the file system, including the WebUI. If you already have a config file or other files on an ESP32, they will be deleted, so this is not recommended for upgrading firmware.
You may notice a message like this E (38) SPIFFS: mount failed, -10025 on the first run of the firmware. This is normal. It only happens on the first boot and is formatting the flash file system. It could take a long time on ESP32s with larger flash memories.
You now need to load a config file. The instructions on how to do this
