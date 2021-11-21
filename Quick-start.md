
# Safety Statement
The author of this document is not liable or responsible for any accidents, injuries, equipment damage, property damage, loss of money or loss of time resulting from improper use of electrical or mechanical or software products.

Assembling electrical and mechanical CNC machine components like power supplies, motors, drivers or other electrical and mechanical components involves dealing with high voltage AC (alternating current) or DC (direct current) and other hazardous items which can be extremely dangerous and needs high attention to detail, experience, knowledge of software, electricity, electro-mechanics and mechanics.
```diff
- BEFORE MAKING ANY CONNECTIONS OR DISCONNECTIONS POWER MUST BE REMOVED FROM THE DEVICE AND THE CONTROLLER. FAILURE TO DO SO WILL VOID ANY AND ALL WARRANTIES.

+ Before starting please read through all the instructions.
```

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

To configure the XYZ probe in CNCjs have a look [here](https://makerhardware.net/wiki/doku.php?id=add_on_packs:xyz_touch_probe)


