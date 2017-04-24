# Prerequisites

* You have successfully uploaded the grbl onto your Arduino board
* You have a way to communicate with grbl through USB connection. 
  - You can use the Arduino IDE to do this, by starting the serial monitor. Grbl should greet you with its version number
* Communication between Arduino and Stepper Motor drivers has been established.
* X, Y and Z Stepper motors have been connected to stepper motor drivers
* You are able to move stepper motor by sending g-code

# Test Stepper Motors Moving Direction `$3`

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

# Configure the steps per mm `$100`, `$101`, `$102`

For each of the axes, you will need to figure out how much steps make up one millimeter (internally, grbl is entirely metric). While this can be figured out by trial and error, it is probably best to compute it from a formula: steps_per_mm = (steps_per_rev * microsteps ) / mm_per_rev

The value for `microsteps` is determined by your stepper driver, and the jumpers used to configure it.

The value for `steps_per_rev` is determined by your stepper motor. A typical 1.8° step motor has 200 steps in a full revolution (360°/1.8°).

The value of `mm_per_rev` is determined by your drive mechanism. For lead screws, it is the thread pitch. A typical THSL-300-8D lead screw has a thread pitch of 2mm. For timing belts, it is the gear pitch, multiplied by the number of teeth on the gear. For example, an 8-teeth gt2 drive would advance your axis 16mm per revolution.

Once you've found the number, set it with `$100=value` for the X axis, `$101=value` for the Z axis, `$102=value` for the Z axis.

# Configure max feed rates `$110`, `$111`, `$112`

Feed rates in grbl are configured in mm/minute. There are two maximums or the feed rate: digital and mechanical. You should configure GRBL to use the lowest of the two.

The digital max feed rate is needed to allow grbl some time between steps to do its job. The step frequency should not exceed 30KHz. To compute the maximum allowable feed rate, use this formula: `max_rate` = 60 * 30000 / `steps_per_mm`.

The mechanical max feed rate is mostly determined by the quality of your bearings and the load on your axes. Set it to a low value (e.g. 100mm/minute) and move up until you achieve a reasonable speed.

Use `$110` for the X axis, `$111` for the Y axis and `$112` for the Z axis.

# Configure max acceleration `$120`, `$121`, `$122`

The max acceleration is mostly determined by the power of your motor, the weight of your axis, and the drag on that axis. If your notice your axis losing steps, tune down the acceleration. Note that the same acceleration is used when the tool is engaged, so you should tune this down to allow additional drag if your tool causes any.

Use `$120` for the X axis, `$121` for the Y axis and `$122` for the Z axis.

# Configure max travel `$130`, `$131`, `$132`

This is only required if you intend to use soft limits or homing.

Set these values to the length of your axes in mm.

Use `$130` for the X axis, `$131` for the Y axis and `$132` for the Z axis.





