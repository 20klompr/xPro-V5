The USB serial port is most basic connection. It can fully control the xProV5 and it also sends useful data at startup and if there is a crash. 

The default settings are **115200 baud, N-8-1**. One of the easiest serial terminals to use is the one that comes with the Arduino IDE. You get to it via the magnifying glass icon and you need to adjust the settings in the lower right per the image below.

![](http://www.buildlog.net/blog/wp-content/uploads/2019/10/serial_console.png)

If you have the serial terminal open before you compile/upload, it will connect and show you some helpful information.

It will show you the cpu map you are using and some of the features associated with it. You can send **$I** to get the version number.

If you want to reboot the xProV5 (ESP32) to see the startup info, send **[ESP444]RESTART** or click the recessed program button next to the USB-C port on your xProV5.

### Serial Port Rebooting ESP32 (Technical Details)

In order for you to be able to manually program an ESP32 through the serial port. The serial port must be able to reboot the xProV5's ESP32 processor and tell it to enter bootloader mode. To do this hold the program button depressed and toggle the reset button.

This does this using this sequence:

- "program" = released, "reset" = momentarily pressed -> Reset the chip
- "program" = pressed while "reset" = momentarily pressed -> Switch to boot mode
- "program" = released, "reset" = released -> normal operation

If your serial terminal does the first step of this when opening a connection, it will reboot the ESP32. If you are trying to connect to a running ESP32 and don't want to reboot it, you must make sure the serial terminal does not do this.

- Arduino IDE (Windows) may not reboot the ESP32
- PlatformIO (Windows) Inconsistent, but usually reboots the ESP32
- GCode senders
  - Some reboot it the ESP32
  - Some send a Grbl reset command
  - Some do neither.