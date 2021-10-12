**_DISCLAIMER: Lasers are extremely dangerous devices. They can instantly cause fires and permanently damage your vision. Please read and understand all related documentation for your laser prior to using it. The Grbl project is not responsible for any damage or issues the firmware may cause, as defined by its GPL license._**

**\[_免责声明：激光是极其危险的设备。它们会立即引起火灾，永久性地损害你的视力。使用激光器前，请阅读并理解激光器的所有相关文档。Grbl项目不对其GPL许可证定义的固件可能导致的任何损坏或问题负责_\]**

----

Outlined in this document is how Grbl alters its running conditions for the new laser mode to provide both improved performance and attempting to enforce some basic user safety precautions.

\[本文件概述了Grbl如何改变新激光模式的运行条件，以提高性能，并尝试实施一些基本的用户安全预防措施。\]

## Laser Mode Overview \[激光模式概述\]

The main difference between default Grbl operation and the laser mode is how the spindle/laser output is controlled with motions involved. Every time a spindle state `M3 M4 M5` or spindle speed `Sxxx` is altered, Grbl would come to a stop, allow the spindle to change, and then continue. This is the normal operating procedure for a milling machine spindle. It needs time to change speeds. 

\[默认Grbl操作和激光模式之间的主要区别在于主轴/激光输出如何通过涉及的运动进行控制。每当主轴状态'M3 M4 M5'或主轴速度'Sxxx'改变时，Grbl将停止，允许主轴改变，然后继续。这是铣床主轴的正常操作程序。改变速度需要时间。\]

However, if a laser starts and stops like this for every spindle change, this leads to scorching and uneven cutting/engraving! Grbl's new laser mode prevents unnecessary stops whenever possible and adds a new dynamic laser power mode that automagically scales power based on current speed related to programmed rate. So, you can get super clean and crisp results, even on a low-acceleration machine!

\[但是，如果每次更换主轴时激光器都像这样启动和停止，则会导致烧焦和切割/雕刻不均匀！Grbl的新激光模式尽可能防止不必要的停止，并添加了一个新的动态激光功率模式，该模式根据与编程速率相关的当前速度自动调整功率。所以，即使是在低加速度的机器上，你也可以得到超级干净和清晰的效果！\]

Enabling or disabling Grbl's laser mode is easy. Just alter the **$32** Grbl setting. 

\[启用或禁用Grbl的激光模式很容易。只需更改**$32**Grbl设置即可。\]
- **To Enable**: Send Grbl a `$32=1` command. 
- \[**要启用**：向Grbl发送`$32=1`命令。\]
- **To Disable:** Send Grbl a `$32=0` command.
- \[**要禁用：*，请向Grbl发送一个`$32=0`命令。\]

**WARNING:** If you switch back from laser mode to a spindle for milling, you **MUST** disable laser mode by sending Grbl a `$32=0` command. Milling operations require the spindle to get up to the right rpm to cut correctly and to be **safe**, helping to prevent a tool from breaking and flinging metal shards everywhere. With laser mode disabled, Grbl will briefly pause upon any spindle speed or state change to give the spindle a chance to get up to speed before continuing.

 \[**警告：**如果从激光模式切换回主轴进行铣削，则**必须**通过发送Grbl `$32=0`命令来禁用激光模式。铣削操作要求主轴达到正确的转速，以便正确切割，并且是**安全的**，有助于防止刀具断裂和到处抛撒金属碎片。禁用激光模式时，Grbl将在主轴速度或状态发生任何变化时短暂暂停，以使主轴有机会在继续之前达到速度。\]

### Two-axis Systems \[双轴系统\]

