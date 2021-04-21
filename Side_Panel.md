# Side Panel
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/QuickStart_FRONT.jpg" width="800">

## Power Switch
The power switch controls turns off the main **+24V** which powers the Stepper Drivers as well as the 5V and 3.3V when a USB cable is not plugged in.

With the Power Switch **OFF**, +24V to the stepper drivers is removed; however, with USB plugged in, 5V will power the 32 bit Arm processor and associated logic functions. _NOTE: if one of the [Motor fault indicators]() you recieve a fault indication on the side of th

## Reset Switch

## SD Card

## Motor fault indicators

## Status indicators

## Probe input

## Macro1 & Macro2

## TMC diag_0


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
