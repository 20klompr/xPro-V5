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

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front-plugs.jpg" width="400">

## Cool

The Coolant output can be used to control dust-extraction or cutting fluid systems, and/or repurposed for other switching requirements. 
_Note: the port labeled "GND" acts as a switched ground source, the signal labeled "SIG" is a constant 5V(default) or 24V*_

**The Cool(mist) signal is enabled using the _M7_ (mist) gcode statement and turned off with the _M9_ gcode statement**

Electrical Specifications:
- Voltage: jumper select<sub>(1)</sub> - 24v or 5v(default)
- Max Current: 250mA(5V) and 2A(24V)
- Suitable for inductive loads (24V only)

*the SIG port can also be configured to output 24V by removing the xProV5 lid and moving the "MIST" jumper<sub>(1)</sub> from 5V to 24V</sub>
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
   - The pwm signal is used primarily to drive lasers and small spindles.  The pwm signal is activated by an ```M3``` or ```M4``` gcode statement.  The value of the pwm signal is determined by the speed portion of the M3/M4 command – ex. ```M3 S6000``` will create a half max pwm signal output (assumes the default max spindle speed of 12,000rpm) 
   - When **laser** mode is enabled, Grbl controls laser power by varying the 0-5V voltage from the spindle PWM connector. 0V should be treated as disabled, while 5V is full power. Intermediate output voltages are also assumed to be linear with laser power, such that 2.5V is approximate 50% laser power.
   - By default, the spindle PWM frequency is 5kHz, and compatible with most current Grbl-compatible lasers system. If a different frequency is required, such as the _"[Bulkman 15W Laser](https://bulkman3d.com/product/15w-blue-light-laser-module/) a modulation frequency <9 kHz"_ is recommended; this can be changed by typing ```$Spindle/PWM/Frequency 9000``` in the console window (**$Spindle/PWM/Frequency** _"Spindle PWM Freq"_) 

     - For [LightBurn](https://forum.lightburnsoftware.com/t/laser-not-burning/13944/7) users please note the xPro-V5 ```$30``` setting. The ```S-value max``` setting in LightBurn should match the ```$30``` setting on the xPro [(typically ```$30=1000```)](https://github.com/Spark-Concepts/xPro-V5/wiki/Changing-settings#grbl-settings)

**NOTE:** _go to **Edit** > **Device Settings** in [LightBurn](https://forum.lightburnsoftware.com/t/laser-not-burning/13944/7) and modify the S-Value as required_
![image](https://user-images.githubusercontent.com/8650709/115465025-f25d5c00-a1fb-11eb-891a-e8612288f1af.png) 

<!--- On some xPro's a resistor was inadvertently populated which will cause the 0-5V PWM output to drive to 5V when the Spindle Enable goes low - to remedy this there are two options (remove resistor R23 on the bottom board) or --->
_For_ **SAFETY** _and unintentional actuation of the spindle when a PWM signal isn't being commanded, it is suggested that you wire the PWM through the relay and move the relay jumper<sub>(1)</sub> to SP_EN as illustrated below:_
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/SpindlePWM_1.jpg" width="800">

3. **3.3V Spindle Enable**
   - The spindle enable signal is used by some laser modules and spindles to act like an “on” switch.  When an ```M3``` or ```M4``` command is issued, the Spindle Enable signal goes high to 3.3V and stays constant regardless of the speed command **if your laser module does not have an enable, you will need to route the PWM output through the relay**
   - ### WARNING: SEE [$Spindle/Type=LASER](https://github.com/Spark-Concepts/xPro-V5/wiki/Front_Panel#spindletypelaser) FOR LASER WIRING INSTRUCTIONS

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

<!--- Optional #define SPINDLE_TYPE SpindleType::LASER
Required #define LASER_OUTPUT_PIN GPIO_NUM_nn where nn is the pin number.
Optional #define LASER_ENABLE_PIN GPIO_NUM_nn If you want an enable signal. --->
- Most PWM Settings also apply
- **$GCode/LaserMode** Set ```$GCode/LaserMode=1``` for laser mode
- **$Laser/FullPower=nnnn**; sets the full power number used for the PWM. ```$Laser/FullPower=100``` means your power range is 0-100.

<!---Laser mode activates several features. Use $32 to turn on and off laser mode.

The laser will only fire during an active G1, G2 or G3 command. This means rapid moves between cuts will turn off the laser
You can change speed on the fly without halting the move. This allows things like high speed laser engraving

On/Off Control
If you want an On/Off signal rather than variable speed (PWM) output, you just change $30 (Maximum spindle speed) to 1. This means any requested RPM above 0 will output a 100% duty signal _(default $30 setting is ```$30=1000```)_

$30=1
$31=0
--->
It is critical that the spindle enable signal is used if your laser module has on; **if your laser module does not have an enable, you will need to route the PWM output through the relay**

#### _Wiring lasers **with ENABLE SIGNALS**_
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/LasPWM_1.jpg" width="800">

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/LasPWM_2.jpg" width="800">

***

#### _Wiring lasers **without ENABLE SIGNALS**_
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/LasPWM_3.jpg" width="800">

### $Spindle/Type=XX // RS485

This spindle mode talks to a **Huanyang VFD** _(a very popular Chinese VFD)_ using an RS485 serial connection. It can control the speed and which direction the spindle should turn. To change this setting flip the EN/PWM switch<sub>(1)</sub> over to **RS485 A/B** and enter the command ```$Spindle/Type=HUANYANG``` or ```$Spindle/Type=H2A```; Note: type **HUANYANG** is their original protocol and **HY2** is Huanyang's latest protocol. Wire terminal A on the xPRO V5 to RS+ of the VFD and terminal B to the RS- of the VFD. 

- With the _EN/PWM-RS485 A/B_ switch<sub>(1)</sub> set to **_RS485 A/B_** the _TOOLHEAD_ port GPIO's are de-activated and the bi-directional differential pair RS-485 circuit enabled:
1. RS485 (RS+)
2. RS485 (RS-) 

_RS485 circuit automatically asserts a send signal as it transmits - e.g. a direction pin is not required_

The VFD's AC and Spindle cables induce a lot of noise/EMI on the RS485 - special considerations with wiring must be followed:
  
- Only use a twisted-pair (minimum of 2 or 3 twists per inch) or twisted-shielded-pair for the RS-485 communications line (20-24AWG); this will help to eliminate common mode noise that can corrupt the data on the line. _(if using a twisted-shielded-pair be sure to terminate the shield to ground at only one end of the cable)_

- DIY twisted-pair:
  1. Carefully insert the two cut ends of 20-24AWG wire into a drill chuck
  2. Pull the wire taut so that it will twist evenly when the drill is started 
  3. Using the drill twist the wire until you achieve a minimum of 2 or 3 twists per inch

- Recommended: place 100-120 ohm network termination resistor at the ends of an RS-485 twisted-pair communications line; this will help to eliminate data pulse signal reflections that can corrupt the data on the line<sub>(2)</sub>. _(any 100-120 ohm standard 1/2-1/4 watt carbon film resistor will suffice)_

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/485VFD.jpg" width="800">

_note: make sure you have the [“CNC_xPRO_V5_----_485_--”](https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware) firmware variant installed_

### Configure the Huanyang VFD
 Using the procedure in the [Huanyang-VFD-manual](https://github.com/Spark-Concepts/xPro-V5/files/6247012/Huanyang-VFD-manual.pdf)
 , set the following register values:
| Register  | Value | Description |
| :-------: | :-----: | ------------- |
 | PD000 | 0 | **unlock parameters** |
 | PD001 | 2 | **Command source is RS485** |
 | PD002 | 2 | **Speed source is RS485** |
 | PD163 | 1 | **Communications address 1** |
 | PD164 | 1 | **9600 b/s** |
 | PD165 | 3 | **8 Bit No Parity - RTU** |
 | PD000 | 1 | **lock parameters** |
 
 _Also verify the following settings (based on the values on the motor's nameplate)_
 
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

## Relay Terminal
The onboard relay provides a high-power switch to activate device the normally could not be controlled by a digital logic pin – example Plasma Trigger, 24V coolant pump, 24V water cooled spindle pump, 24V VFD logic signal etc. The relay acts as a simple switch connecting two wires and creating a path for current to flow. The relay is controlled via the **Spindle Enable signal** or the **Mist signal**. The selection is made using the internal jumper<sub>(1)</sub> next to the Relay (as seen below). _note: the default relay control is set to Spindle Enable<sub>(1)</sub>_

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Relay.jpg" width="800">

## Door/Estop

A switch can be connected to the Door/Estop terminal to allow a physical button to stop the machine.  The Door/Estop button is a safety switch and will completely abort the current motion (or gcode cut) and require a reset the controller.  You can use a Normally Open (NO) or Normally Closed (NC) switch for the Door/Estop switch.  
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/GPIO_plugs.jpg" width="800">

***
1. To use a NO switch, simply wire the switch to the 2 pin, 3.81mm header. Verify the default jumper settings<sub>(1 & 2)</sub> (see figure below).⤵
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/NO_Estop.jpg" width="800">

***
2. To use a NC switch, you must first update the firmware to one of the “CNC_xPRO_V5_----_---_NC” variants. Verify the jumper settings<sub>(1 & 2)</sub> Then wire the NC switch to the 2 pin, 3.81mm header (see figure below).⤵ 
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/NC_Estop.jpg" width="800">
 
***
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front-plugs_Prog.jpg" width="400">

## Program Button
This button is used to initialize the bootloader when uploading firmware via USB.  Otherwise, don’t push it 😊.  

## USB Connection
The USB connection for the xProV5 is a USB-C type connector.  This connector allows the use of USB based [GRBL Senders](https://github.com/Spark-Concepts/xPro-V5/wiki/USB_guide) (CNCjs, Universal Gcode Sender, etc). These sender provide more features than available through the WebUI – mostly in terms of graphics and job tracking.  