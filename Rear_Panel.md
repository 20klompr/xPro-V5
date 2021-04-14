# Rear Panel

## Motor Connectors

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Steppers.jpg" width="800">

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

<!--- ## Connect LED Ring

You can use the Coolant output to switch any other 24v device as well, as an example, you can connect a Spindle LED Ring as shown to put it under M-Code control (for example if you want a Job to turn the LED ring on at the start and off at the end, you can add an M8 to the header and an M9 to the footer of your g-code)

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-led-ring.jpg" width="600">

## Connect Dust Extraction via IoT Relay

You can use our IoT Relay Power Strip to control a Vacuum for dust extraction

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-iot-vacuum.jpg" width="800">

## Connect 24vDC Air Solonoid

You can switch 24v Solenoids using the Coolant output. Typically you'd use this configuration in-line between an Air-compressor and a Nozzle pointed at the endmill. Compressed air blowing on the endmill will help evacuate chips from the cut, and keep the endmill cool

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-solenoid.jpg" width="600"> --->

## Toolhead/VFD-RS485

In the xProV5 there are **$Spindle** settings for each spindle type. This allows a simple method of creating new spindles and a standard interface for Grbl to work with. 

You can change between spindles types dynamically. For example: You could have a spindle and a laser on the machine and switch between without recompiling or even rebooting.

***
### RS485A/B switch

**EN/PWM - RS485A/B** Switch controls the tool control output type, switching between the toolhead signals (pwm, en, 0-10V) and the VFD-RS485 signals of A and B.  Note: Changing between the different control options requires the appropriate firmware to be installed, flipping the switch simply changes the data lines not the control type.  
***

### Spindle Types

Spindle classes are defined in the firmware (the default firmware on the xProV5 is **PWM**). The Spindle Type is dynamically selected by entering ```$Spindle/Type=XXXXX```' in the command line. There are two classes of Spindles with two separate Spindle Types.

**For the PWM and Laser Spindle classes you will need the [“CNC_xPRO_V5_----_PWM_--”](https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware)  firmware variant**
```
$Spindle/Type=NONE
$Spindle/Type=PWM
$Spindle/Type=LASER 
```
**For the RS485 Laser Spindle classes you will need the [“CNC_xPRO_V5_----_485_--”](https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware) firmware variant**
```
$Spindle/Type=NONE
$Spindle/Type=HUANYANG 
$Spindle/Type=H2A 
```
### $Spindle/Type=NONE 
If your machine does not require a spindle, like a pen plotter, choose this type. It will not use any I/O. It will default to this type if no I/O is defined.

### $Spindle/Type=PWM
This is the default setting on the xProV5. Many speed control circuits for spindles use a PWM signal to set the speed. 
- With the _EN/PWM-RS485 A/B_ switch<sub>(1)</sub> set to **_EN/PWM_** the _TOOLHEAD_ port provides the means to drive many different toolheads (spindles, VFDs, laser ```$Spindle/Type=LASER```, and plasmas, etc.)  This 4-pin connector provides the following:

1. **Ground Reference**
2. **0-5V PWM Signal**
   - The pwm signal is used primarily to drive lasers and small spindles.  The ppwm signal is activated by an M3 or M4 gcode statement.  The value of the pwm signal is determined by the speed portion of the M3/M4 command – ex. M3 S6000 will create a half max pwm signal output (assumes the default max spindle speed of 12,000rpm)
3. **3.3V Spindle Enable**
   - The spindle enable signal is used by some laser modules and spindles to act like an “on” switch.  When an M3 or M4 command is issued, the Spindle Enable signal goes high to 3.3V and stays constant regardless of the speed command
4. **0-10V Analog Signal**
   - The 0-10V signal is used primarily to drive VFDs and a select few laser modules.  The 0-10V output act identical to the pwm output, except it is processed to create an analog 0-10V ouput that scales with the speed command. _note: the maximum output of 10V can be calibrated by commanding ```M3 S12000``` and adjusting the potentiometer<sub>(2)</sub> as indicated below_
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Analog_VFD.jpg" width="800">

- ```$GCode/MaxS=XXXX``` sets Maximum spindle speed _(XXXX=0-1000)_. This value is used to match a PWM duty to the RPM you want. If you set this to 1000 and send S500, it will set the duty to 50%. If you send S1500, it will change clip your value to 1000.
- ```$GCode/MinS=XXX``` sets the the minimum RPM. If your spindle does not work well below 200 RPM, set $31=200. If you send an S value below 200, it will set the speed to 200.
<!--- - ```$Spindle/PWM/Frequency``` Spindle PWM Freq. This sets the frequency of the PWM signal. --->
- ```$Spindle/PWM/Off=XXX``` sets spindle PWM Off Value. Typically set to 0. The PWM values are set in percentage of duty cycle.
- ```$Spindle/PWM/Min=XXX``` sets spindle PWM Min Value Typically set to 0.
- ```$Spindle/PWM/Max=XXX``` sets spindle PWM Max Value. Typically set to 100.

#### Dewalt Spindle
You can also install a Solid State Relay (SSR) to control your spindle using Gcode commands (ensure the _EN/PWM-RS485 A/B_ switch<sub>(1)</sub> is set to **_EN/PWM_**)
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/xPRO_SSR.jpg" width="800">

### $Spindle/Type=LASER

A laser is basically a PWM spindle with a few extra features. You want it to turn off when the machine is doing a rapid move or is paused. It can also do a speed compensation feature. If you are engraving you want the laser to proportionally reduce the power when it is accelerating or decelerating. Use the M4 command, normally used for counter clockwise rotation, to enable this feature.