If you're building a laser-grbl system, it's probably a two-axis system, since the Z axis is not usually needed. You may wish to read the [Two-Axis System Considerations page](https://github.com/gnea/grbl/wiki/Two-Axis-System-Considerations).

\[如果你正在建造一个激光grbl系统，它可能是一个双轴系统，因为通常不需要Z轴。您可能希望阅读[双轴系统注意事项页](https://github.com/gnea/grbl/wiki/Two-Axis-System-Considerations).\]

## Laser Mode Operation \[激光模式操作\]

When laser mode is enabled, Grbl controls laser power by varying the **0-5V** voltage from the spindle PWM D11 pin. **0V** should be treated as disabled, while **5V** is full power. Intermediate output voltages are also assumed to be linear with laser power, such that **2.5V** is approximate 50% laser power. (A compile time option exists to shift this linear model to start at a non-zero voltage.) 

\[启用激光模式时，Grbl通过改变主轴PWM D11引脚的**0-5V**电压来控制激光功率**0V**应视为禁用，而**5V**为满功率。还假设中间输出电压与激光功率成线性关系，因此**2.5V**约为50%激光功率。（存在一个编译时选项，用于将此线性模型转换为以非零电压启动。）\]

By default, the spindle PWM frequency is **1kHz**, which is the recommended PWM frequency for most current Grbl-compatible lasers system. If a different frequency is required, this may be altered by editing the `cpu_map.h` file. 

\[默认情况下，主轴PWM频率为**1kHz**，这是大多数当前Grbl兼容激光器系统的建议PWM频率。如果需要不同的频率，可以通过编辑`cpu_map.h`文件来更改。\]

The laser is enabled with the `M3` spindle CW and `M4` spindle CCW commands. These enable two different laser modes that are advantageous for different reasons each.

\[激光器通过`M3`主轴顺时针和`M4`主轴逆时针命令启用。这使得两种不同的激光模式因各自不同的原因而具有优势。\]

- **`M3` Constant Laser Power Mode:** 
- **\[**`M3`恒定激光功率模式：**\]**

    - Constant laser power mode simply keeps the laser power as programmed, regardless if the machine is moving, accelerating, or stopped. This provides better control of the laser state. With a good G-code program, this can lead to more consistent cuts in more difficult materials. 
	- \[恒定激光功率模式只是将激光功率保持在编程状态，而不管机器是在移动、加速还是停止。这样可以更好地控制激光状态。有了一个好的G代码程序，这可以在更难的材料中实现更一致的切割。\]
    
    - For a clean cut and prevent scorching with `M3` constant power mode, it's a good idea to add lead-in and lead-out motions around the line you want to cut to give some space for the machine to accelerate and decelerate.
	- \[对于`M3`恒功率模式的清洁切割和防止灼伤，最好在要切割的线路周围添加引入和引出运动，为机器提供一些加速和减速空间。\]

    - NOTE: `M3` can be used to keep the laser on for focusing.
	- \[注：`M3`可用于使激光器保持开启状态以进行聚焦。\]

- **`M4` Dynamic Laser Power Mode:**
- **\[`M4`动态激光功率模式：\]**
    - Dynamic laser power mode will automatically adjust laser power based on the current speed relative to the programmed rate. It essentially ensures the amount of laser energy along a cut is consistent even though the machine may be stopped or actively accelerating. This is very useful for clean, precise engraving and cutting on simple materials across a large range of G-code generation methods by CAM programs. It will generally run faster and may be all you need to use.
	- \[动态激光功率模式将根据相对于编程速率的当前速度自动调整激光功率。它基本上确保了切割时的激光能量一致，即使机器可能停止或主动加速。这对于通过CAM程序生成大量G代码的方法在简单材料上进行干净、精确的雕刻和切割非常有用。它通常会运行得更快，并且可能是您需要使用的全部。\]
    - Grbl calculates laser power based on the assumption that laser power is linear with speed and the material. Often, this is not the case. Lasers can cut differently at varying power levels and some materials may not cut well at a particular speed and/power. In short, this means that dynamic power mode may not work for all situations. Always do a test piece prior to using this with a new material or machine.
	- \[Grbl基于激光功率与速度和材料成线性关系的假设计算激光功率。通常情况并非如此。激光可以在不同的功率水平下进行不同的切割，某些材料在特定的速度和/或功率下可能切割不好。简而言之，这意味着动态电源模式可能不适用于所有情况。在将其与新材料或机器一起使用之前，务必进行试件测试。\]
    - When not in motion, `M4` dynamic mode turns off the laser. It only turns on when the machine moves. This generally makes the laser safer to operate, because, unlike `M3`, it will never burn a hole through your table, if you stop and forget to turn `M3` off in time.
	- \[不运动时，`M4`动态模式关闭激光器。它只在机器移动时打开。这通常会使激光操作更安全，因为与`M3`不同，如果你停下来并忘记及时关闭`M3`，它永远不会在你的桌子上烧一个洞。\]

Describe below are the operational changes to Grbl when laser mode is enabled. Please read these carefully and understand them fully, because nothing is worse than a garage _**fire**_.

\[以下是启用激光模式时Grbl的操作变化。请仔细阅读并充分理解，因为没有什么比车库火灾更糟糕了。\]

- Grbl will move continuously through **consecutive** motion commands when programmed with a new `S` spindle speed (laser power). The spindle PWM pin will be updated instantaneously through each motion without stopping.
- \[当使用新的`S`主轴速度（激光功率）编程时，Grbl将通过**连续**运动命令连续移动。主轴PWM引脚将在每次运动中即时更新，而不会停止。\]
	- Example: The following set of g-code commands will not pause between each of them when laser mode is enabled, but will pause when disabled.
	- \[示例：启用激光模式时，以下g代码命令集不会在每个g代码命令之间暂停，但禁用时将暂停。\]
	
		```
		G1 X10 S100 F50
		G1 X0 S90
		G2 X0 I5 S80
		``` 
	- Grbl will enforce a laser mode motion stop in a few circumstances. Primarily to ensure alterations stay in sync with the G-code program.
	- \[Grbl将在少数情况下强制激光模式运动停止。主要是为了确保变更与G代码计划保持同步。\]

		- Any `M3`, `M4`, `M5` spindle state _change_. 
		- \[任何'M3'、'M4'、'M5'主轴状态变化。\]
		- `M3` only and no motion programmed: A `S` spindle speed _change_.
		- \[`M3`仅可编程且不可编程运动：A`S`主轴速度变化。\]
		- `M3` only and no motion programmed: A `G1 G2 G3` laser powered state _change_ to `G0 G80` laser disabled state.
		- \`M3`仅限且未编程运动：G1 G2 G3`激光供电状态\更改为`G0 G80`激光禁用状态。[\]
		- NOTE: `M4` does not stop for anything but a spindle state _change_.
		- \[注意：`M4`只在主轴状态改变时停止。\]

- The laser will only turn on when Grbl is in a `G1`, `G2`, or `G3` motion mode. 
- \[只有当Grbl处于`G1`、`G2`或`G3`运动模式时，激光器才会开启。\]

	- In other words, a `G0` rapid motion mode or `G38.x` probe cycle will never turn on and always disable the laser, but will still update the running modal state. When changed to a `G1 G2 G3` modal state, Grbl will immediately enable the laser based on the current running state.
	- \[换言之，`G0`快速运动模式或`G38.x`探头循环将永远不会开启并始终禁用激光器，但仍将更新运行模式状态。当更改为`G1 G2 G3`模式状态时，Grbl将根据当前运行状态立即启用激光器。\]
	
	- Please remember that `G0` is the default motion mode upon power up and reset. You will need to alter it to `G1`, `G2`, or `G3` if you want to manually turn on your laser. This is strictly a safety measure.
	- \[请记住，`G0`是通电和复位时的默认运动模式。如果要手动打开激光器，需要将其更改为`G1`、`G2`或`G3`。这是严格的安全措施。\]
	
	- Example: `G0 M3 S1000` will not turn on the laser, but will set the laser modal state to `M3` enabled and power of `S1000`. A following `G1` command will then immediately be set to `M3` and `S1000`.
	- \[示例：`G0 M3 S1000`不会打开激光器，但会将激光器模式状态设置为`M3`已启用，功率为`S1000`。随后的`G1`命令将立即设置为`M3`和`S1000`。\]

	- To have the laser powered during a jog motion, first enable a valid motion mode and spindle state. The following jog motions will inherit and maintain the previous laser state. Please use with caution though. This ability is primarily to allow turning on the laser on a _very low_ power to use the laser dot to jog and visibly locate the start position of a job.
	- \[要使激光器在点动运动期间通电，首先启用有效的运动模式和主轴状态。以下点动运动将继承并保持先前的激光状态。但请谨慎使用。该功能主要允许在非常低的功率下打开激光器，以使用激光点进行点动，并明显定位作业的开始位置。\]


- An `S0` spindle speed of zero will turn off the laser. When programmed with a valid laser motion, Grbl will disable the laser instantaneously without stopping for the duration of that motion and future motions until set greater than zero.
- \[`S0`主轴转速为零将关闭激光器。当使用有效的激光运动编程时，Grbl将在该运动和未来运动的持续时间内立即禁用激光，直到设置为大于零。\]

	- `M3` constant laser mode, this is a great way to turn off the laser power while continuously moving between a `G1` laser motion and a `G0` rapid motion without having to stop. Program a short `G1 S0` motion right before the `G0` motion and a `G1 Sxxx` motion is commanded right after to go back to cutting.
	- \[`M3`恒定激光模式，这是一种关闭激光电源的好方法，同时可以在'G1'激光运动和'G0'快速运动之间连续移动，而无需停止。在`G0`运动之前编程一个短的`G1 S0`运动，然后命令一个`G1 Sxxx`运动返回切割。\]


-----
### CAM Developer Implementation Notes \[CAM开发人员实施说明\]

TODO: Add some suggestions on how to write laser g-code for Grbl. 

\[TODO:添加一些关于如何为Grbl编写激光g代码的建议。\]

- When using `M3` constant laser power mode, try to avoid force-sync conditions during a job whenever possible. Basically every spindle speed change must be accompanied by a valid motion. Any motion is fine, since Grbl will automatically enable and disable the laser based on the modal state. Avoid a `G0` and `G1` command with no axis words in this mode and in the middle of a job.
- \[使用`M3`恒定激光功率模式时，尽可能避免在作业期间出现强制同步情况。基本上，每次主轴转速变化都必须伴随有效运动。任何动作都可以，因为Grbl将根据模式状态自动启用和禁用激光器。在这个模式和中间的工作中，避免一个`G0`和`G1`命令，没有轴字。\]

- Ensure smooth motions throughout by turning the laser on and off without an `M3 M4 M5` spindle state command. There are two ways to do this:
- \[通过在无`M3 M4 M5`主轴状态命令的情况下打开和关闭激光器，确保整个过程平稳运行。有两种方法可以做到这一点：\]
    - _Program a zero spindle speed `S0`_: `S0` is valid G-code and turns off the spindle/laser without changing the spindle state. In laser mode, Grbl will smoothly move through consecutive motions and turn off the spindle. Conversely, you can turn on the laser with a spindle speed `S` greater than zero. Remember that `M3` constant power mode requires any spindle speed `S` change to be programmed with a motion to allow continuous motion, while `M4` dynamic power mode does not.
	- \[_编程设定零主轴转速'S0`_.'S0`是有效的G代码，在不改变主轴状态的情况下关闭主轴/激光器。在激光模式下，Grbl将在连续运动中平稳移动，并关闭主轴。相反，您可以在主轴速度大于零的情况下打开激光器。记住，`M3`恒定功率模式要求任何主轴速度的变化都通过运动编程，以允许连续运动，而`M4`动态功率模式则不需要。\]

    - _Program an unpowered motion between powered motions_: If you are traversing between parts of a raster job that don't need to have the laser powered, program a `G0` rapid between them. `G0` enforces the laser to be disabled automatically. The last spindle speed programmed doesn't change, so if a valid powered motion, like a `G1` is executed after, it'll immediately re-power the laser with the last programmed spindle speed when executing that motion.
	- \[_在通电运动之间编程一个无动力运动：如果您正在光栅作业中不需要激光通电的部分之间穿行，请在它们之间编程一个'G0'快速`G0`强制激光器自动禁用。上一次编程的主轴转速不会改变，因此，如果之后执行有效的动力运动，如`G1`，则在执行该运动时，它将立即以上一次编程的主轴转速重新为激光器供电。\]
