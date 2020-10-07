# Connect Power Supply

Please note: The xPro-V5 utilizes Trinamic 5160 stepper drivers running at up to 4A RMS, and over 6A peak - we recommend using a UL listed 24v Power Supply rated at or above 300W (such as the Meanwell LRS350 24v Power Supply)
  

## Set the 115/230v Switch

Before connecting your power supply to mains power, it is critical that you check and set the 115v/230v Switch on your PSU.

<img src="https://i.stack.imgur.com/zWdO5.jpg" width="400">

* [Website](https://electronics.stackexchange.com/questions/479800/why-do-some-smps-power-supplies-require-an-input-voltage-select-switch) 

## Connecting 24v Power Supply to the xPro-V5

Connect Positive ("+24V") and Negative ("GND") from your Power Supply to the xPro-V5 using the supplied [(5mm) Connector](https://media.digikey.com/Photos/On%20Shore%20Technology%20Photos/OSTTJ027150.jpg) - be sure to carefully observe correct polarity.

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Front%20copy.jpg" width="400">

## Connect Motors

* How to: [verify coil wires](https://www.youtube.com/watch?v=S0pGKgos498) 
* Simple verification of 4 wire stepper motors:
 1. Using a multi-meter, resistance Set the Ohmmeter to the lowest resistance scale.
 2. Attach one of the wires to the meter and, in turn, check each of the remaining three
 3. Mark the wire that gave a reading and mark the first wire you started with (coil A)
 4. The remaining two wires will be coil B

_NOTE: switching the polarity of either (one) coil will reverse the stepper's direction_
![coils](https://cdn8.bigcommerce.com/s-itwgldve/images/stencil/1280x1280/products/180/3534/Nema_23_motor_4__11459.1537467879.png?c=2?imbypass=on)
* Connect stepper motor wires to 3.5mm motor terminals

## Connector Pinouts
Each of the motors has a 4 terminal screw-down block wired as:

	Pin | Motor Phase (Output) | Description
	----|--------------------- |-------------
	1 | A1 | Coil A Positive
	2 | A2 | Coil A Negative
	3 | B2 | Coil B Negative
	4 | B1 | Coil B Positive


* [Website](http://lasergrbl.com/en/)

## LaserWeb4

* [Github](https://github.com/LaserWeb/LaserWeb4)
* [Website](https://cncpro.yurl.ch/)

## Candle

![](https://github.com/Denvi/Candle/raw/master/screenshots/screenshot_heightmap_original.png)

* [Website](https://github.com/Denvi/Candle)

## Grbl-Plotter

<img src="https://github.com/svenhb/GRBL-Plotter/raw/master/doc/GRBLPlotter_GUI.png" width="400">

* [Website](https://github.com/svenhb/GRBL-Plotter)
