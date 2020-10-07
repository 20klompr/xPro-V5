#![](https://github.com/Spark-Concepts/xPro-V5/blob/main/images/xproV5_iso.jpg)

<img src="https://www.trinamic.com/fileadmin/_processed_/7/0/csm_TMC5160A-TA_baa5ea6044.jpg" width="400">

The CNC xPRO V4 Controller is an all-in-one multi axis CNC controller. It is a specifically designed for CNC applications and pre-loaded with the latest stable version of GRBL. The CNC xPRO is equipped with 4 DRV8825 motor drivers; allowing for dual motor configurations on the X,Y, or Z axis. The V4 board also features isolated digital and analog power buses to greatly reduced noise and false triggers.    

[Trinamic 5160](https://www.trinamic.com/products/integrated-circuits/details/tmc5160)

Introduced with the V4 is our new DriverMinder logic - the V4 constantly monitors the status of each motor driver. Should a fault occur, your selected action will be taken, options are:

* Pause - will initiate a system pause allowing you to safely troubleshoot and potentially save your work-piece
* Stop - emergency abort, system stop immediately and reboots 
* Play - continue on as if nothing happened (i.e. ignore fault).  

# What's New for the V4
* Less than 5-minute install with the use of plug and play motors headers, limit switches and power supplies  
* 4-layer design featuring completely isolated motor and logic power buses
* All smd parts moved to back of board to protect again debris and short
* Integrated #4-40 standoff for easy mounting and short protection
* Three ways to power - Laptop PSU (2.5x5.5mm plug), a 2 wire power supply, or ATX computer power supply
* Selectable spindle output voltage range: 0-5V or 0-10V for full compatibility with spindles, VFDs, Lasers and more 
* Coolant output is now an open drain (directly sink up to 3A or use to control external relays)
* Screw terminal hookup for external Step/Dir drivers makes beefing up easy
* Extremely robust power management for overvoltage/reverse voltage protection including replaceable mini blade fuses 
* Motor driver step and fault LEDs for easier troubleshooting
* Enhanced silkscreen with more pictures and less labels
* Compatible with Openbuilds XTension wiring system for even easier setup! 
* DIP SWITCHES = NO MORE SOLDER JUMPERS - want to change decay mode → slide the switch, want to clone Z motion instead of Y → slide the switch 

![](https://github.com/Spark-Concepts/xPro-V4/blob/master/wiki/xProV4_Bottom_Views.jpg)

# Specs:
* Pre-loaded with latest stable version of [GRBL](https://github.com/gnea/grbl/releases)
* Compatible with [ESTLcam](http://estlcam.com/)
* Capable of powering from [ATX PSU](http://en.wikipedia.org/wiki/ATX#Power_supply), dedicated [12/24V two wire power supply](https://openbuildspartstore.com/24v-meanwell-power-supply-bundle/), or a [laptop power supply](http://a.co/d/3vbO8BL)
* Drive 4 motors with [TI DRV8825](http://www.ti.com/product/drv8825) Stepper Drivers - 2.5A (peak) with 1.75A (RMS) with up to 1/32 microstepping
* 1 Driver capable of cloning X,Y, or Z
* Hardware support for both USB and Wireless Operation (XBee, WiFly, or Bluelink)*
* Emergency Stop to cut all motor power (with optional override)
* 12V and 5V** outputs for powering peripherals (fans, pumps, vacuums)
* Quickly connect to Stepper Motors and limit switches with [3.5mm screw terminals](https://media.digikey.com/Photos/Wurth%20Electronics%20Photos/691361100004.JPG)
* Expansion port for coming upgrades (Illuminated button pendant, jog control, and headless sender)

# Use the CNC xPro to drive:
* 3 Axis CNC Mill With Dual Drive Motors
* Laser Cutter With XY, and Z motion
* Plasma Cutter 
* Pick and Place for SMD components 
* Or wireless robotics (would love to see this used on a wireless quadruped) 

# Software:

The CNC xPRO is designed to run the opensource gcode interpreter [GRBL](https://github.com/gnea/grbl/), [ESTLcam](http://estlcam.com/) control software, or can be programmed with [Arduino](http://www.arduino.cc/) or [Atmel studio](https://www.microchip.com/mplab/avr-support/atmel-studio-7).  The xPRO comes pre-flashed with GRBL so all you need to do is wire up your motors! 

From the computer side we have found that [CNCjs Desktop](https://cnc.js.org/docs/desktop-app/)  is amazing and packed with great features! (open source / free download). 

For designing parts here at Spark Concepts we use the following stacks:

1. For mechanical parts (think gantry plates, mounts, boxes, etc):
* [Fusion 360](https://www.autodesk.com/products/fusion-360/overview) (both CAD design and CAM, includes GRBL postprocesser by default) (free for hobbyists)
* [CNCjs](https://cnc.js.org/docs/desktop-app/) (send and monitor the cut file to the xPRO) (free/opensource)

2. For artwork (think signs, drawings, plaques, awards, etc):
* Vetric [Vcarve](http://www.spark-concepts.com/vetric-vcarve-desktop-cam-software/) or [Aspire](http://www.spark-concepts.com/vetric-aspire-cam-software/)  depending on design (drawing and CAM, includes GRBL postprocessor) (paid)
* [CNCjs](https://cnc.js.org/docs/desktop-app/) (send and monitor the cut files to the xPRO) (free/opensource) 
<img src="https://cloud.githubusercontent.com/assets/447801/24392019/aa2d725e-13c4-11e7-9538-fd5f746a2130.png" width="400">

* [Website](https://cnc.js.org/) 

## Universal Gcode Sender

<img src="http://winder.github.io/ugs_website/img/platform/screenshot.png" width="400">

