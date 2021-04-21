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

$Stepper/DirInvert (This sets the direction of travel of the axes during homing)
$Limits/Invert (This inverts the switch logic for normally open and normally closed switches)
$Limits/Soft (soft limits)
$Limits/Hard (hard limits)
$Homing/Enable ($Homing/Enable=Off disables homing $Homing/Enable=On enables homing)
$Homing/DirInvert (Homing search direction invert bit mask)
$Homing/Feed (The feed rate during the slower locate phase)
$Homing/Seek (The faster search feed rate )
$Homing/Debounce (Switch debounce delay) This sets a little delay between homing phases to let the switch settle.
$Homing/Pulloff (The distance the the machine moves away from a switch after touching it)

