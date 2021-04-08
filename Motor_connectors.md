# Connect Motors

## By default the motor current is set for Nema23, 2.8A motors - verify your motor max current and adjust the [current settings](https://github.com/Spark-Concepts/xPro-V5/wiki/Changing-settings#xcurrentrun-or-ccurrentrun-140-thru-145--xyzabc--stepper-run-current-extended-settings) if needed.  Your motors should run warm to the touch, but not hot

Warning: Never connect/disconnect motors to a powered-up controller. Always turn off power before connecting/disconnecting accessories to/from your xPro

* How to: [verify coil wires](https://www.youtube.com/watch?v=S0pGKgos498) 
* Simple verification of 4 wire stepper motors:
 1. Using a multi-meter, resistance Set the Ohmmeter to the lowest resistance scale.
 2. Attach one of the wires to the meter and, in turn, check each of the remaining three
 3. Mark the wire that gave a reading and mark the first wire you started with (coil A)
 4. The remaining two wires will be coil B

* Connect stepper motor wires to the supplied [3.81mm connectors](https://media.digikey.com/photos/FCI%20Photos/20020004-D031B01LF.jpg) and plug in to the corresponding receptacles on the xPro-V5

<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/Stepper_drw.jpg" width="800">

_NOTE: the color codes shown may not apply to your particular Motors_; also, depending on your machine configuration, switching the polarity of either (one) coil<sub>(1)</sub> may be required to change the stepper's direction.  
<img src="https://github.com/Spark-Concepts/xPro-V5/blob/main/images/nema23-reversed-y2.jpg" width="600">

