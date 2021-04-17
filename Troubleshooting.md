The xPRO V5 will report issues using Alarms and Errors.  Alarms report issues related to motion - hard limits hit, probe not found, etc.  Errors report issues with processes - invalid Gcode, Sd card issues, etc.  Below you can find a list of all the Alarm and Error IDs and the associated meanings.  

***

## Alarms 
|Alarm Code| Alarm Message| Alarm Description|
|:-:|:-:|:-:|
| 1 | Hard limit | Hard limit has been triggered. Machine position is likely lost due to sudden halt. Re-homing is highly recommended. |
| 2 | Soft limit | Soft limit alarm. G-code motion target exceeds machine travel. Machine position retained. Alarm may be safely unlocked. |
| 3 | Abort during cycle | Reset while in motion. Machine position is likely lost due to sudden halt. Re-homing is highly recommended. |
| 4 | Probe fail | Probe fail. Probe is not in the expected initial state before starting probe cycle when G38.2 and G38.3 is not triggered and G38.4 and G38.5 is triggered. |
| 5 | Probe fail | Probe fail. Probe did not contact the workpiece within the programmed travel for G38.2 and G38.4. |
| 6 | Homing fail | Homing fail. The active homing cycle was reset. |
| 7 | Homing fail | Homing fail. Safety door was opened during homing cycle. |
| 8 | Homing fail | Homing fail. Pull off travel failed to clear limit switch. Try increasing pull-off setting or check wiring. |
| 9 | Homing fail | Homing fail. Could not find limit switch within search distances. Try increasing max travel, decreasing pull-off distance, or check wiring.|

***


## Errors

|Error Code |Error Message |Error Description|
|:-:|:-:|:--|
|1|Expected command letter|G-code words consist of a letter and a value. Letter was not found.|
|2|Bad number format|Missing the expected G-code word value or numeric value format is not valid.|
|3|Invalid statement|Grbl '$' system command was not recognized or supported.|
|4|Value < 0|Negative value received for an expected positive value.|
|5|Setting disabled|Homing cycle failure. Homing is not enabled via settings.|
|6|Value < 3 usec|Minimum step pulse time must be greater than 3usec.|
|7|EEPROM read fail. Using defaults|An EEPROM read failed. Auto-restoring affected EEPROM to default values.|
|8|Not idle|Grbl '$' command cannot be used unless Grbl is IDLE. Ensures smooth operation during a job.|
|9|G-code lock|G-code commands are locked out during alarm or jog state.|
|10|Homing not enabled|Soft limits cannot be enabled without homing also enabled.|
|11|Line overflow|Max characters per line exceeded. Received command line was not executed.|
|12|Step rate > 30kHz|Grbl '$' setting value cause the step rate to exceed the maximum supported.|
|13|Check Door|Safety door detected as opened and door state initiated.|
|14|Line length exceeded|Build info or startup line exceeded EEPROM line length limit. Line not stored.|
|15|Travel exceeded|Jog target exceeds machine travel. Jog command has been ignored.|
|16|Invalid jog command|Jog command has no '=' or contains prohibited g-code.|
|17|Setting disabled|Laser mode requires PWM output.|
|18|No Homing/Cycle defined in settings|Set some $homing/CycleX= Settings|
|20|Unsupported command|Unsupported or invalid g-code command found in block.|
|21|Modal group violation|More than one g-code command from same modal group found in block.|
|22|Undefined feed rate|Feed rate has not yet been set or is undefined.|
|23|Invalid gcode ID:23|G-code command in block requires an integer value.|
|24|Invalid gcode ID:24|More than one g-code command that requires axis words found in block.|
|25|Invalid gcode ID:25|Repeated g-code word found in block.|
|26|Invalid gcode ID:26|No axis words found in block for g-code command or current modal state which requires them.|
|27|Invalid gcode ID:27|Line number value is invalid.|
|28|Invalid gcode ID:28|G-code command is missing a required value word.|
|29|Invalid gcode ID:29|G59.x work coordinate systems are not supported.|
|30|Invalid gcode ID:30|G53 only allowed with G0 and G1 motion modes.|
|31|Invalid gcode ID:31|Axis words found in block when no command or current modal state uses them.|
|32|Invalid gcode ID:32|G2 and G3 arcs require at least one in-plane axis word.|
|33|Invalid gcode ID:33|Motion command target is invalid.|
|34|Invalid gcode ID:34|Arc radius value is invalid.|
|35|Invalid gcode ID:35|G2 and G3 arcs require at least one in-plane offset word.|
|36|Invalid gcode ID:36|Unused value words found in block.|
|37|Invalid gcode ID:37|G43.1 dynamic tool length offset is not assigned to configured tool length axis.|
|38|Invalid gcode ID:38|Tool number greater than max supported value.|
|39|Parameter P exceeded max ID:39|Parameter P exceeded max|
|60|SD mount| SD card failed to mount |
|61|SD reading|SD card failed to open file for reading |
|62|SD opening | SD card failed to open directory |
|63|SD diectory | SD Card directory not found |
|64|SD file| SD Card file empty|
|70|Bluetooth failed| Bluetooth failed to start|

## Console Startup Message

### xPro Status and configuration (viewed using USB/serial console upon xPro reset):
- COM port & baud rate
- Firmware version
- Axis count
- Driver type, status, and axis GPIO assignment
- Miscellaneous GPIO assignment

