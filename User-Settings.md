## Getting Started

### The difference between settings and commands 
Settings are options that affect the behavior of the xPro V5.  They maintain their values when power is off, so when you change a value, the new value persists across reboots and power cycles.  An example setting is "X/StepsPerMm". Commands are actions that happen immediately, without affecting long-term behavior.  An example command is "start a homing cycle" or "$H".  You can get a list of settings by typing **$s** - in a Grbl serial console.  You can get a list of commands with **$cmd** .

When you enter a line of text to the xPro V5, if the line does not start with either $ or [, the line is handled by the GCode interpreter.  Otherwise the line is treated as a command or setting.

#### Note:

The number settings is not dependent on your machine definition. If your machine does not have a spindle, for example, you will still see and be able to set spindle settings. They will not be used, until you define a spindle.

### Commands and Settings without Values

**$_name_**

If _name_ is a command, that command is executed.  For example, **$H** runs a homing cycle.

If _name_ is a setting, the current value of that setting is displayed.  For example, **$X/Microsteps**  displays `$X/Microsteps=8` - or whatever the current value happens to be.

Case does not matter; you could write **$x/microsteps** instead of **$X/Microsteps** .

_name_ can also be a "classic Grbl" setting number or an WebUI "ESPnnn" name. For example, **$110** displays something like `$110=1000.000`

When displaying settings, you can supply a partial name and every setting whose name contains that substring will be displayed.  For example, **$micros** displays

```
$X/Microsteps=16
$Y/Microsteps=16
$Z/Microsteps=16
```

Sometimes you want to use a partial name that conflicts with a command name.  For example, **$X** is a command, so if you wanted to display every setting that contains "X", **$X** would execute the command instead of displaying the settings.  You can work around that by preceding the partial name with "*" - so $*x displays all settings whose names contain the letter X.  Alternatively, you could use a longer partial name, like **$x/**

### Commands and Settings with Values

**$_name_=_value_**

If _name_ is a command that accepts an additional parameter, that command is executed with _value_ as the parameter.  For example, **$J=X10.0** performs a jog to X coordinate 10.0

If _name_ is a setting, the setting is changed to that _value_ (after checking that the value is valid).  _name_ can either be a textual name, a classic Grbl setting number, or an ESP32_WebUI "ESPnnn" name.

Partial names do not work when a value is present.

### Alternate Form

For backwards compatibility, instead of **$_name_** or **$_name_=_value_**, you can write **[_name_]** or **[_name_]_value_** .  Prior to the introduction of the new settings mechanism, **[_name_]** was used only for ESP3D_WebUI settings like **[ESP105]** ; now any setting or command name, whether textual, numerical, or ESPnnn, can be used with either **$_name_** or **[_name_]**.  **$_name_** is preferred; **[_name_]** is provided only for compatibility.

## Grbl Commands

If there is an entry in the "Grbl Name" column, the command exists in "classic Grbl"; otherwise the command is specific to the xPro V5.

|Long Name|Short Name|Grbl Name|Value|Action|
| -----|----------|-------|------|-----|
|Help|$|$||Show brief help line|
|GrblSettings/List|$$|$$||[Show list of GRBL numbered settings and their current values](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#and-xval---view-and-write-grbl-settings) |
|ExtendedSettings/List|$+|||Show list of extended numbered settings and their current values.  Extended settings include those in classic GRBL plus numbered settings that were introduced prior to the new settings framework|
|Settings/List|$S|||Show list of all settings by textual name and their current values|
|GrblNames/List|$L|||Show the correspondence between numbered settings and textual names|
|ErrorCodes/List|$E||number|Show the error numbers and their descriptions.  If a value is provided, for example $e=5, show the description of only that error number.|
|Jog|$J|$J|jogspec|[Perform a jog operation](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#jline---run-jogging-motion).  See also [Grbl Jog Specification](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Jogging) |
|GCode/Offsets|$#|$#||[Show the GCode parameters such as work offsets](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#---view-gcode-parameters)|
|GCode/Modes|$G|$G||[Show the GCode parser state](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#g---view-gcode-parser-state)|
|GCode/Check|$C|$C||[Toggle GCode checking mode](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#c---check-gcode-mode)| 
|Alarm/Disable|$X|$X||[Disable the GRBL Alarm Lock](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#x---kill-alarm-lock)|
|Settings/Stats|$V|||Show non-volatile storage statistics - Used entries, free entries, and total entries|
|Home|$H|$H||[Home all axes](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#h---run-homing-cycle)|
|Home/X|$HX|||Home the X axis|
|Home/Y|$HY|||Home the Y axis|
|Home/Z|$HZ|||Home the Z axis|
|Home/A|$HA|||Home the A axis|
|Home/B|$HB|||Home the B axis|
|Home/C|$HC|||Home the C axis|
|System/Sleep|$SLP|$SLP||[Enter sleep mode](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#slp---enable-sleep-mode)|
|Build/Info|$I|||Display build information|
|GCode/StartupLines|$N|||[Display all GCode startup lines](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#n---view-startup-blocks)|
|Settings/Erase|$NVX|||Erase non-volatile storage, returning all settings to the default values that were established when the firmware was compiled.  If authentication is enabled, this requires admin privileges.|
|Settings/Restore|$RST||subcommand|[Restore saved information](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#rst-rst-and-rst--restore-grbl-settings-and-data-to-defaults)  If subcommand is "$" or "settings", resets the named settings to their default values.  If subscommand is "#" or "gcode", resets the saved GCode offsets to zero.  If subcommand is "@" or "wifi", resets the WiFi/WebUI-related settings to their default values.  If subcommand is "*" or "all", resets everything previously mentioned.  If authentication is enabled, this requires admin privileges.|

### Extended Commands

Extended commands control or display various aspects of the system.  The distinction between Grbl Commands and Extended Commands is, to some extent, an historical artifact reflecting limitations of Grbl running on AVR microprocessors.  The distinction might be eliminated in the future. Many of the extended commands were first introduced with the WebUI interface, but they are not entirely WebUI-specific.  For example, the commands for managing files are useful from the command line.

Commands that say "_For program use._" are primarily intended for use by the WebUI internal code, as their input and output formats are optimized for communicating with computer code instead of humans.  They can be issued directly by humans, but there are more human-friendly ways of doing the same thing.

The "LocalFS" commands refer to a file system that is implemented in storage that is on the microprocessor module, not on an external SD card.  That local file system is sometimes called "SPIFFS", referring to a particular format that is often used to manage on-module storage.

For information about the **Auth** column, see [the Authentication section](#authentication).

|Full Name|Old Name|Auth|Value|Description|
|-|-|-|-|-|
|WebUI/Help|ESP|||Show a WebUI help message|
|WebUI/Help|ESP0|||Show a WebUI help message|
|WebUI/List|ESP400|User||List all of the ESPxxx commands.  _For program use._|
|WebUI/Set|ESP401|Admin|params|Set a setting according to the value string P=fullname T=type V=value .   _For program use._|
|WebUI/SetUserPassword|ESP555|Admin|password|Set the administrator password.  If the value is not given, restore the default password.|
|System/Stats|ESP420|User||Show an extensive list of system information|
|System/Control|ESP444|Admin|RESTART|Restart the Grbl controller.  Requires value = RESTART|
|Firmware/Info|ESP800|||Show firmware information|
|Sta/Setup|ESP103|Admin|params|Setup the WiFi station parameters on one line. The value is a string like IP=ipaddress MSK=netmask GW=gateway . You can also set these with individual settings.  _For program use._|
|System/IP|ESP111|||Show the system's IP address|
|WiFi/ListAPs|ESP410|User||List the WiFi access points that are reachable.|
|Radio/State|ESP115|Admin|state|Set the radio state.  Value is STA for WiFi station mode, AP for WiFi Access Point mode, BT for Bluetooth, or OFF|
|SD/Status|ESP200|User||Show the SD card insertion status.| 
|SD/List|ESP210|User||List files on the SD card.|
|SD/Delete|ESP215|User|path|Delete a file or directory from the SD card.  Value is the file or directory pathname.|
|SD/Run|ESP220|User|path|Run a GCode program from the SD card.  Value is the file pathname.|
|Notification/Send|ESP600|User|message|Send the value "message" as a notification.|
|Notification/Setup|ESP610|Admin|params|Setup notifications.  The value is a string like TYPE=NONE|PUSHOVER|EMAIL|LINE T1=token1 T2=token2 TS=settings , which sets all the notification parameters in one line.  You can also set individual notification parameters with settings.  _For program use._|
|LocalFS/List||User|path|List files in local filesystem.  If a value is supplied, lists only that subdirectory.|
|LocalFS/Run|ESP700|User|path|Run a GCode program from the local filesystem.  Value is the file pathname.|
|LocalFS/Format|ESP710|Admin|FORMAT|Reformat the local filesystem.  Requires value = FORMAT|
|LocalFS/Size|ESP720|User||Show used space and free space in local filesystem|
|LocalFS/ListJSON||User|path|List files in local filesystem in JSON format.  If a value is supplied, lists only that subdirectory.  JSON format is designed so a computer program can easily interpret the data.   _For program use._|

### Grbl Settings

Many of the numbered settings below are present in classic Grbl and their meanings are as defined [Grbl Configuration](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Configuration). The new ones are the Spindle/ settings (33-36), the A, B, and C axis settings (1x3, 1x4, 1x5), and the additional settings for driver modules (1x3, 1x4, 1x5, and 1x6).  The meaning of new settings can usually be inferred from the name, type, and units.

The allowable values for a setting depends on its Type.  For Int and Float settings, Min and Max are the minimum and maximum values.  For String settings, Min and Max constrain the length of the string.

Flag settings can be either ON (alternatively 1) or OFF (alternatively 0).

AxisMask settings can be expressed either as a list of axis letters or, for backwards compatibility, as a numerical sum of bits, where 1 represents the X axis, 2 is Y, 4 is Z, 8 is A, 16 is B, and 32 is C.  For example axes X and Z can be expressed either as s the string **XZ** or as the number 5 (1 + 4). The letter form is easier to remember and type, while the numerical form is backwards compatibility with classic Grbl.  To clear an axis mask, you can send either **$_name_=** (with nothing after the equals sign) or $**_name_=0**.

Enum settings have a specified set of allowable values, that can be expressed either as a small integer or a name.

|Full Name|GRBL #|Type|Units|Default|Min|Max|Description|
|-|-|-|-|-|-|-|-|
|Stepper/Pulse|0|Int|usec|3|3|1000|[Set the stepper motor pulse time](#stepperPulse-or-0-–-step-pulse-microseconds)|
|Stepper/IdleTime|1|Int|msec|250|0|255|[Time before steppers are disabled when idle](#stepperidletime-or-1---step-idle-delay-milliseconds)|
|Stepper/StepInvert|2|AxisMask||0|||[Axes whose step signals are inverted](#stepperstepinvert-or-2--step-port-invert-mask)|
|Stepper/DirInvert|3|AxisMask||0|||[Axes whose direction signals are inverted](#stepperdirinvert-or-3--direction-port-invert-mask)|
|Stepper/EnableInvert|4|Flag||OFF|||[Do not invert (off) or invert (on) all stepper enable signals](#stepperenableinvert-or-4---step-enable-invert-boolean)|
|Limits/Invert|5|Flag||ON|||[Limit switch active high (off) or low (on)](#limitsinvert-or-5---limit-pins-invert-boolean)|
|Probe/Invert|6|Flag||OFF|||[Probe switch active high (off) or low (on)](#probeinvert-or-6---probe-pin-invert-boolean)|
|Report/Status|10|Int||1|0|2|[Fields to include in real-time reports](#reportstatus-or-10---status-report-mask)|
|GCode/JunctionDeviation|11|Float||0.01|0|10|[Controls motion around sharp corners](#gcodejunctiondeviation-or-11---junction-deviation-mm)|
|GCode/ArcTolerance|12|Float||0.002|0|1|[Controls accuracy of arc tracing](#arctolerance-or-12--arc-tolerance-mm)|
|Report/Inches|13|Flag||OFF|||[Report in mm (off) or inches (on)](#reportinches-or-13---report-inches-boolean)|
|Limits/Soft|20|Flag||OFF|||[Disable (off) or enable (on) soft limits](#limitssoft-or-20---soft-limits-boolean)|
|Limits/Hard|21|Flag||OFF|||[Disable (off) or enable (on) hard limits](#limitshard-or-21---hard-limits-boolean)|
|Homing/Enable|22|Flag||OFF|||[Disable (off) or enable (on) homing feature](#homingenable-22---homing-cycle-boolean)|
|Homing/DirInvert|23|AxisMask||3 (XY)|||[Homing directions for various axes](#homingdirinvert-or-23---homing-dir-invert-mask)|
|Homing/Feed|24|Float|mm/min|200|0|10000|[Slower rate for homing](#homingfeed-or-24---homing-feed-mmmin)|
|Homing/Seek|25|Float|mm/min|2000|0|10000|[Faster rate for homing](#homingseek-or-25---homing-seek-mmmin)|
|Homing/Debounce|26|Float|msec|250|0|10000|[Homing switch debounce time](#homingdebounce-or-26---homing-debounce-milliseconds)|
|Homing/Pulloff|27|Float|mm|1|0|1000|[Homing pulloff distance](#homingpulloff-or-27---homing-pull-off-mm)|
|Homing/Squared||AxisMask||0|||[Axes that are squared during homing](#homingsquaring---axes-that-are-squared-during-homing)|
|Homing/MPos||Float | || | | [Desired Machine Position after homing](https://github.com/bdring/Grbl_Esp32/wiki/Machine-Space-and-Homing)|
|Homing/Cycle0||AxisMask|||||[Axis homing order](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-Limit-Homing-Switch)|
|Homing/Cycle1||AxisMask|||||[Axis homing order](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-Limit-Homing-Switch)|
|Homing/Cycle2||AxisMask|||||[Axis homing order](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-Limit-Homing-Switch)|
|Homing/Cycle3||AxisMask|||||[Axis homing order](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-Limit-Homing-Switch)|
|Homing/Cycle4||AxisMask|||||[Axis homing order](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-Limit-Homing-Switch)|
|Homing/Cycle5||AxisMask|||||[Axis homing order](https://github.com/bdring/Grbl_Esp32/wiki/Setting-Up-Limit-Homing-Switch)|
|GCode/MaxS|30|Float|depends|1000|0|100000|[Max value for GCode S word](#gcodemaxs-or-30---max-spindle-speed-rpm)|
|GCode/MinS|31|Float|depends|0|0|100000|[Minimum value for GCode S word](#gcodemins-or-31---min-spindle-speed-rpm)|
|GCode/LaserMode|32|Flag||OFF|||[Disable (off) or enable (on) Laser Mode](#gcodelasermode-or-32---laser-mode-boolean)|
|GCode/Line1|N1|String|||0|255|[First GCode startup line](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#n---view-startup-blocks)|
|GCode/Line0|N0|String|||0|255|[Second GCode startup line](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#n---view-startup-blocks)|
|Spindle/PWM/Frequency|33|Float|cycles/sec|5000|0|100000|[Spindle PWM frequency](#spindlepwmfrequency-or-33---spindle-pwm-frequency-extended-setting)|
|Spindle/PWM/Off|34|Float|percent|0|0|100|[Value to turn spindle off](#spindlepwmoff-or-34---spindle-pwm-off--extended-setting)|
|Spindle/PWM/Min|35|Float|percent|0|0|100|[Minimum value for running spindle](#spindlepwmmin-or-35---spindle-pwm-min-extended-setting)|
|Spindle/PWM/Max|36|Float|percent|100|0|100|[Maximum value for running spindle](#spindlepwmmax-or-36---spindle-pwm-max--extended-setting)|
|Spindle/Enable/Invert | |Flag||Off|||Inverts the spindle enable output|
|Spindle/PWM/Invert | |Flag||Off|||Inverts the spindle output|
|Spindle/Enable/OffWithSpeed | |Flag||Off|||Enable will be off at 0 RPM Regardless of M3/M4/M5|
|Report/StallGuard||AxisMask||0|||List of axes for reporting stallguard status|
|Spindle/Type||Enum||NONE|||[Spindle type](spindletype---spindle-type)|
|X/StepsPerMm|100|Float|steps/mm|100|1|100000|[Steps per mm for X axis](#xstepspermm-thru-cstepspermm-or-100-thru-105--xyzabc-stepsmm)|
|X/MaxRate|110|Float|mm/min|1000|1|100000|[Maximum speed for X axis](#xmaxrate-thru-cmaxrate--or-110-thru-115--xyzabc-max-rate-mmmin)|
|X/Acceleration|120|Float|mm/sec^2|200|1|100000|[Acceleration for X axis](#xacceleration-thru-cacceleration-or-120-thru-125--xyzabc-acceleration-mmsec2)|
|X/MaxTravel|130|Float|mm|300|1|100000|[Length of X axis](#xmaxtravel-thru-cmaxtravel-or-130-thru-135--xyzabc-max-travel-distance-mm)|
|X/Current/Run|140|Float|amperes|0.25|0|20|[Trinamic driver run current for X axis](#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings)|
|X/Current/Hold|150|Float|amperes|0.125|0.05|20|[Trinamic driver hold current for X axis](#xcurrenthold-thru-ccurrenthold-or-150-thru-155--xyzabc--stepper-idle-current-extended-settings)|
|X/Microsteps|160|Int|microsteps/step|16|0|256|[Trinamic driver microstepping for X axis](#xmicrosteps-thru-cmicrosteps-or-160-thru-165--xyzabc--microstepping-extended-settings)|
|X/StallGuard|170|Int|register value|16|-64|63|[Trinamic driver Stallguard setting for X axis](#xstallguard-thru-cstallguard-or-170-thru-175--xyzabc--stall-guard-value-extended-settings)|
|Y/StepsPerMm|101|Float|steps/mm|100|1|100000|[Steps per mm for Y axis](#xstepspermm-thru-cstepspermm-or-100-thru-105--xyzabc-stepsmm)|
|Y/MaxRate|111|Float|mm/min|1000|1|100000|[Maximum speed for Y axis](#xmaxrate-thru-cmaxrate--or-110-thru-115--xyzabc-max-rate-mmmin)|
|Y/Acceleration|121|Float|mm/sec^2|200|1|100000|[Acceleration for Y axis](#xacceleration-thru-cacceleration-or-120-thru-125--xyzabc-acceleration-mmsec2)|
|Y/MaxTravel|131|Float|mm|300|1|100000|[Length of Y axis](#xmaxtravel-thru-cmaxtravel-or-130-thru-135--xyzabc-max-travel-distance-mm)|
|Y/Current/Run|141|Float|amperes|0.25|0|20|[Trinamic driver run current for Y axis](#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings)|
|Y/Current/Hold|151|Float|amperes|0.125|0.05|20|[Trinamic driver hold current for Y axis](#xcurrenthold-thru-ccurrenthold-or-150-thru-155--xyzabc--stepper-idle-current-extended-settings)|
|Y/Microsteps|161|Int|microsteps/step|16|0|256|[Trinamic driver microstepping for Y axis](#xmicrosteps-thru-cmicrosteps-or-160-thru-165--xyzabc--microstepping-extended-settings)|
|Y/StallGuard|171|Int|register value|16|-64|63|[Trinamic driver Stallguard setting for Y axis](#xstallguard-thru-cstallguard-or-170-thru-175--xyzabc--stall-guard-value-extended-settings)|
|Z/StepsPerMm|102|Float|steps/mm|100|1|100000|[Steps per mm for Z axis](#xstepspermm-thru-cstepspermm-or-100-thru-105--xyzabc-stepsmm)|
|Z/MaxRate|112|Float|mm/min|1000|1|100000|[Maximum speed for Z axis](#xmaxrate-thru-cmaxrate--or-110-thru-115--xyzabc-max-rate-mmmin)|
|Z/Acceleration|122|Float|mm/sec^2|200|1|100000|[Acceleration for Z axis](#xacceleration-thru-cacceleration-or-120-thru-125--xyzabc-acceleration-mmsec2)|
|Z/MaxTravel|132|Float|mm|300|1|100000|[Length of Z axis](#xmaxtravel-thru-cmaxtravel-or-130-thru-135--xyzabc-max-travel-distance-mm)|
|Z/Current/Run|142|Float|amperes|0.25|0|20|[Trinamic driver run current for Z axis](#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings)|
|Z/Current/Hold|152|Float|amperes|0.125|0.05|20|[Trinamic driver hold current for Z axis](#xcurrenthold-thru-ccurrenthold-or-150-thru-155--xyzabc--stepper-idle-current-extended-settings)|
|Z/Microsteps|162|Int|microsteps/step|16|0|256|[Trinamic driver microstepping for Z axis](#xmicrosteps-thru-cmicrosteps-or-160-thru-165--xyzabc--microstepping-extended-settings)|
|Z/StallGuard|172|Int|register value|16|-64|63|[Trinamic driver Stallguard setting for Z axis](#xstallguard-thru-cstallguard-or-170-thru-175--xyzabc--stall-guard-value-extended-settings)|
|A/StepsPerMm|103|Float|steps/mm|100|1|100000|[Steps per mm for A axis](#xstepspermm-thru-cstepspermm-or-100-thru-105--xyzabc-stepsmm)|
|A/MaxRate|113|Float|mm/min|1000|1|100000|[Maximum speed for A axis](#xmaxrate-thru-cmaxrate--or-110-thru-115--xyzabc-max-rate-mmmin)|
|A/Acceleration|123|Float|mm/sec^2|200|1|100000|[Acceleration for A axis](#xacceleration-thru-cacceleration-or-120-thru-125--xyzabc-acceleration-mmsec2)|
|A/MaxTravel|133|Float|mm|300|1|100000|[Length of A axis](#xmaxtravel-thru-cmaxtravel-or-130-thru-135--xyzabc-max-travel-distance-mm)|
|A/Current/Run|143|Float|amperes|0.25|0|20|[Trinamic driver run current for A axis](#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings)|
|A/Current/Hold|153|Float|amperes|0.125|0.05|20|[Trinamic driver hold current for A axis](#xcurrenthold-thru-ccurrenthold-or-150-thru-155--xyzabc--stepper-idle-current-extended-settings)|
|A/Microsteps|163|Int|microsteps/step|16|0|256|[Trinamic driver microstepping for A axis](#xmicrosteps-thru-cmicrosteps-or-160-thru-165--xyzabc--microstepping-extended-settings)|
|A/StallGuard|173|Int|register value|16|-64|63|[Trinamic driver Stallguard setting for A axis](#xstallguard-thru-cstallguard-or-170-thru-175--xyzabc--stall-guard-value-extended-settings)|
|B/StepsPerMm|104|Float|steps/mm|100|1|100000|[Steps per mm for B axis](#xstepspermm-thru-cstepspermm-or-100-thru-105--xyzabc-stepsmm)|
|B/MaxRate|114|Float|mm/min|1000|1|100000|[Maximum speed for B axis](#xmaxrate-thru-cmaxrate--or-110-thru-115--xyzabc-max-rate-mmmin)|
|B/Acceleration|124|Float|mm/sec^2|200|1|100000|[Acceleration for B axis](#xacceleration-thru-cacceleration-or-120-thru-125--xyzabc-acceleration-mmsec2)|
|B/MaxTravel|134|Float|mm|300|1|100000|[Length of B axis](#xmaxtravel-thru-cmaxtravel-or-130-thru-135--xyzabc-max-travel-distance-mm)|
|B/Current/Run|144|Float|amperes|0.25|0|20|[Trinamic driver run current for B axis](#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings)|
|B/Current/Hold|154|Float|amperes|0.125|0.05|20|[Trinamic driver hold current for B axis](#xcurrenthold-thru-ccurrenthold-or-150-thru-155--xyzabc--stepper-idle-current-extended-settings)|
|B/Microsteps|164|Int|microsteps/step|16|0|256|[Trinamic driver microstepping for B axis](#xmicrosteps-thru-cmicrosteps-or-160-thru-165--xyzabc--microstepping-extended-settings)|
|B/StallGuard|174|Int|register value|16|-64|63|[Trinamic driver Stallguard setting for B axis](#xstallguard-thru-cstallguard-or-170-thru-175--xyzabc--stall-guard-value-extended-settings)|
|C/StepsPerMm|105|Float|steps/mm|100|1|100000|[Steps per mm for C axis](#xstepspermm-thru-cstepspermm-or-100-thru-105--xyzabc-stepsmm)|
|C/MaxRate|115|Float|mm/min|1000|1|100000|[Maximum speed for C axis](#xmaxrate-thru-cmaxrate--or-110-thru-115--xyzabc-max-rate-mmmin)|
|C/Acceleration|125|Float|mm/sec^2|200|1|100000|[Acceleration for C axis](#xacceleration-thru-cacceleration-or-120-thru-125--xyzabc-acceleration-mmsec2)|
|C/MaxTravel|135|Float|mm|300|1|100000|[Length of C axis](#xmaxtravel-thru-cmaxtravel-or-130-thru-135--xyzabc-max-travel-distance-mm)|
|C/Current/Run|145|Float|amperes|0.25|0|20|[Trinamic driver run current for C axis](#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings)|
|C/Current/Hold|155|Float|amperes|0.125|0.05|20|[Trinamic driver hold current for C axis](#xcurrenthold-thru-ccurrenthold-or-150-thru-155--xyzabc--stepper-idle-current-extended-settings)|
|C/Microsteps|165|Int|microsteps/step|16|0|256|[Trinamic driver microstepping for C axis](#xmicrosteps-thru-cmicrosteps-or-160-thru-165--xyzabc--microstepping-extended-settings)|
|C/StallGuard|175|Int|register value|16|-64|63|[Trinamic driver Stallguard setting for C axis](#xstallguard-thru-cstallguard-or-170-thru-175--xyzabc--stall-guard-value-extended-settings)|

### WebUI Settings

WebUI settings configure the way the xPro v5 connects to the network - either by connecting to an external WiFi router (STA - Station - mode), by acting as an access point to which other machines can connect (AP) (**_Recommended_**), or by Bluetooth (BT).  AP mode is primarily used for initial setup, so you can talk to the xPro V5 for the purpose of setting up the Station parameters, if desired.  
**Note** The xPro V5 can handle only a few client connections in AP mode, so it is not suitable for use as a general-purpose wireless router.

Some of the settings below are only available if a corresponding configuration option is enabled when the firmware is compiled.

The default values shown below can be changed by modifying the source and recompiling; the defaults as shown are the ones for unmodified source.

Some of the settings groups below can be set _en masse_ on a single line with a WebUI Command in the table above - or you can set them individually.

|Full Name|Old Name|Default|Values|Description|
|-|-|-|-|-|
|WebUI/UserPassword||user|string|User password|
|WebUI/AdminPassword||admin|string|Admin password|
|Radio/Mode|ESP110|AP|NONE or STA or AP or BT|Radio mode|
|System/Hostname|ESP112|cnc_xpro_v5|string|Hostname seen by other machines on the network|
|Sta/SSID|ESP100||string|Station SSID|
|Sta/Password|ESP101||string|Station Password|
|Sta/IPMode|ESP102|DHCP|DHCP or Static|Station IP Mode|
|Sta/IP||0.0.0.0|IP address|Station Static IP|
|Sta/Gateway||0.0.0.0|IP address|Station Static Gateway|
|Sta/Netmask||0.0.0.0|IP mask|Station Static Netask|
|AP/SSID|ESP105|cnc_xpro_v5|string|AP SSID|
|AP/Password|ESP106|12345678|string|AP Password|
|AP/IP|ESP107|192.168.0.1|IP address|AP Static IP address|
|AP/Channel|ESP108|1|number|AP Channel number|
|Http/Enable|ESP120|ON|ON or OFF|HTTP Enable|
|Http/Port|ESP121|80|number|HTTP Port number|
|Telnet/Enable|ESP130|ON|ON or OFF|Telnet Enable|
|Telnet/Port|ESP131|23|number|Telnet Port|
|Bluetooth/Name|ESP140|btcnc_xpro||Bluetooth name|
|Notification/Type||NONE|NONE or LINE or PUSHOVER or EMAIL|Notification type|
|Notification/T1|||string|Notification Token 1|
|Notification/T2|||string|Notification Token 2|
|Notification/TS|||string|Notification Settings|

### Authentication

If authentication is enabled, some of the settings cannot be displayed without at least the "user level" password, and cannot be set without the "admin level" password.  When commands and settings are managed via WebUI, a login procedure sets the password so it need only be supplied once.  If authentication is on and you need to execute a protected command from a serial monitor, include "pwd=<password>" at the end of the command line - separated from the value with a space, if necessary.  For example
```
$Sta/SSID=MySSID pwd=admin
```

To enable authentication you must uncomment `#define ENABLE_AUTHENTICATION `in config.h and recompile.

NOTE: Grbl's authentication does not provide strong defense against an attacker.  It should be treated as a guard against inadvertent mistakes, rather than as strong security.

### More details on the Grbl Settings

#### $Stepper/Pulse or $0 – Step pulse, microseconds

Stepper drivers are rated for a certain minimum step pulse length. Check the data sheet or just try some numbers. You want the shortest pulses the stepper drivers can reliably recognize. If the pulses are too long, you might run into trouble when running the system at very high feed and pulse rates, because the step pulses can begin to overlap each other. We recommend something around 10 microseconds, which is the default value.

#### $Stepper/IdleTime or $1 - Step idle delay, milliseconds

Every time your steppers complete a motion and come to a stop, Grbl will delay disabling the steppers by this value. **OR**, you can always keep your axes enabled (powered so as to hold position) by setting this value to the maximum 255 milliseconds. Again, just to repeat, you can keep all axes always enabled by setting `$1=255`.

The stepper idle lock time is the time length Grbl will keep the steppers locked before disabling. Depending on the system, you can set this to zero and disable it. On others, you may need 25-50 milliseconds to make sure your axes come to a complete stop before disabling. This is to help account for machine motors that do not like to be left on for long periods of time without doing something. Also, keep in mind that some stepper drivers don't remember which micro step they stopped on, so when you re-enable, you may witness some 'lost' steps due to this. In this case, just keep your steppers enabled via `$1=255`.

#### $Stepper/StepInvert or $2 – Step port invert, mask

This setting inverts the step pulse signal. By default, a step signal starts at normal-low and goes high upon a step pulse event. After a step pulse time set by `$0`, the pin resets to low, until the next step pulse event. When inverted, the step pulse behavior switches from normal-high, to low during the pulse, and back to high. Most users will not need to use this setting, but this can be useful for certain CNC-stepper drivers that have peculiar requirements. For example, an artificial delay between the direction pin and step pulse can be created by inverting the step pin.

To set it the easy way, just list the axes that you want to invert.  For example, `$Stepper/StepInvert=ya` would invert the `Y` and `A` axes.

For compatibility with classic Grbl, you can also use the numeric "bit mask" form. You really don't need to completely understand how it works. You simply need to enter the settings value for the axes you want to invert. For example, if you want to invert the X and Z axes, you'd send `$2=5` to Grbl and the setting should now read `$2=5 (step port invert mask:00000101)`.

In the table below, the "CBAZYX" field is the binary value for the `#define LIMIT_MASK Bxxxxxx` line in the machine configuration file.  Note that the "Y" entries in the Invert columns exactly correspond to the positions of the "1" bits in the "CBAZYX" column. The "Decimal" field is the decimal equivalent of that binary number.

The table shows only the A, Z, Y, and X axes, for brevity (each axis doubles the number of rows, so the table would have 64 rows were C and B axes included).  To invert the B axis, simply put a 1 in the B-*---- bit - or add 16 to the decimal value.  To invert the C axis, put a 1 in the B*----- bit, or add 32 to the decimal value.

| Decimal |  CBAZYX | Invert A | Invert Z | Invert Y | Invert X |
| ------- | ------- | -------- | -------- | -------- | -------- |
| 0       | B000000 | N        | N        | N        | N        |
| 1       | B000001 | N        | N        | N        | Y        |
| 2       | B000010 | N        | N        | Y        | N        |
| 3       | B000011 | N        | N        | Y        | Y        |
| 4       | B000100 | N        | Y        | N        | N        |
| 5       | B000101 | N        | Y        | N        | Y        |
| 6       | B000110 | N        | Y        | Y        | N        |
| 7       | B000111 | N        | Y        | Y        | Y        |
| 8       | B000000 | Y        | N        | N        | N        |
| 9       | B000001 | Y        | N        | N        | Y        |
| 10      | B000010 | Y        | N        | Y        | N        |
| 11      | B000011 | Y        | N        | Y        | Y        |
| 12      | B000100 | Y        | Y        | N        | N        |
| 13      | B000101 | Y        | Y        | N        | Y        |
| 14      | B000110 | Y        | Y        | Y        | N        |
| 15      | B000111 | Y        | Y        | Y        | Y        |

#### $Stepper/DirInvert or $3 – Direction port invert, mask

This setting inverts the direction signal for each axis. By default, Grbl assumes that the axes move in a positive direction when the direction pin signal is low, and a negative direction when the pin is high. Often, axes don't move this way with some machines. This setting will invert the direction pin signal for those axes that move the opposite way.

As with Stepper/StepInvert, you can either list the axes by letter, or use the numeric bit mask form with the table above.

For example, to invert the Y axis direction only, send either `$Stepper/DirInvert=y` or `$3=2`.

#### $Stepper/EnableInvert or $4 - Step enable invert, boolean

By default, the stepper enable pin is high to disable and low to enable. If your setup needs the opposite, just invert the stepper enable pin by sending `$Stepper/EnableInvert=on` or `$4=1`. Disable with `$Stepper/EnableInvert=off` or `$4=0`. You may need a power cycle to load the change.

#### $Limits/Invert or $5 - Limit pins invert, boolean

By default, the limit pins are held normally-high with the Arduino's internal pull-up resistor. When a limit pin is low, Grbl interprets this as triggered. For the opposite behavior, just invert the limit pins by sending `$Limits/Invert=on` or `$5=1`. Disable with `$Limits/Invert=off` or `$5=0`. You may need a power cycle to load the change.

NOTE: For more advanced usage, the internal pull-up resistor on the limit pins may be disabled in config.h.

#### $Probe/Invert or $6 - Probe pin invert, boolean

By default, the probe pin is held normally-high with the Arduino's internal pull-up resistor. When the probe pin is low, Grbl interprets this as triggered. For the opposite behavior, just invert the probe pin by sending `$Probe/Invert=on` or `$6=1`. Disable with `$Probe/Invert=off` or  `$6=0`. You may need a power cycle to load the change.

#### $Report/Status or $10 - Status report, mask

This setting determines what Grbl real-time data it reports back to the user when a '?' status report is sent. This data includes current run state, real-time position, real-time feed rate, pin states, current override values, buffer states, and the g-code line number currently executing (if enabled through compile-time options).

By default, the new report implementation in Grbl v1.1+ will include just about everything in the standard status report. A lot of the data is hidden and will appear only if it changes. This increases efficiency dramatically over of the old report style and allows you to get faster updates and still get more data about your machine. The interface documentation outlines how it works and most of it applies only to GUI developers or the curious.

To keep things simple and consistent, Grbl v1.1 has only two reporting options. These are primarily here just for users and developers to help set things up.

- Position type may be specified to show either machine position (`MPos:`) or work position (`WPos:`), but no longer both at the same time. Enabling work position is useful in certain scenarios when Grbl is being directly interacted with through a serial terminal, but *machine position reporting should be used by default.*
- Usage data of Grbl's planner and serial RX buffers may be enabled. This shows the number of blocks or bytes available in the respective buffers. This is generally used to helps determine how Grbl is performing when testing out a streaming interface. *This should be disabled by default.*

Use the table below enables and disable reporting options. Simply add the values listed of what you'd like to enable, then save it by sending Grbl your setting value. For example, the default report with machine position and no buffer data reports setting is `$10=1`. If work position and buffer data are desired, the setting will be `$10=2`.

| Report Type   | Value | Description                                                  |
| ------------- | ----- | ------------------------------------------------------------ |
| Position Type | 0     | Enable `WPos:` Disable `MPos:`.                              |
| Position Type | 1     | Enable `MPos:`. Disable `WPos:`.                             |
| Buffer Data   | 2     | Enabled `Buf:` field appears with planner and serial RX available buffer. |

#### $GCode/JunctionDeviation or $11 - Junction deviation, mm

Junction deviation is used by the acceleration manager to determine how fast it can move through line segment junctions of a G-code program path. For example, if the G-code path has a sharp 10 degree turn coming up and the machine is moving at full speed, this setting helps determine how much the machine needs to slow down to safely go through the corner without losing steps.

How we calculate it is a bit complicated, but, in general, higher values gives faster motion through corners, while increasing the risk of losing steps and positioning. Lower values makes the acceleration manager more careful and will lead to careful and slower cornering. So if you run into problems where your machine tries to take a corner too fast, *decrease* this value to make it slow down when entering corners. If you want your machine to move faster through junctions, *increase* this value to speed it up. For curious people, hit this [link](http://t.co/KQ5BvueY) to read about Grbl's cornering algorithm, which accounts for both velocity and junction angle with a very simple, efficient, and robust method.

#### $ArcTolerance or $12 – Arc tolerance, mm

Grbl renders G2/G3 circles, arcs, and helices by subdividing them into teeny tiny lines, such that the arc tracing accuracy is never below this value. You will probably never need to adjust this setting, since `0.002mm` is well below the accuracy of most all CNC machines. But if you find that your circles are too crude or arc tracing is performing slowly, adjust this setting. Lower values give higher precision but may lead to performance issues by overloading Grbl with too many tiny lines. Alternately, higher values traces to a lower precision, but can speed up arc performance since Grbl has fewer lines to deal with.

For the curious, arc tolerance is defined as the maximum perpendicular distance from a line segment with its end points lying on the arc, aka a chord. With some basic geometry, we solve for the length of the line segments to trace the arc that satisfies this setting. Modeling arcs in this way is great, because the arc line segments automatically adjust and scale with length to ensure optimum arc tracing performance, while never losing accuracy.

#### $Report/Inches or $13 - Report inches, boolean

Grbl's real-time position reports provide user feedback on the current machine location, as well as, parameters for coordinate offsets and probing. By default, reports are in mm, but sending `$Report/Inches=on` or `$13=1` causes subsequent reports to be in inches. `$Report/Inches=off` or `$13=0` sets it back to mm.

#### $Limits/Soft or $20 - Soft limits, boolean

Soft limits is a safety feature to prevent your machine from traveling too far, beyond the limits of travel, thus crashing or breaking something expensive. It works by knowing the maximum travel limits for each axis and where Grbl is in machine coordinates. Whenever a new G-code motion is sent to Grbl, it checks whether or not you accidentally have exceeded your machine space. If you do, Grbl will issue an immediate feed hold wherever it is, shutdown the spindle and coolant, and then set the system alarm indicating the problem. Machine position will be retained afterwards, since it's not due to an immediate forced stop like hard limits.

NOTE: Soft limits requires homing to be enabled and accurate axis maximum travel settings, because Grbl needs to know where it is. `$Limits/Soft=on` or `$20=1` to enable, `$Limits/Soft=off` or `$20=0` to disable.

#### $Limits/Hard or $21 - Hard limits, boolean

Hard limit work basically the same as soft limits, but use physical switches instead. Basically you wire up some switches (mechanical, magnetic, or optical) near the end of travel of each axes, or where ever you feel that there might be trouble if your program moves too far to where it shouldn't. When the switch triggers, it will immediately halt all motion, shutdown the coolant and spindle (if connected), and go into alarm mode, which forces you to check your machine and reset everything.

To use hard limits with Grbl, the limit pins are held high with an internal pull-up resistor, so all you have to do is wire in a normally-open switch with the pin and ground, then enable hard limits with `$Limits/Hard=on` or `$21=1` (disable with `$Limits/Hard=off` or `$21=0`). We strongly advise taking electric interference prevention measures. If you want a limit for both ends of travel of one axes, just wire in two switches in parallel with the pin and ground, so if either one of them trips, it triggers the hard limit.

Keep in mind, that a hard limit event is considered to be critical event, where steppers immediately stop and will have likely have lost steps. Grbl doesn't have any feedback on position, so it can't guarantee it has any idea where it is. So, if a hard limit is triggered, Grbl will go into an infinite loop ALARM mode, giving you a chance to check your machine and forcing you to reset Grbl. Remember it's a purely a safety feature.

#### $Homing/Enable $22 - Homing cycle, boolean

Ahh, homing. For those just initiated into CNC, the homing cycle is used to accurately and precisely locate a known and consistent position on a machine every time you start up your Grbl between sessions. In other words, you know exactly where you are at any given time, every time. Say you start machining something or are about to start the next step in a job and the power goes out, you re-start Grbl and Grbl has no idea where it is due to steppers being open-loop control. You're left with the task of figuring out where you are. If you have homing, you always have the machine zero reference point to locate from, so all you have to do is run the homing cycle and resume where you left off.

To set up the homing cycle for Grbl, the limit switches must be in a fixed position that won't get bumped or moved, otherwise your reference point will be inconsistent. The usual locations for limit switches are at the farthest points in +x, +y, +z of each axes. Wire your limit switches in with the limit pins, add a recommended RC-filter to help reduce electrical noise, and enable homing. If you're curious, you can use your limit switches for both hard limits AND homing. They play nice with each other.

Prior to trying the homing cycle for the first time, make sure you have setup everything correctly, otherwise homing may behave strangely. First, ensure your machine axes are moving in the correct directions per Cartesian coordinates (right-hand rule). If not, fix it with the `$3` direction invert setting. Second, ensure your limit switch pins are not showing as 'triggered' in Grbl's status reports. If are, check your wiring and settings. Finally, ensure your max travel settings (`$13x' or `<axis>/MaxTravel`) are somewhat accurate (within 20%), because Grbl uses these values to determine how far it should search for the homing switches.

By default, Grbl's homing cycle moves the Z-axis positive first to clear the workspace and then moves both the X and Y-axes at the same time in the positive direction. To set up how your homing cycle behaves, there are more Grbl settings down the page describing what they do (and compile-time options as well.)

Also note - when homing is enabled, Grbl will lock out all G-code commands until you perform a homing cycle. Meaning no axes motions, unless the lock is disabled ($X) but more on that later. Most, if not all CNC controllers, do something similar, as it is mostly a safety feature to prevent users from making a positioning mistake, which is very easy to do and be saddened when a mistake ruins a part. If you find this annoying or find any weird bugs, please let us know and we'll try to work on it so everyone is happy. :)

NOTE: Check out config.h for more homing options for advanced users. You can disable the homing lockout at startup, configure which axes move first during a homing cycle and in what order, and more.

#### $Homing/DirInvert or $23 - Homing dir invert, mask

By default, Grbl assumes your homing limit switches are in the positive direction, first moving the z-axis positive, then the x-y axes positive before trying to precisely locate machine zero by going back and forth slowly around the switch. If your machine has a limit switch in the negative direction, the homing direction mask can invert the axes' direction. It works just like the step port invert and direction port invert masks, where all you have to do is send the value in the table to indicate what axes you want to invert and search for in the opposite direction.

#### $Homing/Feed or $24 - Homing feed, mm/min

The homing cycle first searches for the limit switches at a higher seek rate, and after it finds them, it moves at a slower feed rate to home into the precise location of machine zero. Homing feed rate is that slower feed rate. Set this to whatever rate value that provides repeatable and precise machine zero locating.

#### $Homing/Seek or $25 - Homing seek, mm/min

Homing seek rate is the homing cycle search rate, or the rate at which it first tries to find the limit switches. Adjust to whatever rate gets to the limit switches in a short enough time without crashing into your limit switches if they come in too fast.

#### $Homing/Debounce or $26 - Homing debounce, milliseconds

Whenever a switch triggers, some of them can have electrical/mechanical noise that actually 'bounce' the signal high and low for a few milliseconds before settling in. To solve this, you need to debounce the signal, either by hardware with some kind of signal conditioner or by software with a short delay to let the signal finish bouncing. Grbl performs a short delay, only homing when locating machine zero. Set this delay value to whatever your switch needs to get repeatable homing. In most cases, 5-25 milliseconds is fine.

#### $Homing/Pulloff or $27 - Homing pull-off, mm

To play nice with the hard limits feature, where homing can share the same limit switches, the homing cycle will move off all of the limit switches by this pull-off travel after it completes. In other words, it helps to prevent accidental triggering of the hard limit after a homing cycle. Make sure this value is large enough to clear the limit switch. If not, Grbl will throw an alarm error for failing to clear it.

#### $Homing/Squaring - Axes that are squared during homing

See [Motor Ganging and Axis Squaring](Motor-Ganging-and-Axis-Squaring).  Under certain conditions, axes that have two motors can be automatically "squared" while homing, so the gantry is perpendicular to its orthogonal axis.  This setting lists the axes that should be squared in this way.

#### $GCode/MaxS or $30 - Max spindle speed, RPM

This setting is nominally in RPM, but RPM is meaningless for "spindles" like lasers, so the more general interpretation is "this is the value of the GCode S word" that corresponds to "full speed" or "full power".

This sets the spindle speed for the maximum 5V PWM pin output. For example, if you want to set 10000 RPM at 5V, program `$30=10000`. For 255 RPM at 5V, send `$GCode/MaxS` or `$30=255`. If a program tries to set a higher spindle RPM greater than the MaxS value, Grbl will just output the max 5V, since it can't go any faster. By default, Grbl linearly relates the max-min RPMs to 5V-0.02V PWM pin output in 255 equally spaced increments. When the PWM pin reads 0V, this indicates spindle disabled. Note that there are additional configuration options are available in config.h to tweak how this operates.

#### $GCode/MinS or $31 - Min spindle speed, RPM

This setting is nominally in RPM, but RPM is meaningless for "spindles" like lasers, so the more general interpretation is "this is the value of the GCode S word" that corresponds to "lowest speed" or "lowest power".

This sets the spindle speed for the minimum 0.02V PWM pin output (0V is disabled). Lower RPM values are accepted by Grbl but the PWM output will not go below 0.02V, except when RPM is zero. If zero, the spindle is disabled and PWM output is 0V.

#### $GCode/LaserMode or $32 - Laser mode, boolean

When laser mode is enabled, Grbl will move continuously through consecutive `G1`, `G2`, or `G3` motion commands when programmed with a `S` spindle speed (laser power). The spindle PWM pin (which controls the laser power level) will be updated instantaneously through each motion without stopping. Laser mode also affects the interpretation of M4 - please read the [GRBL laser documentation](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Laser-Mode) and your laser device documentation prior to using this mode.

_**Lasers are very dangerous. They can instantly damage your vision permanently and cause fires. Grbl does not assume any responsibility for any issues the firmware may cause, as defined by its GPL license.**_

When laser mode is disabled, Grbl will stop motion with every `S` spindle speed command. This is the default operation of a milling machine to allow a pause to let the spindle change speeds.

#### $Spindle/Type - Spindle Type

The xPro V5 supports several different kinds of spindles.  The values for this setting include:
* NONE - No spindle
* RELAY - A spindle that can be turned on or off but not otherwise controlled
* PWM - A spindle whose speed can be controlled via Pulse Width Modulation
* LASER - A laser whose power level can be controlled via Pulse Width Modulation
* DAC (0-10V) - A spindle whose speed can be controlled via an analog voltage driven by a Digital to Analog converter


#### $Spindle/PWM/Frequency or $33 - Spindle PWM Frequency (extended setting)

This is the frequency of the Spindle/laser PWM. The ESP32 must be restarted for this to take effect.

#### $Spindle/PWM/Off or $34 - Spindle PWM Off  (extended setting)

This is the PWM off value as a percentage. This is typically left at the default of 0. It is used on ESC type spindles to keep a pulse going in the off state to keep the ESC happy.

#### $Spindle/PWM/Min or $35 - Spindle PWM Min (extended setting)

This is the PWM minimum value as a percentage. This is typically left at the default of 0. Some lasers and spindles will not start until some minimum PWM is reached.

#### $Spindle/PWM/Max or $36 - Spindle PWM Max  (extended setting)

This is the PWM maximum value as a percentage. This is typically left at the default of 100.0

####  $X/StepsPerMm thru $C/StepsPerMm or $100 thru $105 – [X,Y,Z,A,B,C] steps/mm

Grbl needs to know how far each step will move the tool on a given machine. To calculate steps/mm for an axis of your machine you need to know:

- The mm traveled per revolution of your stepper motor. This is dependent on your belt drive gears or lead screw pitch.
- The full steps per revolution of your steppers (typically 200)
- The microsteps per step of your controller (typically 1, 2, 4, 8, or 16). *Tip: Using high microstep values (e.g., 16) can reduce your stepper motor torque, so use the lowest that gives you the desired axis resolution and comfortable running properties.*

The steps/mm can then be calculated like this: `steps_per_mm = (steps_per_revolution*microsteps)/mm_per_rev`

Compute this value for every axis and write these settings to Grbl.

#### $X/MaxRate thru $C/MaxRate  or $110 thru $115 – [X,Y,Z,A,B,C] Max rate, mm/min

This sets the maximum speed each axis can move. Whenever Grbl plans a move, it checks whether or not the move causes any one of these individual axes to exceed their max rate. If so, it will slow down the motion to ensure none of the axes exceed their max rate limits. This means that each axis has its own independent speed, which is extremely useful for limiting the typically slower Z-axis.

The simplest way to determine these values is to test each axis one at a time by slowly increasing max rate settings and moving it. For example, to test the X-axis, send Grbl something like `G0 X50` with enough travel distance so that the axis accelerates to its max speed. You'll know you've hit the max rate threshold when your steppers stall. It will make a bit of noise, but shouldn't hurt your motors. Enter a setting a 10-20% below this value, so you can account for wear, friction, and the mass of your workpiece/tool. Then, repeat for your other axes.

NOTE: This max rate setting also sets the G0 seek rates.

#### $X/Acceleration thru $C/Acceleration or $120 thru $125 – [X,Y,Z,A,B,C] Acceleration, mm/sec^2

This sets the axes acceleration parameters in mm/second/second. Simplistically, a lower value makes Grbl ease slower into motion, while a higher value yields tighter moves and reaches the desired feed rates much quicker. Much like the max rate setting, each axis has its own acceleration value and are independent of each other. This means that a multi-axis motion will only accelerate as quickly as the lowest contributing axis can.

Again, like the max rate setting, the simplest way to determine the values for this setting is to individually test each axis with slowly increasing values until the motor stalls. Then finalize your acceleration setting with a value 10-20% below this absolute max value. This should account for wear, friction, and mass inertia. We highly recommend that you dry test some G-code programs with your new settings before committing to them. Sometimes the loading on your machine is different when moving in all axes together.

#### $X/MaxTravel thru $C/MaxTravel or $130 thru $135 – [X,Y,Z,A,B,C] Max travel distance, mm

This sets the maximum travel distance from end to end for each axis in mm. This is only useful if you have soft limits (and homing) enabled, as this is only used by Grbl's soft limit feature to check if you have exceeded your machine limits with a motion command. **This is always a positive number**. The positive and negative limits are determined using this number after homing.

**Important Notes:** Changing this number will likely affect your work offsets (G54-G59). Consider resetting your work offsets with $rst=# any time you change the travel. If you clear a homing alarm with $X rather than home, the soft limits will not have accurate position information and not function properly.

#### $X/Current/Run or $C/Current/Run $140 thru $145 – [X,Y,Z,A,B,C]  Stepper Run Current (extended settings)

This is the run current in amperes for Trinamic SPI stepper drivers.  If you have such drivers, Grbl will use this value to configure them at startup.

#### $X/Current/Hold thru $C/Current/Hold or $150 thru $155 – [X,Y,Z,A,B,C]  Stepper Idle Current (extended settings)

This is the hold current in amperes for Trinamic SPI stepper drivers.  If you have such drivers, Grbl will use this value to configure them at startup.

#### $X/Microsteps thru $C/Microsteps or $160 thru $165 – [X,Y,Z,A,B,C]  Microstepping (extended settings)

This is the microstepping ratio for Trinamic SPI stepper drivers.  If you have such drivers, Grbl will use this value to configure them at startup.

#### $X/StallGuard thru $C/StallGuard or $170 thru $175 – [X,Y,Z,A,B,C]  Stall Guard Value (extended settings)

This the Stallguard register setting for Trinamic SPI stepper drivers.  If you have such drivers, Grbl will use this value to configure them at startup.

## Motivation for the New Settings Mechanism

In classic Grbl, settings had numerical names and values, for example $3=6 mean "invert the directions of the Y and Z axes".  The xPro V5 now has an improved settings mechanism with these advantages:

* Settings have meaningful textual names, for example "Stepper/DirInvert" controls inversion of the motor direction signals.  (This is the same setting as $3 in classic Grbl)
* Setting values that are not fundamentally numerical can be expressed with text.  For example, the value for Stepper/DirInvert can be expressed either as 6 - a bitmask representing Y (2) and Z (4) - or by writing YZ.
* The classic Grbl numbered settings are retained for compatibility with existing GCode senders.  You can use either the old form or the new form to display and modify the numbered settings.
* The storage format for settings is designed to be stable across firmware updates.  Previously, some kinds of firmware changes could invalidate the settings store, requiring you to reestablish your machine configuration.
* The naming scheme for settings is hierarchical so that related settings are easily discerned, and so that it is relatively easy to come up with names for newly invented settings.
* The ESP3D_WebUI settings, of the form [ESP400], are incorporated into the new scheme.  Those settings now have textual names in addition to the ESPnnn names.  For backwards compatibility, the [...] format for expressing settings is provided in addition to the $...= format.
* There is an extended syntax for displaying and changing settings.  The new syntax has partial matching so you can look at related groups of settings with a single command.
* Classic Grbl has some "commands", for example $H, that have similar syntax as settings, but instead of storing persistent values, they perform some immediate action.  Those commands are integrated into the new scheme and additional ones are provided.
* A goal of the new scheme is to allow many aspects of the system to be controlled by settings - aspects that use to require recompilation.  The objective is to provide a single firmware build that can be configured for a wide variety of machines simply by changing settings.  To support that goal, the code that manages settings and commands was rewritten from the ground up, using object-oriented techniques, so that it is much easier to maintain and to add new settings and commands.
* Newly-invented settings will have only textual names; they will not be assigned setting numbers or ESPnnn names.  Numeric names have little mnemonic value, while managing a numeric namespace becomes very difficult over time. 
