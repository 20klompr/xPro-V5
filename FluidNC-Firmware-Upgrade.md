## Upgrading Firmware

FluidNC installation instructions:

- Go to the [FluidNC project page](https://github.com/bdring/FluidNC) on Github
  - Click on the [releases link](https://github.com/bdring/FluidNC/releases) on the right side of the page.
  - Click on the release that you want. You should generally use the latest non "Pre" release.
  - Download the zip file for your operating system (win64 for Windows and posix for Linux and Mac) from the Assets section of the release. You do not need to download the source code to use pre-compiled files.
  - Unzip it in a folder on your computer. Do not try to run from inside the zip file. Make sure it is fully extracted before you start. On some operating systems, like Windows, the folder should be on local drives, not a networked folder.
- Connect the ESP32 via USB. It is best to remove all other USB/Serial devices while installing it because it might try the wrong one.

*For first time FluidNC, jump to [First time install]()

- Run either install-wifi.bat or install-bt.bat (.sh on other OS's). Make sure you are running the script while in that that folder.

## First time install
1. Open FluidNC folder and Run `erase.bat`

![image](https://user-images.githubusercontent.com/8650709/229307587-6d09410c-db2f-40c3-a707-1bdfdf2550b0.png)

- For subsequent steps each and every time you see `Connecting......._____...`, press and hold the **programming button** until the load or action begins

![image](https://user-images.githubusercontent.com/8650709/229308320-3ac23f0e-a09c-4ab0-b499-8a5d21a7fc1d.png)

2. Run either `install-wifi.bat` or `install-bt.bat` (.sh on other OS's). NOTE: *Make sure you are running the script while in that that folder*

![image](https://user-images.githubusercontent.com/8650709/229308383-badb247b-0e7b-46e8-aef9-67397c15eae1.png)

3. Run `install-fs.bat` (or .sh) to install the file system, including the **WebUI**
  - If you already have a config file or other files on an ESP32, they will be deleted, so this is not recommended for upgrading firmware.
  - *You may notice a message like this* `E (38) SPIFFS: mount failed, -10025` *on the first run of the firmware. This is normal. It only happens on the first boot and is formatting the flash file system.* Note: *this operation may take a few minutes*
  - Load a [`config.yaml`](https://github.com/Spark-Concepts/xPro-V5/blob/main/FluidNC/config.yaml) file. NOTE: *spindle type default is viod and will need to be set depending on your configuration*

## Upload a new configuration `config.yaml` file
1. Plug in xPRO V5 to computer using a USB to USB-C cable
2. Open `FluidTerm.bat`
3. Select the COM port named `(\Devie\Silabser0)`

![image](https://user-images.githubusercontent.com/8650709/229311544-4698cc86-ae9e-47fc-8375-dc87b41e720b.png)

4. Press `CTRL+U` to start and upload [`config.yaml`](https://github.com/Spark-Concepts/xPro-V5/blob/main/FluidNC/config.yaml) NOTE:*by default FluidNC looks for the config.yaml file name, if you name the file something else you will need to update the configuration field in fluidNC WebUI*

![image](https://user-images.githubusercontent.com/8650709/229311578-345a206a-f175-41fb-a4f2-6c75846967a9.png)
![image](https://user-images.githubusercontent.com/8650709/229311634-28ce746b-4c79-41ba-88d8-b00cf3944637.png)

5. Press `Enter` to begin upload - if successful it should look like this:

![image](https://user-images.githubusercontent.com/8650709/229311668-4c3100ec-7b1e-4e0c-9f27-e07e99b3be33.png)

## Updating WebUI Fimware (must currently be running FluidNC)
- If the [releases notes](https://github.com/bdring/FluidNC/releases) say that the WebUI has been updated, you will need to upload `index.html.gz` from the wifi folder. Do this using the local file system panel.

![image](https://user-images.githubusercontent.com/8650709/229311874-ca4edd97-13d9-4a0d-a137-417aede4588b.png)

![image](https://user-images.githubusercontent.com/8650709/229311881-47b86a28-4dc3-4951-81fe-9a3c84464812.png)

## Over the air (OTA) Updates
If you have Wifi and the WebUI running, you can update via the FluidNC tab. This will not overwrite your config yaml file. Click the yellow cloud icon to upload your compiled binary (.bin) file. The compiled binaries are in the bt or wifi folders of downloaded releases.

![image](https://user-images.githubusercontent.com/8650709/229311918-b35b2e27-bbce-4ddb-97a1-f0cf52dcba06.png)

If the upgrade affects the WebUI, you will need to upload index.html.gz from the [FluidNC/data](https://github.com/bdring/FluidNC/tree/main/FluidNC/data) folder of the repo by clicking on the green folder icon in the image above.

### For comprehensive information about FluidNC please refer to [fluidnc.com](http://wiki.fluidnc.com/en/config/overview)