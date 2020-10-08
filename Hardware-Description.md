![](https://github.com/Spark-Concepts/xPro-V5/blob/main/images/xproV5_iso.jpg)

The CNC xPRO V5 is an easy to use high power motion controller for CNC machines.  The CNC xPRO is capable of supporting a wide variety of CNC machines with up to 4 motor working independently or ganged (multiple motors working together to move one axis like the Workbee machines, also called dual drive, cloned, etc.). The CNC xPRO-V5 runs on a 32Bit processor and has integrated Wifi and Web Interface capabilities. 

The xPRO V5 uses [Trinamic stepper drivers](https://www.trinamic.com/products/integrated-circuits/details/tmc5160) to drive 4 high Power MOSFETs per axis and capable of driving motors up to 6 amps.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/trinamic2.png" width="600">

# Specs:

* 24v - 15 Amp Input Power Fuse
* Built-in Heatsink & Temperature Controlled Cooling Fan

## Motors

* Control up to 4 coordinated axes (XYZA)
* Each axis can have 1 or 2 motors each for a total of 8 motors
* Dual motors axes can optionally auto square using a home switch and independent control for each motor.
* Motor drivers can be dynamically assigned to axes, so a 4 motor XYZA controller could be converted to a XYYZ (dual motor Y axis) without any hardware changes.
* Step rates up to 120,000 per/second.
* Up to 1/256 Micro stepping (Defaulted to 1/8 for higher Torque).
* Trinamic (SPI controlled) stepper motors are supported including StealthChop, CoolStep and StallGuard modes. 
* Sensorless homing can be used.

## Peripherals

* Limit/Homing Switches with debouncing
* Up to 24v 5A switched output for coolant devices such as 24v DC pumps, solenoids, contactors, or Solid State Relays
* Configurable Relay for generic use cases (Plasma trigger, Coolant Pumps, Spindle control, etc)
* Configurable Macro input
* Z Probe (XYZ)
* Safety Door (open door safely retracts and stops spindle, can be resumed) - alternatively configured as an E-Stop to disable stepper motors when a normally closed switch is depressed
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/door-sensor-wiring.jpg" width="600">

## Spindles

* PWM
* RS485 Modus
* DAC (analog voltage) 0-10V
* Relay Based
* RC type Brushless DC motors using low cost BESCs
* Laser PWM with power/speed compensation
* Easy to create custom spindles

## Connectivity

* USB/Serial
* Bluetooth/Serial Creates a virtual serial port on your phone or PC. Standard serial port applications can use Bluetooth.
WIFI
* Creates its own access point or connects to yours.
* Built in web server. The server has full featured CNC control app that will run on your phone or PC in a browser. No app required.
Telnet sending of gcode
* Push notifications (like...job done, get a text/email)
* OTA (over the air) firmware upgrades.
* SD card (Gcode can be saved, loaded, and run via WIFI)

## Compatibility

The xPro-V5 is fully backward compatible with GRBL and can use all gcode senders.

## Customizable

- Easy to map pins to any functions.
- Custom machines can be designed without touching the main code.
- Custom initialization
- Kinematics
- Custom homing
- Tool changer sequences
- Button macros (run gcode sequence, etc.)
- Custom end of Job sequence
- RTOS Real time operating system allows background monitoring and control without affecting motion control performance
- Fast boot

