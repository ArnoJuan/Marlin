# Marlin
MKS Gen 1.4 with BL-Touch customized version.

# Description
Model：<br/>
Prusa i3 DIY with Aluminum extruded line track

Firmware：<br/>
Marlin version 1.1.8

Electrical and Mechanical：<br/>
MKS Gen v1.4 Integral forming board (Arduino Mega 2560 R3 + Ramps 1.4)<br/>
TMC2130 chip 5pcs (XYZZE, A4988 8pcs spare)<br/>
Motor rectifier filter 4pcs (for XYZZ)<br/>
Stepping motor 1.3A 5pcs<br/>
MK8 Extruder 1 set<br/>
TFT3.5" Touch screen 2pcs (one spare)<br/>
Broken material detection 1pcs<br/>
Omron switch 2pcs (for X & Y axis)<br/>
BL-Touch 1pcs (for Z axis)<br/>
Thermistor temperature detection 2pcs (Extruder head, heat bed)<br/>
300W Power supply 1pcs<br/>

Organization：<br/>
European regulation 2020 aluminum<br/>
Line track - Triaxial rail(for XYYZZ, total 5pcs)<br/>
Plum coupling 2pcs<br/>
T8 screw 8mm lead 2pcs<br/>
T8 screw elimination gap 2set<br/>
5125 Mini blower 1pcs (for TMC2130 Chip cooling)<br/>
4010 Silent fan 3pcs (Board 2pcs、Extruder head 1pcs)<br/>
3010 Silent fan 2pcs (Extruder head, one spare)<br/>

# Custom parameters
X & Y axis:<br/>
GT2 belt & 16 teeth synchronous wheel<br/>
GT2 belt pitch 2mm, a circle of 16 teeth that move around in a circle 32mm.<br/>
Stepping motor step angle of 1.8 degrees, the chip-driven 16 microstep.<br/>
It means that it takes (360 / 1.8) x 16 = 3200 steps to complete a revolution.<br/>
This axis moves 1mm requires 3200 / 32 = 100 steps.<br/>

Z axis:<br/>
T8 screw lead 8mm, means a move will be 8mm.<br/>
Stepping motor step angle of 1.8 degrees, the chip-driven 16 microstep.<br/>
It means that it takes (360 / 1.8)x16 = 3200 steps to complete a revolution.<br/>
This axis moves 1mm requires 3200 / 8 = 400 steps.<br/>

Extruder:<br/>
MK8 extrusion gear, gear diameter 11mm.<br/>
The circumference is 11xPI = 34.54mm<br/>
Stepping motor step angle of 1.8 degrees, the chip-driven 16 microstep.<br/>
It means that it takes (360 / 1.8) x 16 = 3200 steps to complete a revolution.<br/>
This axis moves 1mm requires 3200 / 34.54 = 92.646 steps.<br/>
Formula: (3200 x Extruder gear ratio) / (pix extrusion gear diameter)<br/>

# Stepper motor IC:<br/>
Stepper motor 1.3A, A4988 Rs = 0.1Ω, current limit 2A, Vref should be set to 1.6V.<br/>
(1.4/0.7)x8x0.1Ω = 1.6<br/>
The current actual setting is 0.65V / (8 x 0.1Ω) = 0.8125A Convert RMS current => 0.815A / 1.41 = 0.578A<br/>
Stepper motor 1.3A with TMC2130 chip:<br/>
The current actual setting is 1.0V x 1.77 / 2.5 = 0.71A, acceleration is set to 1000 mm/s2.<br/>

# BL-Touch auto-leveling related commands:<br/>
X_PROBE_OFFSET_FROM_EXTRUDER 2  // X offset: -left  +right  [of the nozzle]<br/>
Y_PROBE_OFFSET_FROM_EXTRUDER -50  // Y offset: -front +behind [of the nozzle]<br/>
Z_PROBE_OFFSET_FROM_EXTRUDER 0   // Z offset: -below +above  [of the nozzle]<br/>
G28 (X, Y return to zero, Z back to zero in the middle of heat bed)<br/>
G29 (Generating a 3x3 detection matrix)<br/>
M851 Z-1.7 (Adjust Z-axis height)<br/>
M420 V (View the detection matrix)<br/>
M420 S1 (Auto-leveling enabled)<br/>
M500 (Save parameter to EEPROM)<br/>
M501 (Load EEPROM storage parameters)<br/>
M502 (Clear EEPROM parameters)<br/>
M503 (View EEPROM parameters)<br/>

# Extruder head, heat bed temperature control auto tuning PID parameters:<br/>
M303 E0 C8 S200 (PID parameters automatically adjust the extrusion head, the temperature shock stopped after 8 times, the target temperature of 200 degrees)<br/>
M303 E-1 C8 S70 (PID automatically adjust the hot bed parameters, temperature shock stop after 8 times, the target temperature of 70 degrees)<br/>
If you receive the error message "PID Autotune failed! Temperature too high", it means that the initial test condition will cause the temperature to exceed the target temperature by 20 degrees.<br/>
This needs to correct the PID_MAX in firmware. (Heating head maximum current limit)<br/>
After receiving the message "PID Autotune finished! Put the Kp, Ki and Kd constants into Configuration.h",<br/>
Please replace DEFAULT_Kp, DEFAULT_Ki, and DEFAULT_Kd in Configuration.h with the last Kp, Ki, Kd values in the test.<br/>
