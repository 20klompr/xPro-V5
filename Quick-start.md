
# Safety Statement
The author of this document is not liable or responsible for any accidents, injuries, equipment damage, property damage, loss of money or loss of time resulting from improper use of electrical or mechanical or software products.

Assembling electrical and mechanical CNC machine components like power supplies, motors, drivers or other electrical and mechanical components involves dealing with high voltage AC (alternating current) or DC (direct current) and other hazardous items which can be extremely dangerous and needs high attention to detail, experience, knowledge of software, electricity, electro-mechanics and mechanics.
```diff
- BEFORE MAKING ANY CONNECTIONS OR DISCONNECTIONS POWER MUST BE REMOVED FROM THE DEVICE AND THE CONTROLLER. FAILURE TO DO SO WILL VOID ANY AND ALL WARRANTIES.
```
Before starting please read through all the instructions.

# Pre-Install Notes:

- Wire color codes or cable color code charts for both AC and DC are classified based on national standards for each region. You may find different colors in different countries. However, we like to use as much as we can specify colors for specific purposes, such as Red for 24V DC and Black for GND. Having this good practice will help you to troubleshoot possible issues that might occur in the future.
- Always use shielded cable for motors and drivers, this will minimize significantly electromagnetic interference, in consequence, this will permit you to run a job without any problem.
- NEVER connect or disconnect any wire while power is applied. Failure to do the above will result in damage to the stepper drivers.
ALWAYS disconnect power before making any connections or disconnections.
- If using our hardware, please follow the wiring guides below:

# 24V DC Input Wiring:

If you received the xPro V5 Controller bundle with a Meanwell 24V power supply, wire your system as shown below: Connect V- from the power supply to GND on the xPro and V+ from the power supply to +24V on the xPro. The mains voltage must be wired by a licensed electrician or similarly qualified individual.
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front%20copy.jpg" width="400">

# Stepper Motor Wiring:

# Micro Limit Switch Wiring:
Connect the Blue lead into GND and the white lead to SIG. Leave the middle pin blank.

# Z/XYZ Touch Probe Wiring:
The xPRO-V5 uses a 3 pin connector which is provided with the controller.

To connect the xPRO-V5 to the touch probe, connect the red wire to GND and the black wire to SIG using the provided green EDG connector. Leave the middle pin unconnected.

Connect the probe to the probe port as shown below:

To configure the XYZ probe in CNCjs [click here](https://makerhardware.net/wiki/doku.php?id=add_on_packs:xyz_touch_probe)

# Emergency Stop / Door Sensor
Your emergency stop switch can be wired in 2 ways. The first way is to be wired by an electrician in line with your mains lead to the 24V power supply or as a pause button for the xPRO.

The guide below shows the pause/hold method with our Normally Closed Emergency stop switches (NC).

Ensure that the jumper in the xPRO-V5 is in the disabled (default) position. Wiring it this way will pause the program when activated. To use our NC switches with the xPRO-V5, flash your xPRO with the CNC_xPRO_V5_XYYZ_PWM_NC.bin. located in the main [firmware repository](https://github.com/Spark-Concepts/xPro-V5/tree/main/Firmware). To flash this firmware, refer to the [guide](https://github.com/Spark-Concepts/xPro-V5/wiki/Checking_firmware_and_upgrading).

# 0-10V Adjustment:

# Plasma Cutter Connection:

# Bootloader Mode:

# Configuring your xPRO-V5 with CNCjs

## CNCjs Overview

## Connecting you xPRO-V5 to CNCjs

# Machine configuration settings

# Fine-tunin, motor direction and homing

## Inverting Motor direction
## Inverting the homing direction
## Jogging
## Probing with the XYZ probe

# xPRO V5 Laser Control
```diff
Laser safety goggles MUST be worn whenever the laser unit’s power supply is turned on.
```
CNCjs is recommended for controlling the laser. You may generate the gcode from GRBL laser and then open the gcode in CNCjs It is recommended to wear your safety glasses whenever your laser module has its power supply turned on.

The 2.5W and 15W diode lasers are built for a variety of custom laser applications so custom wiring will be required. These laser kits can be adapted for use in CNC applications with longer cables and custom mounting hardware.

To connect the laser to the Maker Shield, you will only need to connect the signal wires from the laser control board to the Maker Shield controller. All other connections are already pre-done. Locate the TTL signal port circled in orange as shown below:

TTL+ connects to the PWM connector and TTL- connects to GND on the TOOLHEAD connector port as illustrated below. The lime green wire corresponds to TTL- (Black wire) and the red wire corresponds to TTL+.

Flash the xPRO with the appropriate firmware.
- [Normally Closed Emergency Stop Switch](https://www.makerstore.com.au/download/software/CNCxPROv5_XYYZ_NC_Laser_2.bin)
- Normally Open Emergency Stop Switch(https://www.makerstore.com.au/download/software/CNCxPROv5_XYYZ_NO_Laser_2.bin)
In the CNCjs software, you can enable the laser mode by typing the following commands in the console:

$Spindle/Type=Laser
$Gcode/LaserMode=ON
$31=1.