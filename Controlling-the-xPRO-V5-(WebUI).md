**<u>Note: Some of the screenshots are out of date and will be replaced soon</u>**



![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp32_webui_1.png)

The [WEBUI](https://github.com/luc-github/ESP3D-WEBUI) project has been modified for use by Luc of [luc-github](https://github.com/luc-github) so you can control the xPro V5 over WiFi using any browser, without needing any other programs.

Basically, the xPro V5 becomes a web server. You use a browser to access a web interface. You can then control Grbl and run jobs using only the browser. It is sort of like [OctoPrint](https://octoprint.org/), except there is no need for a Raspberry Pi. Everything runs on the xPro V5.

**Features**

- Works in either AP (access point) mode or as a client on an existing WiFi network
- Full control and monitoring of Grbl
- Supports multiple languages
- Easy control of Grbl $$ settings
- Firmware upload
- Full interface to SD card
- Easily add your own macros
- Display a camera in UI

The WEBUI project represents quite a contribution to the open source CNC world and you should consider a donation to it.

 [<img src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG_global.gif" border="0" alt="PayPal – The safer, easier way to pay online.">](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=Y8FFE7NA4LJWQ)    



##### Startup

At startup the ESP32 will try to connect to the WiFi network it was connected to last. If it cannot connect to that network, it will enter AP (Access Point) mode, thus creating a WiFi network named CNC_XPRO_V5, with password 12345678. Connect to that network with a PC, tablet or phone and use a web browser to load the WebUI to access the URL http://192.168.0.1 (you can bookmark this under the name CNC_XPRO_V5 for easy access).

**Note:** If the WebUI fails to load, open the following URL in your web browser. http://192.168.0.1/?forcefallback=yes.  From here you can re-upload the [index.html.gz](https://github.com/Spark-Concepts/xPro-V5/blob/main/src/index.html.gz)


## Dashboard

### Control Panel

![ESP32 Controls Panel](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_controls-1.png)

##### Jogging

Jogging can be done by clicking in the jogging area. The speed of the jogging is controlled by the feed rate values at the bottom of the panel.

##### Homing

Homing can be done with the little house icons. The lower left icon does a standard ($H) home of all axes. Individual axes can also be homed use their respective home buttons. 

**DROs**

The DROs (Digital Read out) are the axis values below the jog graphic. These display the current Work Coordinates. Each has a zero button next to them to zero that axis. There is also a zero xyz to zero them all.

##### Macros

This feature allows you to add custom commands. Here is an example of how to add a command to move to X0, Y0

1. Create a text file with the gcode. In this case the gcode would be "G0X0Y0". Save it with a ".g" extension. In this case I named it zeroxy.g.
2. Upload that file to the ESP3D File System (Not to the SD card)
3. Click on the macro editor button  in the controls panel. click the plus icon to add a macro. Give it a name, select a color, set the target as ESP and enter the path to the gcode file you uploaded.
4. Click Save

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_macros.png)

### Grbl Panel

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_grbl_pnl2.png)

### 

### Commands Panel

![Commands Panel](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_commands_pnl.png)

### SD Card Panel

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp3d_sd_pnl.png)

This panel allows you to upload and run files that are stored on an SD card attached to your machine. Click Refresh to show all files and folders on your SD card. Only gcode files (.txt, .nc and .gcode) will have the play icon next to them. Files are also filtered by legal characters for grbl, so files with a blank will not be able to be sent.

Refresh

Add directory

Upload

Download

Delete

Play

Filter

## Grbl Configuration ($) Panel

![](http://www.buildlog.net/blog/wp-content/uploads/2018/09/esp32_grbl_dollar.png)

This provides an easy interface to update the most common GRBL settings.  To see a list of all setting the command '$S' must be sent from the Commands Panel.  For more information on updating settings, see the Settings page.  

## ESP3D Settings

Using the ESP3D settings tab, advanced users can modify the wifi operation of the xPro V5.  

**Note** For security purposes we recommend using the xPro V5 as a local Access Point. 