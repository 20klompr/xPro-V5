# Front Panel

 - [Power](Power) 
 - [Cool](Cool)
 - [VFD-RS485](VFD-RS485)
 - [Toolhead](Toolhead)
 - [Relay](Relay)
 - [Door](Door)
 - [RS485A/B switch](RS485A/B_switch)
 - [Program button](Program_button)
 - [USB](USB)





## Power

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front%20copy.jpg" width="400">

## Cool

The Coolant output can be used to control dust-extraction or cutting fluid systems, and/or repurposed for other switching requirements. 
_Note: the port labeled "GND" acts as a switched ground source, the signal labeled "SIG" is a constant 5V(default) or 24V*_

**The Cool(mist) signal is enabled using the _M7_ (mist) gcode statement and turned off with the _M9_ gcode statement**

Electrical Specifications:
- Voltage: jumper select - 24v(default) or 5v
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

## Spindle Types

In the xProV5 there are **$** settings for each spindle type. This allows a simple method of creating new spindles and a standard interface for Grbl to work with. 

You can change between spindles types dynamically. For example: You could have a spindle and a laser on the machine and switch between without recompiling or even rebooting.

Spindles are defined in your machine definition file (the default setting on the xProV5 is PWM). The spindle type is dynamically selected by entering **$Spindle/Type=XXXXX**' in the command line. Here are the spindle types currently available. _note: the I/O pins you need to define depends on the spindle type you choose_

```
#define SPINDLE_TYPE     SpindleType::NONE 
#define SPINDLE_TYPE     SpindleType::PWM
#define SPINDLE_TYPE     SpindleType::RELAY
#define SPINDLE_TYPE     SpindleType::LASER 
#define SPINDLE_TYPE     SpindleType::DAC   
#define SPINDLE_TYPE     SpindleType::HUANYANG // RS485
#define SPINDLE_TYPE     SpindleType::BESC
#define SPINDLE_TYPE     SpindleType::_10V
#define SPINDLE_TYPE     SpindleType::H2A // RS485
```

### VFD – RS485 port

The VFD-RS485 terminal provides the ability to drive HY Series inverters via RS485 serial protocol.  To do this, you must first update the firmware to one of the “CNC_xPRO_V5_----_485_--” variants.  485 denotes serially controlled HY VFD.  Next flip the EN/PWM switch over to RS485 A/B. Lastly, wire terminal A on the xPRO V5 to RS+ of the VFD and terminal B to the RS- of the VFD.  
 
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/485VFD.jpg" width="800">


o	Toolhead
Toolhead Port
The toolhead port provides the means to drive many different toolheads (spindles, VFDs, laser, and plasmas, etc.)  This 4-pin connector provides a ground reference, a 0-5V pwm signal, a 3.3V Spindle Enable and a 0-10V analog signal.  
	PWM 
	The pwm signal is used primarily to drive lasers and small spindles.  The ppwm signal is activated by an M3 or M4 gcode statement.  The value of the pwm signal is determined by the speed portion of the M3/M4 command – ex. M3 S6000 will create a half max pwm signal output (assumes the default max spindle speed of 12,000rpm).  
	Spindle Enable:
	The spindle enable signal is used by some laser modules and spindles to act like an “on” switch.  When an M3 or M4 command is issued, the Spindle Enable signal goes high to 3.3V and stays constant regardless of the speed command.  
	0-10V
	The 0-10V signal is used primarily to drive VFDs and a select few laser modules.  The 0-10V output act identical to the pwm output, except it is processed to create an analog 0-10V ouput that scales with the speed command.  
o	Relay
   	Relay Terminal
 The onboard relay provides a high-power switch to activate device the normally could not be controlled by a digital logic pin – example Plasma Trigger, 24V coolant pump, 24V water cooled spindle pump, 24V VFD logic signal etc.  
The relay acts as a simple switch connecting two wires and creating a path for current to flow.  
The relay is typically used as a low side switch, simply wire the GND terminal of the 2-pin 3.81 EDG connector the appropriate ground, and wire the NO terminal of the 2-pin 3.81 EDG connector the your “device” ground. 
The relay is controlled via the Spindle Enable signal, or the Mist signal – action is selected using the internal jumper next to the Relay (default is Spindle Enable) 
(Sample circuit here)
  
*Note the Relay pin GND is isolated from the Controller GND.  

o	Door
Door/Estop Terminal


A switch can be connected to the Door/Estop terminal to allow a physical button to stop the machine.  The Door/Estop button is a safety switch and will completely abort the current motion (or gcode cut) and require a reset the controller.  You can use a Normally Open (NO) or Normally Closed (NC) switch for the Door/Estop switch.  
To use a NO switch, simply wire the switch to the 2 pin, 3.81mm header.  
(picture would be useful)
To use a NC switch, you must first update the firmware to one of the “CNC_xPRO_V5_----_---_NC” variants.  NC denotes a Normally Closed Door/Estop switch.  Then wire the NC switch to the 2 pin, 3.81mm header. 
(picture would be useful)

  

o	RS485A/B switch
EN/PWM - RS485A/B Switch
This switch controls the tool control output type, switching between the toolhead signals (pwm, en, 0-10V) and the VFD-RS485 signals of A and B.  Note: Changing between the different control options requires the appropriate firmware to be installed, flipping the switch simple changes the data lines not the control type.  
o	Program button
Program Button
This button is used to initialize the bootloader when uploading firmware via USB.  Otherwise, don’t push it 😊.  

o	USB
USB Connection
The USB connection for the aPRO is a USB-C type connector.  This connector allows the use of USB based GRBL Senders (CNCjs, Universal Gcode Sender, etc).  These sender provide more features than available through the WebUI – mostly in terms of graphics and job tracking.  
