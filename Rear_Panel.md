# Rear Panel

## Motor Connectors

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Steppers.jpg" width="800">

## Limit Connectors

All CNC machine should have homing switches. They make sure your machine knows where it is in the machine space.

### Overview

A typical homing sequence works like this:

1. The machine will move towards a switch at a relatively rapid rate
2. Once it detects the switch it will back off the switch and rehome at a slower rate to get a more accurate home position
3. It then backs off the switch
4. The machine work position is then set
_Homing can be done one axis at a time or several axes can be done at one time. The default is one axis at a time_

### Switch Types
A variety of switch types can be used; generally they will be either normally open (N.O.) or normally closed (N.C.) _...or combination where they have both types of contacts_

1. #### Mechanical Switches
   
   I recommend a combination normally closed and normally open in a 3 wire configuration (illustrated below). In this fashion they are considered more fail safe and less prone to noise when in the closed state.
   - Use a switch with good mechanical mounting
   - It's also a good idea to have a mechanical stop that the axis can crash into if the switch fails to stop the motion

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Limit_Switch_Mech.jpg" width="800">

2. #### Proximity Switches
   
   With proximity switches, you generally need to provide these with power ground. Be sure they are compatible with 5V or 24V. The default setting is 5V, however most proximity switches require 10-24VDC; therefore you may need to move a jumper inside the x-ProV5 to provide 24V logic. 

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Limit_Switch_SS.jpg" width="800">

### Limit Switch Settings
You will find more on the use of these settings below.

Setting | Description
-------------|------------- 
```$Stepper/DirInvert``` | (This sets the direction of travel of the axes during homing)
```$Limits/Invert``` | (This inverts the switch logic for normally open and normally closed switches)
```$Limits/Soft``` | (Soft limits)
```$Limits/Hard``` | (Hard limits)
```$Homing/Enable``` | ($Homing/Enable=Off disables homing $Homing/Enable=On enables homing)
```$Homing/DirInvert``` | (Homing search direction invert bit mask)
```$Homing/Feed``` | (The feed rate during the slower locate phase)
```$Homing/Seek``` | (The faster search feed rate )
```$Homing/Debounce``` | (Switch debounce delay)
```$Homing/Pulloff``` | (The distance the the machine moves away from a switch after touching it)

### First Tests
1. Switch Activation:
   - The first thing to do it to check that the switches are reporting correctly
   - Manually move the machine axes so no switches are being activated by an axis 
   - Send ? to get the current status, It should report something like this:
     - ```<Alarm|WPos:0.000,-0.000,0.000|Bf:15,128|FS:0,0>```
   - Now manually activate one of the switches. It should look something like this:
     - ```<Alarm|WPos:0.000,-0.000,0.000|Bf:15,128|FS:0,0|Pn:X>```
   - You are looking for ```Pn:X``` (or whatever axis you activated)
   - You should **NOT** see ```Pn:?``` in your status without switches depressed
   - You **Should** see the applicable ```Pn:X```, ```Pn:Y```, ```Pn:Z```... status for each axis switch activated
     - ```Pn:``` will report the ```XYZABC``` axis letters and also ```PDRHS``` (Probe,Door,Reset,Hold,Start) 
     - You want to make sure no switches are reporting when the limit are not activated.

     If you see all switches reporting in the ```Pn:``` status when no switches are being activated something is inverted. The ```$5``` setting inverts the switch reporting status. ```$5``` can be either ```$5=0``` or ```$5=1```. Swap the ```$5``` value if you are not seeing the correct status. Recheck the status of no switch activated and all switches individually activate. **DO NOT move onto the the next step until you are getting the correct status**

