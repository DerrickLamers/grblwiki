# Prerequisites:

 * Correctly configured axes.

# Home switches pins and wiring

3 digital input pins are used for signaling Grbl:

- *Pin 9* X Axis limit/Home input pin
- *Pin 10* Y Axis limit/Home input pin
- *Pin 12* Z Axis limit/Home input pin

Another place that explain the Limit switch configuration: [Wiring-Limit-Switches](https://github.com/gnea/grbl/wiki/Wiring-Limit-Switches)
Your limit switches usually have three terminals. One is common terminal, one is normally open to common terminal and another one is normally closed to common. In this case, we are going to use two terminals, normally open (`NO`) and common (`COM`). You can use all NC instead of NO by configuring $5=1. All the common lines go to the arduino's GND, the `NO` lines go to the pin for that axis. This will result in this wiring:


- *X- limit `NO` -> Arduino Pin 9
- *X- limit `COM` -> Arduino Pin GND
- *X+ limit `NO` -> Arduino Pin 9
- *X+ limit `COM` -> Arduino Pin GND
- *Y- limit `NO` -> Arduino Pin 10
- *Y- limit `COM` -> Arduino Pin GND
- *Y+ limit `NO` -> Arduino Pin 10
- *Y+ limit `COM` -> Arduino Pin GND
- *Z- limit `NO` -> Arduino Pin 12
- *Z- limit `COM` -> Arduino Pin GND
- *Z+ limit `NO` -> Arduino Pin 12
- *Z+ limit `COM` -> Arduino Pin GND

# Enable Home Cycle and Setup Home Parameters

Homing is controlled by parameter `$22`. Type `$22=1` to enable it, `$22=0` to disable it. Homing can be triggered by typing `$H`.

# Homing direction

The homing directions are controlled by setting `$23`. In the default setting (`$23=0`), the home location is the top right of your work area, with the spindle all the way up. If you want the left to be the home location, add 1 to setting `$23`, if you want the bottom of your work area to be the home location, add 2 to `$23`. You probably don't want spindle down to be the home location, but if you do, add 4 to setting `$23`

# Homing Cycle Steps

By default, the homing cycle goes through the following steps:

- Z axis
  1.    Z Axis will move up (positive) with Fast Rate (`$24`)
  1.    When Z home switch triggered, Z stop for a short time (`$26`) and back off a distance (`$27`)
  1.    Z Axis will move up slowly util it touches the Z home switch again (`$25`)
  1.    Z Axis backs off a small distance (`$27`)
- X and Y axis
  1.    X, Y Axis move both to Homing direction at fast rate  (`$24`)
  1.    The first Axis triggers the switch will stop and wait for the second axis to trigger
  1.    When second axis triggers the switch, both axis back off a distance  (`$27`)
  1.    Both X and Y axis will move toward switches again slowly, until both switches triggered again  (`$25`)
  1.    Both X and Y axis will back off a small distance (`$27`)

# Homing speed

As described above, homing is done in two distinct phases per axis: feed and seek. The feed speed is controlled by setting `$24`. In this phase, GRBL is just trying to find the limit switch within a reasonable amount of time.

After the feed phase, the seek phase does exactly the same thing, but at a low speed, controlled by setting `$25`. This phase is all about accurately finding the trigger point for the limit switch.

# Homing travel

GRBL will give up searching for a limit switch after 1.5x the max travel distance. The max travel distance is controlled by `$130`, (for x), `$131` (for y) and `$132` (for z). These numbers are also used for soft-limits, and should be set slightly below the length of your axes.

After the feed phase, the axis moves back a little, to un-trigger the switch. This distance is controlled by setting `$27`. Set this number high enough so the limit switch is cleared, even when the feed phase overshoots.

# TODO (documentation)

- How to configure for custom homing cycles.
- Common issues.
- What's the homing cycle for? How do you use it? Is it really that use? YES.
  - Explain work coordinate systems(WCS) and G28/30 move to predefined locations
  - Explain how to save WCS and G28/30 coordinate frames using G10 and G28.1/30.1.
  - Provide clear link to LinuxCNC g-code descriptions
  - Explain how WCS and G28/30 are used in common scenarios.
