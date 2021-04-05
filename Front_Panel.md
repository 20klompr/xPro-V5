# Front Panel

<!--- - [Power](Power) 
 - [Cool](Cool)
 - [VFD-RS485](VFD-RS485)
 - [Toolhead](Toolhead)
 - [Relay](Relay)
 - [Door](Door)
 - [RS485A/B switch](RS485A/B_switch)
 - [Program button](Program_button)
 - [USB](USB) --->

## Power

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front%20copy.jpg" width="400">

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

Spindles are defined in your machine definition file (the default setting on the xProV5 is **PWM**). The spindle type is dynamically selected by entering ```$Spindle/Type=XXXXX```' in the command line. Here are the spindle types currently available. _note: the I/O pins you need to define depends on the spindle type you choose_

```
$Spindle/Type=NONE
$Spindle/Type=PWM
$Spindle/Type=LASER 
$Spindle/Type=HUANYANG // RS485
$Spindle/Type=H2A // RS485
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

### $Spindle/Type=LASER

A laser is basically a PWM spindle with a few extra features. You want it to turn off when the machine is doing a rapid move or is paused. It can also do a speed compensation feature. If you are engraving you want the laser to proportionally reduce the power when it is accelerating or decelerating. Use the M4 command, normally used for counter clockwise rotation, to enable this feature.

<!--- Optional #define SPINDLE_TYPE SpindleType::LASER
Required #define LASER_OUTPUT_PIN GPIO_NUM_nn where nn is the pin number.
Optional #define LASER_ENABLE_PIN GPIO_NUM_nn If you want an enable signal. --->
- Most PWM Settings also apply
- **$GCode/LaserMode** Set ```$GCode/LaserMode=1``` for laser mode
- **$Laser/FullPower=nnnn**; sets the full power number used for the PWM. ```$Laser/FullPower=100``` means your power range is 0-100.

### $Spindle/Type=XX // RS485

This spindle mode talks to a **Huanyang VFD** _(a very popular Chinese VFD)_ using an RS485 serial connection. It can control the speed and which direction the spindle should turn. To change this setting flip the EN/PWM switch<sub>(1)</sub> over to **RS485 A/B** and enter the command ```$Spindle/Type=HUANYANG // RS485``` or ```$Spindle/Type=H2A // RS485```; Note: type **HUANYANG** is their original protocol and **HY2** is Huanyang's latest protocol. Wire terminal A on the xPRO V5 to RS+ of the VFD and terminal B to the RS- of the VFD. 

- With the _EN/PWM-RS485 A/B_ switch<sub>(1)</sub> set to **_RS485 A/B_** the _TOOLHEAD_ port GPIO's are de-activated and the bi-directional differential pair RS-485 circuit enabled:
1. RS485 (RS+)
2. RS485 (RS-) 

_RS485 circuit automatically asserts a send signal as it transmits - e.g. a direction pin is not required_
  
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/485VFD.jpg" width="800">

_note: If you're having issues, try updating the firmware to the **HY** “CNC_xPRO_V5_----_485_--” variant_


## Relay Terminal
The onboard relay provides a high-power switch to activate device the normally could not be controlled by a digital logic pin – example Plasma Trigger, 24V coolant pump, 24V water cooled spindle pump, 24V VFD logic signal etc. The relay acts as a simple switch connecting two wires and creating a path for current to flow. The relay is controlled via the **Spindle Enable signal** or the **Mist signal**. The selection is made using the internal jumper<sub>(1)</sub> next to the Relay (as seen below). _note: the default relay control is set to Spindle Enable<sub>(1)</sub>_

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Relay.jpg" width="800">

## Door/Estop

A switch can be connected to the Door/Estop terminal to allow a physical button to stop the machine.  The Door/Estop button is a safety switch and will completely abort the current motion (or gcode cut) and require a reset the controller.  You can use a Normally Open (NO) or Normally Closed (NC) switch for the Door/Estop switch.  
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/GPIO_plugs.jpg" width="800">

- To use a NO switch, simply wire the switch to the 2 pin, 3.81mm header. Verify the default jumper settings<sub>(1 & 2)</sub> 
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/NO_Estop.jpg" width="800">

- To use a NC switch, you must first update the firmware to one of the “CNC_xPRO_V5_----_---_NC” variants. Verify the jumper settings<sub>(1 & 2)</sub> Then wire the NC switch to the 2 pin, 3.81mm header. 
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/NC_Estop.jpg" width="800">
 
***
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front%20copy.jpg" width="400">

## Program Button
This button is used to initialize the bootloader when uploading firmware via USB.  Otherwise, don’t push it 😊.  

## USB Connection
The USB connection for the xProV5 is a USB-C type connector.  This connector allows the use of USB based GRBL Senders (CNCjs, Universal Gcode Sender, etc). These sender provide more features than available through the WebUI – mostly in terms of graphics and job tracking.  