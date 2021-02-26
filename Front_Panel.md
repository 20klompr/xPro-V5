# Front Panel

## Power

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front%20copy.jpg" width="400">

## Cool

The Coolant output can be used to control dust-extraction or cutting fluid systems, and/or repurposed for other switching requirements. 
_Note: the port labeled "GND" is actually a switched ground, the signal labeled "SIG" is a constant 24V*_

The Cool(mist) signal is enabled using the M7 (mist) gcode statement and turned off with the M9 gcode statement

Electrical Specifications:
- Voltage: jumper select - 24v(default) or 5v
- Max Current: 3A (24v only)
- Suitable for inductive loads (24v only)
<sub>*the SIG can also be configured to output 5V by removing the xProV5 lid and moving the "MIST" jumper from 24V to 5V</sub>

## Connect LED Ring

You can use the Coolant output to switch any other 24v device as well, as an example, you can connect a Spindle LED Ring as shown to put it under M-Code control (for example if you want a Job to turn the LED ring on at the start and off at the end, you can add an M8 to the header and an M9 to the footer of your g-code)

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-led-ring.jpg" width="600">

## Connect Dust Extraction via IoT Relay

You can use our IoT Relay Power Strip to control a Vacuum for dust extraction

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-iot-vacuum.jpg" width="800">

## Connect 24vDC Air Solonoid

You can switch 24v Solenoids using the Coolant output. Typically you'd use this configuration in-line between an Air-compressor and a Nozzle pointed at the endmill. Compressed air blowing on the endmill will help evacuate chips from the cut, and keep the endmill cool

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-solenoid.jpg" width="600">



The Mist signal is enabled using the M7 (mist) gcode statement and turned off with the M9 gcode statement. 
o	VFD-RS485
VFD – RS485 port
The VFD-RS485 terminal provides the ability to drive HY Series inverters via RS485 serial protocol.  To do this, you must first update the firmware to one of the “CNC_xPRO_V5_----_485_--” variants.  485 denotes serially controlled HY VFD.  Next flip the EN/PWM switch over to RS485 A/B. Lastly, wire terminal A on the xPRO V5 to RS+ of the VFD and terminal B to the RS- of the VFD.  
 
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
