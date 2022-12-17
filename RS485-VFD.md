For debugging, the best route is to use CNCjs and check the start-up/initialization messaging that comes out over USB.
1) Power on your VFD, power on xPRO
2) Open CNCjs, connect to the Silicon Labs port
3) Hit the physical **Reset** button on the xPRO

This will spit out a bunch of information - you are looking for the spindle information (directly below the trinamic self-tests)
If there is an issue with the communication you will see something like this:
![image](https://user-images.githubusercontent.com/62680473/114034261-055f4c00-984c-11eb-85b8-b5a0ad46295a.png)

While a successful start will look like this:
![image](https://user-images.githubusercontent.com/62680473/114034313-127c3b00-984c-11eb-8b63-0e7afa6d24e5.png)

If still unsuccessful a few things to check:
1) One of the RS485 specific firmwares are loaded (CNC_xPRO_V5_....**_485_**..) 
2) The switch to select the EN/PWM or RS485 mode on the xPRO V5 uses a long actuator and will sometimes not fully seat.  Flip the switch back to the EN/PWM side then flip it to the RS485 side (use a small driver to make sure the base of the actuator has flipped fully) 


All, 

Please read below in it's entirety (also for 220V VFD user please see note at the end of this post) - also, if at any point I'm not clear or what I'm saying doesn't make sense, please repost and I will do my best to explain in more detail.   

we're seeing a-lot of issues regarding RS485 communication errors.... most all issues regarding this revolve around folks with 220VAC mains and 90% of the issues are related to ground loops, RF coupling and/or faulty wiring or loose/misplaced grounds/neutrals: [https://github.com/Spark-Concepts/xPro-V5/issues/116#issuecomment-1154745817](https://github.com/Spark-Concepts/xPro-V5/issues/116#issuecomment-1154745817), [https://github.com/Spark-Concepts/xPro-V5/issues/157#issuecomment-1180644324](https://github.com/Spark-Concepts/xPro-V5/issues/157#issuecomment-1180644324). These can sometimes be extremely frustrating and often difficult to troubleshoot . Some folks have also used RS485 to USB converters to test communications between the VFD without issue - in this situation your computer's power supply is typically isolated from mains power; in other words  you eliminate the potential for ground loop disturbances on the RS485 signal path 

