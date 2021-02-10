### Currently Supported G and M Codes

* [G0](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g0), [G1](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g1): Linear Motions
 * [G2, G3](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g2-g3): Arc and Helical Motions
 * [G4](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g4): Dwell
 * [G10 L2](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g10-l2), [G10 L20](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g10-l20): Set Work Coordinate Offsets
 * [G17, G18, G19](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g17-g19.1): Plane Selection
 * [G20, G21](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g20-g21): Units
 * [G28, G30](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g28-g28.1): Go to Pre-Defined Position
 * [G28.1, G30.1](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g30-g30.1): Set Pre-Defined Position
 * [G38.2, G38.3, G38.4, G38.5](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g38): Probing
 * [G40](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g40): Cutter Radius Compensation Modes OFF (Only)
 * [G43.1, G49](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g43): Dynamic Tool Length Offsets
 * [G53](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g53): Move in Absolute Coordinates
 * [G54, G55, G56, G57, G58, G59](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g54-g59.3): Work Coordinate Systems
 * [G61](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g61-g61.1): Path Control Modes
 * [G80](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g80): Motion Mode Cancel
 * [G90, G91](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g90-g91): Distance Modes
 * [G91.1](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g90.1-g91.1): Arc IJK Distance Modes
 * [G92](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g92): Coordinate Offset
 * [G92.1](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g92.1-g92.2): Clear Coordinate System Offsets
 * [G93, G94](http://www.linuxcnc.org/docs/html/gcode/g-code.html#gcode:g93-g94-g95): Feedrate Modes
 * [M0, M2, M30](http://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m2-m30): Program Pause and End
 * [M3, M4, M5](http://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m3-m4-m5): Spindle Control
 * [M7 , M8, M9](http://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m7-m8-m9): Coolant Control
 * M56 : Parking Motion Override Control
 * [M62, M63](http://www.linuxcnc.org/docs/html/gcode/m-code.html#mcode:m62-m65) Digital Output Control 

### Comments

[Gcode comments](http://www.linuxcnc.org/docs/html/gcode/overview.html#gcode:comments) are supported in the following formats.

 * Parentheses. Both internal to the line and at the end
   * G0 (Rapid to start) X1 Y1
   * G0 X1 Y1 (Rapid to start)
 * Parentheses with message. If you have MSG, inside the parentheses it will send a Grbl message to all all open interfaces (Serial, Bluetooth Wifi, etc)
   * T4 M6 (MSG, Using tool 4 6mm end mill)  ... This will display [MSG: Using tool 4 6mm end mill]
 * Semicolon. These are for comments at the end
   * M30 ; End of program.


