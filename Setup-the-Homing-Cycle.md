TODO: Write an extensive description of how the homing cycle works and solutions to common problems/misunderstanding.

# # Prerequisites:
 * You have successfully uploaded the grbl onto your Arduino board
 * You have a way to communicate with grbl through USB connection
 * Communication between Arduino and Stepper Motor drivers has been established
 * X, Y and Z Stepper motors have been connected to stepper motor drivers
 * You are able to move stepper motor by sending g-code

# Test Stepper Motors Moving Direction
  At this point, you can simply use Arduino IDE to send command to Arduino Grbl board. When you start a "Serial Monitor" in Arduino IDE, you will see those lines appearing in the response window.

## X axis:

To move the motors, type ``G1 X5 F100`` followed by ``G1 X0 F100``. The spindle should move to the right, and back again.

If the movement is the other way around, change setting `$3`. If `$3` is even, add 1, if `$3` is odd, subtract one.

## Y axis:

Type `G1 Y5 F100` followed by `G1 Y0 F100`. 

If you're using a mobile gantry, the spinle should move away from you, and back again. If you're using a moving bed, the bed should move towards you, and back again.

If the movement is the other way around, change setting `$3`. If `$3` is one of 0, 1, 4 or 5, add 2. Otherwise, subtract 2.

## Z axis

Type `G1 Z-5 F100` followed by `G1 Z0 F00`. The spindle should move down and back again. (note that the initial movement is negative, typically the Z axis uses 0 as the top, so positive movement often isn't possible).

If the movement is the other way around, change settint `$3`. If `$3` is greater or equal to 4, subtract 4, otherwise add 4.


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
- What's the homing cycle for? How do you use it? Is it really that use? YES.
  - Explain work coordinate systems(WCS) and G28/30 move to predefined locations
  - Explain how to save WCS and G28/30 coordinate frames using G10 and G28.1/30.1.
  - Provide clear link to LinuxCNC g-code descriptions
  - Explain how WCS and G28/30 are used in common scenarios.

- And much more...