# Connect Power Supply

Please note: The xPro-V5 utilizes Trinamic 5160 stepper drivers running at up to 4A RMS, and over 6A peak - we recommend using a UL listed 24v Power Supply rated at or above 300W (such as the Meanwell LRS350 24v Power Supply)
  

## Set the 115/230v Switch

Before connecting your power supply to mains power, it is critical that you check and set the [115v/230v Switch](https://i.stack.imgur.com/zWdO5.jpg) on your PSU.

## Connecting 24v Power Supply to the xPro-V5

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front%20copy.jpg" width="400">


# Connect Motors

## By default the motor current is set for Nema23, 2.8A motors - verify your motor max current and adjust the [current settings](https://github.com/Spark-Concepts/xPro-V5/wiki/Changing-settings#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings) if needed.  Your motors should run warm to the touch.

Warning: Never connect/disconnect motors to a powered-up controller. Always turn off power before connecting/disconnecting accessories to/from your xPro

* How to: [verify coil wires](https://www.youtube.com/watch?v=S0pGKgos498) 
* Simple verification of 4 wire stepper motors:
 1. Using a multi-meter, resistance Set the Ohmmeter to the lowest resistance scale.
 2. Attach one of the wires to the meter and, in turn, check each of the remaining three
 3. Mark the wire that gave a reading and mark the first wire you started with (coil A)
 4. The remaining two wires will be coil B

* Connect stepper motor wires to the supplied [3.81mm connectors](https://media.digikey.com/photos/FCI%20Photos/20020004-D031B01LF.jpg) and plug in to the corresponding receptacles on the xPro-V5.  Wire the above identified coil A across the A+ and A- terminal, wire coil B across the B+ and B- terminals. 

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/End%20copy.jpg" width="400">

NOTE: switching the polarity of either (one) coil will reverse the stepper's direction - also the color codes shown below may not apply to your particular Motors.  _This is used for machines with ganged axis' with motors facing opposite directions. _

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/nema23-reversed-y2.jpg" width="600">

# Connecting Limit Switches

Warning: Incorrect wiring can short V+ to GND causing damage to your controller: Double check wiring before powering on
 
Use a 2-wire cable to wire up regular microswitches

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/regular-microswitches.jpg" width="600">

The use of a 3-wire inductive proximity switch SN-04-N(NO) or SN-04-N2(NC) may also be used. Standard inductive proximity switch wiring is brown = V+, blue = gnd, and black = signal (verify using the manufacturer's data sheet). Most inductive NPN NO sensors require 10 to 30 volts to operate; though most sensor's may still work using 5V, it is still recommended that you adjust the limit-switch logic voltage select jumper to 24V. Thus 24v is connected to the brown V+ and ground connected to the blue. 

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Hall_limits_wiring.jpg" width="600">

With no signal is detected, the NO (normally open) black signal wire will output 24v while the NC (normally closed) black signal wire will source a ground. When the sensor detects metal the signal wire will either drop the voltage to zero or rise to 24V. 

# Connect Probe
When wiring the xPRO-V5 to an unpowered touch probe, connect one wire to GND and the other to SIG; **be sure to leave the middle pin unconnected**, otherwise permanent damage may result

_Please be sure to double-check your wiring before plugging in all accessories!_ 

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/z-touchplate2.jpg" width="600">  

***
When connecting a powered probe, please refer to the diagram below:
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/xyzprobe-wiring.jpg" width="800">

# Connect Tools

## Dewalt Spindle

You can install an IOT Relay to control your spindle using Gcode commands

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/iotrelay.jpg" width="600">

## 0-10v Analog Signal / VFD

Spark Concepts xPro-V5 includes a 0-10v Analog Signal Voltage output that can be used to control spindles/other toolheads that need a 0-10v signal to run.

Note: This is a low level logic Signal voltage, it should not be used to drive anything directly. This signal should be connected to an external drive system, for example a VFD or a DC Spindle Controller. 

To use the signal, connect between the GND and 0-10v pins on the toolhead plug as shown.

Calibrate output voltage
TIP: You may need to fine tune the output to be exactly 10v:

Send an M3 S12000 to the controller (12000 = default Grbl configuration, or send S=what you have set for $30 - Max spindle speed, RPM)
Measure the voltage between GND and the 0-10v Terminal
Use a small flat head screwdriver to adjust the 0-10v Fine Tuning Adjustment until the output is exactly 10.0v
This will ensure that command Spindle RPM is as close to the actual as possible - M5 command turns the spindle output off

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/0-10v.jpg" width="800">

# Connect Coolant Output

The Coolant output is primarily used to control chip-evacuation, dust-extraction or cutting fluid systems, but can also be creatively repurposed for other switching requirements.

It can be controller with M-Codes:

M8 = On; M9 = Off

Electrical Specifications:
- Voltage: jumper select - 24v(default) or 5v
- Max Current: 3A (24v only)
- Suitable for inductive loads (24v only)

## Connect LED Ring

You can use the Coolant output to switch any other 24v device as well, as an example, you can connect a Spindle LED Ring as shown to put it under M-Code control (for example if you want a Job to turn the LED ring on at the start and off at the end, you can add an M8 to the header and an M9 to the footer of your g-code)

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-led-ring.jpg" width="600">

## Connect Dust Extraction via IoT Relay

You can use our IoT Relay Power Strip to control a Vacuum for dust extraction

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-iot-vacuum.jpg" width="800">

## Connect 24vDC Air Solonoid

You can switch 24v Solenoids using the Coolant output. Typically you'd use this configuration in-line between an Air-compressor and a Nozzle pointed at the endmill. Compressed air blowing on the endmill will help evacuate chips from the cut, and keep the endmill cool

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/coolant-solenoid.jpg" width="600">





