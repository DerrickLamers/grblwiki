TODO: Write an extensive description of how the homing cycle works and solutions to common problems/misunderstanding.

# # Prerequisites:
 * You have successfully uploaded the grbl onto your Arduino board
 * You have a way to communicate with grbl through USB connection
 * Communication between Arduino and Stepper Motor drivers has been established
 * X, Y and Z Stepper motors have been connected to stepper motor drivers
 * You are able to move stepper motor by sending g-code

# # Test Stepper Motors Moving Direction:
  At this point, you can simply use Arduino IDE to send command to Arduino Grbl board. When you start a "Serial Monitor" in Arduino IDE, you will see those lines appearing in the response window.

  To move the motors, type in the command as below:

  ``G1 X5 F100``
  Then:
  ``G1 X0 F100``

  See whether the X axis moving to the right direction, + to right, - to left.

  ``G1 Y5 F100``
  Then:
  ``G1 Y0 F100``

  See whether the Y axis moving to the right direction, if you are facing the CNC machine, spindle move away from you is Y+, and spindle moving toward you is Y-.

  ``G1 Z5 F100``
  Then:
  ``G1 Z0 F100``

  See whether the Z axis moving to the right direction, spindle moving up is Z+, and spindle moving down is Z-.
  
  If you see motor moving the direction as expected, continue. Otherwise, you will need to change the $3 parameters accordingly. see Grbl Configuration Wiki.

# # Home switches pins and wiring
  3 digital input pins are used for signaling Grbl. 

  ``Pin 9: X Axis limit/Home input pin``

  ``Pin 10: Y Axis Limit/Home input pin``

  ``Pin 12: Z Axis Limit/Home input pin``

  You limit switches usually have three terminals. One is common terminal, one is normally open to common terminal and another one is normally closed to common. In this case, we are going to use two terminals, normally open and common.

  ``X- Limit NO``  ==> ``Arduino Pin9``

  ``X- Limit COM`` ==> ``Arduino GND``

  ``X+ Limit NO``  ==> ``Arduino Pin 9``

  ``X+ Limit COM`` ==> ``Arduino GND``

  ``Y- Limit NO``  ==> ``Arduino Pin 10``

  ``Y- Limit COM`` ==> ``Arduino GND``

  ``Y+ Limit NO``  ==> ``Arduino Pin 10``

  ``Y+ Limit COM`` ==> ``Arduino GND``

  ``Z- Limit NO``  ==> ``Arduino Pin 12``

  ``Z- Limit COM`` ==> ``Arduino GND``

  ``Z+ Limit NO``  ==> ``Arduino Pin 12``

  ``Z+ Limit COM`` ==> ``Arduino GND``

# # Enable Home Cycle and Setup Home Parameters

    To enable home cycling, set parameter ``$23 = 1``

# # Homing Cycle Steps

1.    Z Axis will move up with Fast Rate
1.    When Z home switch triggered, Z stop and back off a distance
1.    Z Axis will move up slowly util touch Z home switch again
1.    Z Axis back off a distance
1.    X, Y Axis move both to Homing direction
1.    The first Axis triggers the switch will stop and wait for the second axis to trigger
1.    When second axis triggers the switch, both axis back off a distance
1.    Both X and Y axis will move toward switches again slowly, until both switches triggered again
1.    Both X and Y axis will back off a distance
1.    Homing Cycle completed 




- Ensure the machine is moving in the correct directions, i.e. Z+ up.
- How to enable homing and invoke it. 
- Default search is positive direction. Invert homing direction to look in other direction.
- Explain homing process. Limit switch search and locate cycles.
- Explain that homing search will look for switches within 1.5x max travel setting, and locate cycle within 5x pull off. 
- What is pull-off and homing delay. Pull off setting must be large enough to clear switch to avoid homing error. 

- How to configure for custom homing cycles.
- Common issues.
- And much more...