RS485 is a differential pair, meaning the differential voltage between RS485A and RS458B _and not with referenced to ground_); this means common mode noise (noise the the same on both A and B and can dance around in sync if measured with an oscilloscope relative to ground - however if measured differentially (voltage on A - B) you should get a nice clean square wave... again this all falls short when coupled to ground.

 - RS485 is also bi-directional, meaning it sends and receives on the same set of wires

A few things to check first:
- ensure your [firmware](https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware) is up-to-date
- RS485 wiring is less than 1 meter in length, twisted, and connected properly (resistors are optional)
![image](https://user-images.githubusercontent.com/8650709/183530101-65122f53-1293-4272-8e1c-271b757b6c80.png)
- Verify the **PWM-RS485** switch<sub>(1)</sub> is set to **RS485**
- Verify both the red and green LED's below the PWM-RS485 switch are flashing periodically
- Sometimes the PWM-RS485 switch contacts may be contaminated with flux as received from manufacturing; try using contact cleaner or applying 99% isopropyl alcohol with a dropper to the switch (working it back an forth a few times) _ONLY USE (99% IPA - ANYTHING LESS THAT IS DILUTED WITH WATER AND WILL HAVE THE ADVERSE EFFECT)_
- verify the spindle type is defined - on the command window send the following: ```$Spindle/Type=HUANYANG``` if that doesn't work, try ```$Spindle/Type=H2A```
- Verify your VFD is configured properly using the procedure in the [Huanyang-VFD-manual](https://github.com/Spark-Concepts/xPro-V5/files/6247012/Huanyang-VFD-manual.pdf) _PLEASE REFER TO THE MANUAL SPECIFIC TO YOUR MODEL VFD; not all VFD's are the same despite the name on it_

Set the following register values via the VFD touchpad (should be the same on most VFD's - VERIFY ON YOUR SPECIFIC MODEL VFD):
| Register  | Value | Description |
| :-------: | :-----: | ------------- |
 | PD000 | 0 | **unlock parameters** |
 | PD001 | 2 | **Command source is RS485** |
 | PD002 | 2 | **Speed source is RS485** |
 | PD163 | 1 | **Communications address 1** |
 | PD164 | 1 | **9600 b/s** |
 | PD165 | 3 | **8 Bit No Parity - RTU** |
 | PD000 | 1 | **lock parameters** |
 
 _Also check the following settings (these will setting will vary based on the values indicated on the motor's nameplate and your mains voltage 110VAC vs 220VAC)_
 
| Register  | Description |
| :-------: | ----- |
| PD004 | **Base frequency as listed on spindle** _(typically 400)_ |
| PD005 | **Maximum frequency Hz** _(typical value for spindles is 400)_ |
| PD011 | **Min speed** _(recommended air-cooled=120 water=100)_ |
| PD014 | **Acceleration time** _(test to optimize)_ |
| PD015 | **Deceleration time** _(test to optimize)_ |
| PD023 | **Reverse run enabled** _(set to 1)_ |
| PD141 | **Spindle max rated voltage** _(typically 220)_ |
| PD142 | **Max rated motor current** _(0.8kw=3.7, 1.5kw=7.0, 2.2kw=??)_ |
| PD143 | **Motor poles** _(typically 2 or 4)_ |
| PD144 | **Rated motor revolution at 50Hz** _(typically 3000 @ 50Hz and 24000 @ 400Hz)_ |
 
- If you're still believe your having issues and trying to narrow down ground loop or issues otherwise (partial messages received etc..), load the following RS485 debug firmware: [CNC_xPRO_V5_XYYZ_NO_485_debug.bin](https://github.com/Spark-Concepts/xPro-V5/blob/main/Firmware/CNC_xPRO_V5_XYYZ_NO_485_debug.bin)
  - The RS485 Debug bin file is for debugging VFD RS-485 communication issues and requires USB connection and CNCJS or USB serial sender program
  - this firmware assumes/requires a NO (normally open) e-stop (unplug estop button after debug firmware is loaded)
  - Once connected to the X-Pro using CNCJS will echo commands sent to and from the VFD via RS-485
  - The terminal window will display messages from the X-Pro as Tx:....... and messages recieved from the VFD as Rx:........

_as a side note, we (Spark Concepts) seldom run into issues with VFD's running on 110V mains.  Since we are US based (110V mains), it's impossible to test the 220V 100% representative. Therefore we are in the process of acquiring a 220V voltage converter and 220V VFD since the 220V model VFD is different from the 120V version_

 - Our to-do plan: reverse engineer the RS485 signal path on the most popular 220V model VFD's and will determine if/where coupling may be occurring and  try to reproduce/isolate any potential issues, and provide potential solutions. 

_as always, if you've tried all the steps above and are at your wits end, please contact us via the "Contact Us" on the Spark-concepts.com website and we will do our best to remedy your situation_

The second example is not using a twisted pair or resistors. You are accurate, the twist does improve the signal quality, but not nearly the amount the is1. Placing an Oscilloscope on the RS485 output signal on the xPro ```DISCONNECTED from VFD```:
    ![DS1Z_QuickPrint35](https://user-images.githubusercontent.com/8650709/186558090-bc81d4fa-69fa-41a4-9bac-8a6d5aec7481.png)


2. Measuring RS485 signal at the 220V VFD while connected to the xPro ```WITHOUT RS485 isolation```:
    ![DS1Z_QuickPrint32](https://user-images.githubusercontent.com/8650709/186558074-25fe2e6d-260e-485d-8505-7d62c3331abb.png)olator does (difference is illustrated between image 2 and image 3 above). Also note, even with the degraded signal quality (image 2),  I was still able to communicate with the spindle. 

***NOTE: The resistors were minimally effective  in reducing noise with cable lengths less than 1-meter long. (resistors help eliminate transmission line reflections that may degrade the signal; typically prominent with longer cable lengths***

Ultimately, the Huanyang VFD's due to the nature of how they are constructed, share a common ground and can unintentionally create ground loops thus permitting radiated noise to degrade the rs485 signal despite all best efforts.
![image](https://user-images.githubusercontent.com/8650709/186806336-f848f047-929f-4ca1-a04a-7f634ee90376.png)
Ultimately to mitigate this scenario I reccomend implementing an RS485 isolator as shown above.

I will post a much more extensive write-up this weekend illustrating the effects of each wiring scheme along with actual pictures showing the configuration of each.

### Spindle Types

Spindle classes are defined in the firmware (the default firmware on the xProV5 is **PWM**). The Spindle Type is dynamically selected by entering ```$Spindle/Type=XXXXX```' in the command line. There are two classes of Spindles with two separate Spindle Types.

**For the PWM and Laser Spindle classes:**
```
$Spindle/Type=PWM
$Spindle/Type=LASER 
```
**For the RS485 VFD classes:**
```