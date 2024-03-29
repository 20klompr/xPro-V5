# **For firmware update/upgrade we recommend upgrading to Fluid NC**

**PLEASE NOTE: The information below is only being maintained as a reference. All future firmware updates are targeted at FluidNC.**

## Uncompiled Firmware

Source code can be found here: https://github.com/Spark-Concepts/xPro-V5-Firmware

## Precompiled Firmware

**Default xProV5 firmware is:**  ***CNC_xPRO_V5_XYYZ_NO.bin*** *(this denoted a 3 axis dual Y motor machine, with a real-time selectable spindle (Laser, pwm, RS485), and a normally open door switch)*

**The precompiled firmware is named to clearly identify specific compile time options** - _you can find the most ***up-to-date*** firmware here:_ https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware

##Firmware naming convention 

CNC_xPRO_V5_  | MotorAssignment_ | SpindleType_ | DoorSwitchType.bin
------------- | -------------|------------- | -------------
" | XYYZ | ---  | NO
" | XYYZ | ---  | NC
" | XYYZ | BM3D | NO
" | XYYZ | BM3D | NC

**MotorAssignment:** *the new firmware is currently for XYYZ (3 axis, ganged Y) with XYAZ (4 axis) on the way*

**SpindleType:** *the only specialized spindle option is now BM3D which is for use with the BM3D laser modules.  All other types can define the spindle real-time using the $Spindle/Type = "  "setting*    

 **DoorSwitchType** *controls whether or not the door/Estop switch is Normally Open (NO), or Normally Closed (NC)*

**NOTE:**
- ***If you select firmware "...NC.bin" - a [Normally Closed switch](https://github.com/Spark-Concepts/xPro-V5/wiki/Front_Panel#doorestop) will need to be attached at all times or the system will not run; if the switch is incorrect on startup you will need to correct the error and reset the controller.  The WebUI will no longer lock you out for an unsafe start, you will see a DOOR:1  error upon opening the WebUI but all functionality of the WebUI will be locked until the door condition is met.***


- ***With the "...NO.bin" - a Normally Open switch is NOT required for normal operation, assuming the E-Stop/Door normally open condition is met upon system initialization.***

## Older Firmware naming convention for reference

CNC_xPRO_V5_  | MotorAssignment_ | SpindleType_ | DoorSwitchType.bin
------------- | -------------|------------- | -------------
" | XYYZ | PWM | NO
" | XYAZ | PWM | NO
" | XYYZ | PWM | NC
" | XYAZ | PWM | NC
" | XYYZ | 485 | NO
" | XYAZ | 485 | NO
" | XYYZ | 485 | NC
" | XYAZ | 485 | NC

**MotorAssignment:** *the options are XYYZ (3 axis, ganged Y) and XYAZ (4 axis)*

**SpindleType:** *the options are PWM which allows for 0-5V or 0-10V spindle (or laser) and 485 allows for the use of an RS485 enabled VFD* (only available for HY series VFDs)   

 **DoorSwitchType** *controls whether or not the door/Estop switch is Normally Open (NO), or Normally Closed (NC)*


## OTA (Over The Air) Firmware Updates

The fastest and easiest way to update firmware is by using Over The Air (OTA) updates. 

***

* First connect to your xPro V5 wifi and [open the WebUI](https://github.com/Spark-Concepts/xPro-V5/wiki/Controlling-the-xPRO-V5-(WebUI))  

* In the WebUI, navigate to the ESP3D Menu

![](https://github.com/Spark-Concepts/xPro-V5/blob/main/images/OTA1.png)



***

* Select the Firmware Update Button

![](https://github.com/Spark-Concepts/xPro-V5/blob/main/images/OTA2.png)



***

* Click "Select file" and choose new firmware (.bin), then select "Update".  The new firmware will load and the xPro V5 will reboot.  

![](https://github.com/Spark-Concepts/xPro-V5/blob/main/images/OTA3.png) 
 
***

### Note: If you load new firmware and things are behaving strangely, it is recommended to issue a $RST=* command to load the latest default settings.