```
CNCjs 1.9.15 [Grbl]
Connected to COM10 with a baud rate of 115200
ets Jun  8 2016 00:22:57
rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:1
load:0x3fff0018,len:4
load:0x3fff001c,len:1216
ho 0 tail 12 room 4
load:0x40078000,len:9720
ho 0 tail 12 room 4
load:0x40080400,len:6352
entry 0x400806b8
[MSG:Grbl_ESP32 Ver 1.3a Date 20201022]
[MSG:Compiled with ESP32 SDK:v3.2.3-14-gd3e562907]
[MSG:Using machine:CNC_xPRO_V5_XYYZ]
[MSG:Axis count 3]
[MSG:RMT Steps]
[MSG:Init Motors]
[MSG:TMCStepper Library Ver. 0x000701]
[MSG:X  Axis Trinamic TMC5160 Step:GPIO(12) Dir:GPIO(14) CS:GPIO(17) Disable:None Index:1 Limits(0.000,300.000)]
[MSG:X  Axis Trinamic driver test passed]
[MSG:Y  Axis Trinamic TMC5160 Step:GPIO(27) Dir:GPIO(26) CS:GPIO(17) Disable:None Index:2 Limits(0.000,300.000)]
[MSG:Y  Axis Trinamic driver test passed]
[MSG:Y2 Axis Trinamic TMC5160 Step:GPIO(33) Dir:GPIO(32) CS:GPIO(17) Disable:None Index:3 Limits(0.000,300.000)]
[MSG:Y2 Axis Trinamic driver test passed]
[MSG:Z  Axis Trinamic TMC5160 Step:GPIO(15) Dir:GPIO(2) CS:GPIO(17) Disable:None Index:4 Limits(-300.000,0.000)]
[MSG:Z  Axis Trinamic driver test passed]
[MSG:PWM spindle Output:GPIO(25), Enbl:GPIO(4), Dir:None, Freq:5000Hz, Res:13bits]
[MSG:Local access point CNC_xPRO_V5 started, 192.168.0.1]
[MSG:Captive Portal Started]
[MSG:HTTP Started]
[MSG:TELNET Started 23]
[MSG:Flood coolant on pin GPIO(21)]
[MSG:Mist coolant on pin GPIO(21)]
[MSG:X  Axis limit switch on pin GPIO(35)]
[MSG:Y  Axis limit switch on pin GPIO(34)]
[MSG:Z  Axis limit switch on pin GPIO(39)]
[MSG:Probe on pin GPIO(22)]
```

### xPro Status and configuration displayed with 24V powered off (viewed using USB/serial console):
```
CNCjs 1.9.15 [Grbl]
Connected to COM10 with a baud rate of 115200
ets Jun  8 2016 00:22:57
rst:0x1 (POWERON_RESET),boot:0x13 (SPI_FAST_FLASH_BOOT)
configsip: 0, SPIWP:0xee
clk_drv:0x00,q_drv:0x00,d_drv:0x00,cs0_drv:0x00,hd_drv:0x00,wp_drv:0x00
mode:DIO, clock div:1
load:0x3fff0018,len:4
load:0x3fff001c,len:1216
ho 0 tail 12 room 4
load:0x40078000,len:9720
ho 0 tail 12 room 4
load:0x40080400,len:6352
entry 0x400806b8
[MSG:Grbl_ESP32 Ver 1.3a Date 20201022]
[MSG:Compiled with ESP32 SDK:v3.2.3-14-gd3e562907]
[MSG:Using machine:CNC_xPRO_V5_XYYZ]
[MSG:Axis count 3]
[MSG:RMT Steps]
[MSG:Init Motors]
[MSG:TMCStepper Library Ver. 0x000701]
[MSG:X  Axis Trinamic TMC5160 Step:GPIO(12) Dir:GPIO(14) CS:GPIO(17) Disable:None Index:1 Limits(0.000,300.000)]
[MSG:X  Axis Trinamic driver test failed. Check motor power]
[MSG:Y  Axis Trinamic TMC5160 Step:GPIO(27) Dir:GPIO(26) CS:GPIO(17) Disable:None Index:2 Limits(0.000,300.000)]
[MSG:Y  Axis Trinamic driver test failed. Check motor power]
[MSG:Y2 Axis Trinamic TMC5160 Step:GPIO(33) Dir:GPIO(32) CS:GPIO(17) Disable:None Index:3 Limits(0.000,300.000)]
[MSG:Y2 Axis Trinamic driver test failed. Check motor power]
[MSG:Z  Axis Trinamic TMC5160 Step:GPIO(15) Dir:GPIO(2) CS:GPIO(17) Disable:None Index:4 Limits(-300.000,0.000)]
[MSG:Z  Axis Trinamic driver test failed. Check motor power]
[MSG:PWM spindle Output:GPIO(25), Enbl:GPIO(4), Dir:None, Freq:5000Hz, Res:13bits]
[MSG:Local access point CNC_xPRO_V5 started, 192.168.0.1]
[MSG:Captive Portal Started]
[MSG:HTTP Started]
[MSG:TELNET Started 23]
[MSG:Flood coolant on pin GPIO(21)]
[MSG:Mist coolant on pin GPIO(21)]
[MSG:X  Axis limit switch on pin GPIO(35)]
[MSG:Y  Axis limit switch on pin GPIO(34)]
[MSG:Z  Axis limit switch on pin GPIO(39)]
[MSG:Probe on pin GPIO(22)]
```