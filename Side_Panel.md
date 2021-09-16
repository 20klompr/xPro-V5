# Side Panel
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/QuickStart_FRONT.jpg" width="800">

## Power Switch
The power switch controls turns off the main **+24V** which powers the Stepper Drivers as well as the 5V and 3.3V when a USB cable is not plugged in.

With the Power Switch **OFF**, +24V to the stepper drivers is removed; however, with USB plugged in, 5V will power the 32 bit Arm processor and associated logic functions. _NOTE: if one of the [Motor fault indicators](https://github.com/Spark-Concepts/xPro-V5/wiki/Side_Panel#motor-fault-indicators) illuminates bright **RED** you will need to power cycle the xPro-V5 via the power switch.

## Reset Switch
The reset switch reboots the xPro-V5.

## SD Card
All commands can be in the $ format like $SD/List or the older [ESP...] format, like [ESP210]. The $ format is detailed on this page. You can get a list of all $ commands by sending $Cmd. The [ESP...] format is described in this document.

### SD Card Commands
- Get SD Card Status/Content: ```$SD/Status```
  - _Shows all the files. This is recursive and will search all subdirectories. Each file will print like this... ```[FILE:/TEST.nc|SIZE:29547]```
 ...where /TEST.nc is the filename. including the directory. In this case the directory is the root. The number following the file name is the file size. There is no filter, all files and folders are reported. Senders, WebUI, etc should handle this._
- Print SD file: ```$SD/Run```
  - _```$#D/Run=/TEST.nc``` will run file /TEST.nc - **Note: If in alarm mode, this command will fail with error 9**_

### Adding / uploading files to SD card
You can remove the card and use a PC to add files or you can upload them via the WebUI.

### Errors
Any gcode errors in the SD card file will terminate the job. The offending line number of the file will be reported.

### Status
When an SD card job is running, the percent complete is appended to the status string. This is simply percent of bytes read from the file.
```<Idle|WPos:195.000,144.000,19.000|Bf:15,128|FS:0.000,0.000|Pn:P|WCO:-195.000,-144.000,-19.000|SD:45.5>```

### Card Formatting
The firmware uses the [Arduino SD library](https://www.arduino.cc/en/Reference/SD). This is limited to 4G cards. In general, the smallest, oldest and slowest cards tend to work best with this library.

Some people have trouble when SD cards have been formatted by Windows, but were able to solve the problem by formatting with [SD Card Formatter](https://sd-card-formatter.en.uptodown.com/windows)

## Motor fault indicators
Indication of driver errors: 
- Over temperature & Over temperature prewarning
- Short to GND
- Under voltage

## Status indicators
Indication of (active) function status:
- Relay ([spindle enable or mist](https://github.com/Spark-Concepts/xPro-V5/wiki/Front_Panel#relay-terminal))
- Z-Probe
- Coolant Mist
- Spindle

## Probe input
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/zprobe3.jpg">

### Probe Wiring
When the milling bit (with the clip on), touch the probe plate, **"SIG" and "GND" make contact...**

The xPRO-V5 uses a 3 pin connector which is provided with the controller
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/probe_non-powered.jpg">

To connect the xPRO-V5 to an unpowered touch probe, connect one wire to **GND** and the other to **SIG** using the provided EDG connector. 

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/warning.png" width="35"> Leave the middle pin unconnected -
Connecting 24V or 5V directly to the GND (aka shorting) can permanently damage the controller. Please be sure to double-check your wiring before plugging in all accessories!

When connecting a powered probe, please refer to the diagram below: 
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/probe_powered.jpg">

### Probe Testing and Setup

Before probing make sure the probe circuit is working. Send the `?` character from a serial terminal to get the current status. If your gcode sender does not show you the raw status response, you may need to use a simple serial terminal. The status response will show you any active inputs like the probe.

It will look something like this with no switches active...

`<Idle|MPos:0.000,0.000,0.000|FS:0,0|WCO:0.000,0.000,0.000>`

and something like this with the probe switch active...

`<Idle|MPos:0.000,0.000,0.000|FS:0,0|Pn:P|Ov:100,100,100>`

The **Pn:** section is for active input pins and the P indicates the probe is active.

You do not want to see the probe active when it is not touching and see it active when it is touching.

Use the `$6` or `$Probe/Invert` [setting](https://github.com/Spark-Concepts/xPro-V5/wiki/Changing-settings#probeinvert-or-6---probe-pin-invert-boolean) to flip the logic if your is reporting backwards.

### Probing
Many senders have this feature built in, including the WebUI.

Here is a basic gcode sequence for a Z probe: Typically it is done in the Z direction; however, other direction(s) could be specified.

1. Send...`G38.2 Z-5.0 F50` This tells Grbl to probe a maximum Z distance of -5mm at a speed of 50mm/min
2. Receive...`[PRB: 0.000, 0.000, -15.621]` This is a typical response after touching the plate. This was the machine space location when it touched. The current location is probably a tiny bit lower due to the deceleration.
3. Send...`G53 G0 Z-15.621` This tells Grbl to move to the actual probe location in machine space. This corrects for the overshoot.
4. Send...`G10 L20 P0 Z3.00` This tells Grbl to zero the current work coordinate system (P0) to the thickness of your touch plate (3.00mm).

## Macro1 & Macro2
### Macro Button Feature Overview
Macro buttons allow you to associate a switch closure with a macro. A basic macro can be simple commands or run a file from an SD card or the ESP32 flash file system. 

### Electrical
Buttons connected to "Macro 1" or "Macro 2" must be normally open (by default). Buttons can be inverted, but must be done so in [firmware](https://github.com/Spark-Concepts/xPro-V5-Firmware/blob/main/CNCxPROv5_20210708/src/Machines/CNC_xPRO_V5_Machine_Template.h): 
```CPP
// The mask order is ...
// Macro3 | Macro2 | Macro1 | Macro0 | Cycle Start | Feed Hold | Reset | Safety Door
// For example Bnn101111 will invert the function of the "Macro 1" pin (Macro0).
#define INVERT_CONTROL_PIN_MASK Bnnnnnnnn
``` 

### Implementing Macros:

*note: Macro0="Macro 1" and Macro1="Macro 2" as labeled on the xPro-V5; also, you must be in the idle state to execute button macros*

`$User/Macro0=` *assigns a macro to the "Macro 1" input-(Normally Open Switch)*

`$User/Macro1=` *assigns a macro to the "Macro 2" input-(Normally Open Switch)*

1. **Single Command:** This can be any command or gcode. Example: `$User/Macro0=$H`

2. **Multiple commands:** Place and ampersand "&" between commands (limit 250 chars). Example: `$User/Macro0=G90&G53G0Z-1&G0X0Y0`

3. **File from SD Card:** Example: Use the $SD/Run command and a filename Example: `$User/Macro0=$SD/Run=foo.nc`

A file can be [gcode](http://www.science.smith.edu/cdf/pdf_files/Techno_GCODE%20Commands.pdf), most Grbl_ESP32 [commands](https://github.com/Spark-Concepts/xPro-V5/wiki/Changing-settings#grbl-commands) and [settings](https://github.com/Spark-Concepts/xPro-V5/wiki/Changing-settings#grbl-settings) or a mixture of both.

## TMC diag_0
Configured as open collector output (active low) - active on driver errors: 
- Over temperature & Over temperature prewarning
- Short to GND
- Under voltage

Also used for TMC driver StallGuard output, if enabled

## Power

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front-plugs.jpg" width="400">

## Cool

The Coolant output can be used to control dust-extraction or cutting fluid systems, and/or repurposed for other switching requirements. 
_Note: the port labeled "GND" acts as a switched ground source, the signal labeled "SIG" is a constant 5V(default) or 24V*_

**The Cool(mist) signal is enabled using the _M7_ (mist) gcode statement and turned off with the _M9_ gcode statement**

Electrical Specifications:
- Voltage: jumper select<sub>(1)</sub> - 24v or 5v(default)
- Max Current: 250mA(5V) and 2A(24V)
- Suitable for inductive loads (24V only)

<sub>*the SIG port can also be configured to output 24V by removing the xProV5 lid and moving the "MIST" jumper<sub>(1)</sub> from 5V to 24V</sub>
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Cool_SS.jpg" width="800">
