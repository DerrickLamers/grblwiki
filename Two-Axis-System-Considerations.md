When setting up a system with only two axes, such as a laser engraver or pen plotter, there are a couple of items that must be considered or you will run into strange errors and unexplainable behavior.

# Homing Configuration

Ensure you've modified the config.h properly before uploading the GRBL code to your Arduino or homing may fail and it won't be obvious why. The default 3-axis homing mechanism is defined in config.h as:

```
#define HOMING_CYCLE_0 (1<<Z_AXIS)                // REQUIRED: First move Z to clear workspace.
#define HOMING_CYCLE_1 ((1<<X_AXIS)|(1<<Y_AXIS))  // OPTIONAL: Then move X,Y at the same time.
```

You will note here that the Z axis is homed first, which will work fine as long as you use only soft limits. The system will appear to hang for a few moments, and then home the X and Y axes properly. However, if you've also set up hardware limit switches and hard limits ($21=1), the system will hang indefinitely because while homing Z, it will never hit the non-existent Z limit switch. 

Comment out the above lines in config.h and uncomment one of the two provided two-axis configurations:

```
// NOTE: The following are two examples to setup homing for 2-axis machines.
// #define HOMING_CYCLE_0 ((1<<X_AXIS)|(1<<Y_AXIS))  // NOT COMPATIBLE WITH COREXY: Homes both X-Y in one cycle. 

// #define HOMING_CYCLE_0 (1<<X_AXIS)  // COREXY COMPATIBLE: First home X
// #define HOMING_CYCLE_1 (1<<Y_AXIS)  // COREXY COMPATIBLE: Then home Y
```

The first one will work fine for non-corexy systems where you can home both X and Y and the same time. Use the second one for corexy systems where homing both X and Y at the same time is not possible.

# Limit Switches

Jumper your Z limit terminals to ground for normally closed switch configurations.

If you've built your system to use normally-open limit switches, you will avoid this issue because the unconnected Z axis is normally open by default. However, if you've wired your system to use normally-closed limit switches then, unless you jumper your Z limit pins to ground, GRBL will always start up with a limit-switch fault. You'll scratch your head, wondering why your limit switches seems to be working properly but there's always a limit-switch fault reported. 

For more information on limit switch configurations, refer to the [limit switch page](https://github.com/gnea/grbl/wiki/Wiring-Limit-Switches).