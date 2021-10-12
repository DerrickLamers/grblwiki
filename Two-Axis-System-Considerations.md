When setting up a system with only two axes, such as a laser engraver or pen plotter, there are a couple of items that must be considered or you will run into strange errors and apparently unexplainable behavior.

\[当设置只有两个轴的系统（如激光雕刻机或笔式绘图仪）时，必须考虑几个项目，否则您将遇到奇怪的错误和明显无法解释的行为。\]

# Homing Configuration \[归位配置\]

Ensure you've modified GRBL's config.h file properly before uploading the GRBL code to your Arduino or homing may fail and it won't be obvious why. The default 3-axis homing mechanism is defined in config.h as:

\[在将GRBL代码上载到Arduino之前，请确保已正确修改GRBL的config.h文件，否则归位可能会失败，原因不明。默认的3轴归位机构在config.h中定义为：\]

```
#define HOMING_CYCLE_0 (1<<Z_AXIS)                // REQUIRED: First move Z to clear workspace.
#define HOMING_CYCLE_1 ((1<<X_AXIS)|(1<<Y_AXIS))  // OPTIONAL: Then move X,Y at the same time.
```

You will note here that the Z axis is homed first, which will work fine as long as you use only soft limits. The system will appear to hang for a few moments, and then home the X and Y axes properly. However, if you've also set up hardware limit switches and hard limits ($21=1), the system will hang indefinitely because while homing Z, it will never hit the non-existent Z limit switch. 

\[这里您将注意到，Z轴首先被设置为原点，只要您只使用软限制，就可以正常工作。系统将显示为挂起几分钟，然后将X轴和Y轴正确归位。但是，如果您还设置了硬件限位开关和硬限位（$21=1），系统将无限期地挂起，因为在归零Z时，它将永远不会碰到不存在的Z限位开关。\]

Comment out the above lines in config.h and uncomment one of the two provided two-axis configurations:

\[注释掉config.h中的上述行，并取消注释提供的两个双轴配置之一：\]

```
// NOTE: The following are two examples to setup homing for 2-axis machines.
// #define HOMING_CYCLE_0 ((1<<X_AXIS)|(1<<Y_AXIS))  // NOT COMPATIBLE WITH COREXY: Homes both X-Y in one cycle. 

// #define HOMING_CYCLE_0 (1<<X_AXIS)  // COREXY COMPATIBLE: First home X
// #define HOMING_CYCLE_1 (1<<Y_AXIS)  // COREXY COMPATIBLE: Then home Y
```

The first one will work fine for non-corexy systems where you can home both X and Y and the same time. Use the second one for corexy systems where homing both X and Y at the same time is not possible.

\[第一种方法适用于非corexy系统，在这种系统中，可以同时设置X和Y。对于不可能同时对X和Y进行归位的corexy系统，使用第二种方法。\]

# Limit Switches \[限位开关\]

Jumper your Z limit terminals to ground for normally closed switch configurations.

\[对于常闭开关配置，将Z限制端子跨接至接地。\]

If you've built your system to use normally-open limit switches, you will avoid this issue because the unconnected Z axis is normally open by default. However, if you've wired your system to use normally-closed limit switches then, unless you jumper your Z limit pins to ground, GRBL will always start up with a limit-switch fault. You'll scratch your head, wondering why your limit switches seems to be working properly but there's always a limit-switch fault reported. 

\[如果将系统构建为使用常开限位开关，则可以避免此问题，因为未连接的Z轴默认为常开。然而，如果您已将系统接线为使用常闭限位开关，则除非您将Z限位引脚跨接至接地，否则GRBL将始终在限位开关故障时启动。你会挠头，想知道为什么你的限位开关似乎工作正常，但总是有限位开关故障报告。\]

For more information on limit switch configurations, refer to the [limit switch page](https://github.com/gnea/grbl/wiki/Wiring-Limit-Switches).

\[有关限位开关配置的更多信息，请参阅[限位开关页面](https://github.com/gnea/grbl/wiki/Wiring-Limit-Switches).\]
