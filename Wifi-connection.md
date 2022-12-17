## Setting up the Wifi Network

### Default Settings

The first time you power up the xProV5 it will not be able to connect to your WiFi network because it does not know the SSID and password for your network. It will a create its own Wifi access point with the following settings:

- **SSID:** GRBL_ESP
- **Password:** 12345678
- **IP Address**: 192.168.0.1

Connect your PC, Tablet, etc to this network. Most computers will automatically launch a browser to connect to the xProV5 after connecting via Wifi. If it does not, simply enter **http://192.168.0.1** for the address in your browser.

This is very convenient if you are just getting started or there is no existing network to connect to. Unfortunately, this typically causes your computer to lose its Internet connection, so you probably want to setup the xProV5 to use your WiFi network.

## Connecting the xProV5 to your network.

There are 2 basic ways to do this. The easiest is via the Web. The second method uses the USB/Serial port to change some settings.

## Method #1 Using the WebUi.

Connect to the network created by xProV5 per above. Load the WebUI. Click on the ESP32 tab. Then set the 3 settings shown below.
- Station SSID: Put the SSID of your network here
- Station Password: Put the password of your network here
- Radio Mode: Change this to Client Station

<img src="https://user-images.githubusercontent.com/189677/91180709-a1698800-e6ad-11ea-8233-3aa47dd33811.png" width="400">

## Method #2: Using a serial terminal

First connect to your xProV5 using a USB [Serial Terminal](https://github.com/Spark-Concepts/xPro-V5/wiki/Serial-setup) or [USB Sender](https://github.com/Spark-Concepts/xPro-V5/wiki/USB_guide#gcode-senders)

You can use [Settings](https://github.com/bdring/Grbl_Esp32/wiki/Settings) to change the network settings. This is handy if you cannot connect and open the WebUI. [The network commands are documented here](https://github.com/bdring/Grbl_Esp32/wiki/Settings#webui-settings).

Note: If you have authentication enabled, you will need to supply a password. These examples assume it is off.

Here are the steps required to put it on your network

- $Sta/SSID=YourSSID
- $Sta/Password=YourPassword
- $Radio/Mode=STA

Note: Some senders like GrblCandle and UGS may force your SSID and Password to uppercase, which could cause authentication to fail.  The WebUI doesn't have this issue.

### Determining Current Settings

Here are some methods to determine your current [settings](https://github.com/Spark-Concepts/xPro-V5/wiki/Changing-settings) via the [USB/Serial port](https://github.com/bdring/Grbl_Esp32/wiki/Serial-Port-Setup-and-Usage). It will not tell you any passwords. If you loose the password, you will need to reset the settings with **$RST=$** or reload the firmware. 

- **$I** This will list the basic connection information
- **$System/Stats** This list Wifi stuff as well as a lot of system information 
- **$WiFi/ListAPs** This list all of the Wifi access points found and the signal strength

**Note:** By default authentication is off in firmware. If you have it enabled a second level of authentication is required to access the machine. The default user is "admin" with password "admin". This is control via **#define ENABLE_AUTHENTICATION** in config.h

### Using Smartphones.

If you try to connect a phone to the access point created by Grbl_ESP32, the phone will probably tell you that the network does not have Internet access. The phone does not like this and may try to compensate by using the mobile network as well. This can create issues with the browser in your phone not liking the ESP32 address.

The best solution is to connect the ESP32 to your home WiFi and use the phone to connect through that. This allows the phone to connect to the eSP32 and the Internet.

You can also put your phone in airplane mode with Wifi on. This helps some people.

With some advanced fiddling around, beyond the scope of this Wiki, so should be able to get any mode to work. Try the methods above first to make sure the basic connection is good. 