2. Axis Movement Directions
   
   The next thing to do is check that the axes move in the right direction. If you send it a move in the positive direction, does the axis move in the positive direction. Here is a sequence I like to use:
       - First send ```$X``` to clear any alarms
       - Send ```G91``` to put the machine in incremental mode (the machine should respond with OK, and not move)
       - Each time you send something like ```G0X2``` it will move 2 units in the positive direction (if you send ```G0X-2``` it should move 2 units in the negative direction)
       - Use a short move for each axis to make sure they all move in the correct direction.

   If any axes move in the wrong direction, there are 2 ways to fix this:
       - The first is with the ```$Stepper/DirInvert``` setting
       - The setting is a list of the axis direction you want to invert (For Example: ```$Stepper/DirInvert=XZ``` means **X** and **Z** are currently inverted.
       - Check the current setting by sending ```$Stepper/DirInvert``` (add or remove letters to change the direction of the axes)

   The other way to to change the stepper motor wiring. You would swap the wires on only one coil of the motor going the wrong way. _Note: on machines with 2 motors on an axis, fixing the wiring is the only way to fix the direction_

3. Homing Direction
   
   The axes can home in any direction you prefer. For example: it is common on a lot of routers to home X and Y in the negative direction and Z in the positive (up) direction. This is controlled with the ```$Homing/DirInvert``` setting.

4. Homing Cycles
   
   The axes are homed in cycles:
   - Typically a Z would home up first to clear the work before you home in X and Y
   - You can have more than one axis home per cycle
     - You might want to home Z on the first cycle and then home X and Y at the same time on the second cycle
     - When setting up a machine it best to do one axis per cycle
     - Once everything is working, you might want to speed things up with multi-axis homing
   You can have up to 6 cycles, Example:
   - Set ```$Homing/Cycle0=Z``` to home the Z on the first cycle
   - Set ```$Homing/Cycle1=XY``` to home X and Y together on the second cycle
   - Set ```$Homing/Cycle2=``` to put nothing on the third cycle (Make sure all cycles have what you want)

5. Homing Test Setup

   Make sure the following settings are configured accurately:
   - ```$Limits/Soft=Off``` (Turn off soft limits)
   - ```$Limits/Hard=Off``` (Turn off hard limits)
   - ```$Homing/Enable``` (Enable homing)
   - ```$Homing/Feed=100``` (a slow homing feed rate)
   - ```$Homing/Seek=200``` (a faster seek rate)
   - ```$Homing/Pulloff=3``` (set homing switch pull off to 3mm)
   - ```$Homing/Cycle0=Z```, ```$Homing/Cycle0=X```,  or ```$Homing/Cycle0=Y```; only one axis per cycle

6. Homing Test
   
   Homing is done using the ```$H``` command. By default you can also use single axis homing with ```$HX```, etc. Single axis homing is good for testing, so I will assume it is enabled.

   Be ready to kill the power or hit the xPro-V5 Reset button. Send ```$HZ```; the axis should move towards the limit switch, touch it, back off and repeat at a slower speed.

   If it just does a short slow move and issues an ALARM 8 - It means you detected an activated switch before homing and tried unsuccessfully to clear the switch by backing off. 

   _Note: If you have switches at both ends of the axis (set in firmware), Grbl will not know which way to back off and immediately issue the ALARM 8_

7. Tuning
   Once you have basic homing working, you can tune some values to get better performance. Make sure the ```$27``` pull off is fully clearing the switch. Play with the ```$24``` and ```$25``` speeds. It is nice to have a relatively quick search phase followed by a slow second locate phase.

### Soft Limits
- Soft limits, ```$Limits/Soft=On```, use Grbl's internal position values to determine if a requested move will crash into one of the ends.
- It uses the max travel ```$13x``` to determine this. 
- The machine will enter an Alarm 2 mode. 
- You must reset the machine, but no position has been lost and you do not need to rehome.
- If you want soft limits to ignore an axis, set that ```MaxTravel``` on that axis to 0.

### Hard Limits
- Hard limits, ```$Limits/Hard=On```, use the switches to stop Grbl if it hits a limit switch.
- This is a good fail safe setup; it does an immediate uncontrolled stop. 
- You must rehome before you can use the machine again. 
- Also, CNC machines tend to create a lot of electrical noise when running and can cause false limit switch triggers. _test your machine before using this mode_