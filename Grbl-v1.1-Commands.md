#### _Quick-Links[快速链接]:_
* [Grbl `$` System Commands](#grbl--commands) [	Grbl`$`系统命令](#grbl--commands)
* [Grbl Real-Time Commands](#grbl-v11-realtime-commands) [Grbl实时命令](#grbl-v11-realtime-commands)


*****
### Please help us keep this page maintained! If you spot a problem, feel free to update it or notify us.
******

# Grbl v1.1 Commands
  \[Grbl v1.1 命令\]

In general, Grbl assumes all characters and streaming data sent to it is g-code and will parse and try to execute it as soon as it can. However, Grbl also has two separate system command types that are outside of the normal g-code streaming. One system command type is streamed to Grbl like g-code, but starts with a `$` character to tell Grbl it's not g-code. The other is composed of a special set of characters that will immediately command Grbl to do a task in real-time. It's not part of the g-code stream. Grbl's system commands do things like control machine state, report saved parameters or what Grbl is doing, save or print machine settings, run a homing cycle, or make the machine move faster or slower than programmed. This document describes these "internal" system Grbl commands, what they do, how they work, and how to use them.

\[ 通常，Grbl假定发送给它的所有字符和流数据都是g代码，并且将解析并尝试尽快执行它。 但是，Grbl还具有两种单独的系统命令类型，它们不在常规g代码流传输之外。 一种系统命令类型像g代码一样流式传输到Grbl，但是以$字符开头告诉Grbl不是g代码。 另一个由一组特殊的字符组成，这些字符将立即命令Grbl实时执行任务。 它不是g代码流的一部分。 Grbl的系统命令可以执行以下操作：控制机器状态，报告已保存的参数或Grbl正在执行的操作，保存或打印机器设置，回参考点循环或使机器运动快于或慢于编程速度运动。 本文档介绍了这些`内部`系统Grbl命令，它们的作用，如何工作以及如何使用它们。\]
## Getting Started 
   \[入门介绍\]

First, connect to Grbl using the serial terminal of your choice.

[首先，您应该选择的串行终端连软件接到 Grbl 控制板。
译注: 国内流行的各种版本的`串口助手`可用]

Set the baud rate  to **115200** as 8-N-1 (8-bits, no parity, and 1-stop bit.) 

\[第二步：设置串口参数：波特率：115200 ，数据位 8 , 停止位：1 效验位：无\]

Once connected
 you should get the Grbl-prompt, which looks like this:

\[一旦连接成功：
你将看到Grbl 的提示信息 \]
```
Grbl 1.1e ['$' for help]
```

Type $ and press enter to have Grbl print a help message. You should not see any local echo of the $ and enter. Grbl should respond with:

\[输入 $ 并按回车键可以让 Grbl 打印帮助信息。您不会看到 $ 的任何本地回声并输入。Grbl 应该回应：\]
```
[HLP:$$ $# $G $I $N $x=val $Nx=line $J=line $SLP $C $X $H ~ ! ? ctrl-x]
ok
```

The ‘$’-commands are Grbl system commands used to tweak the settings, view or change Grbl's states and running modes, and start a homing cycle. The last four **non**-'$' commands are realtime control commands that can be sent at anytime, no matter what Grbl is doing. These either immediately change Grbl's running behavior or immediately print a report of the important realtime data like current position (aka DRO). There are over a dozen more realtime control commands, but they are not user type-able. See realtime command section for details.

\['$'命令是 Grbl 系统命令，用于调整设置、查看或更改 Grbl 的状态和运行模式，以及启动归位循环。最后四个非-'$'命令是实时控制命令，可以随时发送，不管Grbl在做什么。这些要么立即改变 Grbl 的运行行为，要么立即打印当前位置（又名 DRO）等重要实时数据的报告。还有十多个实时控制命令，但它们不是用户可以键入的。有关详细信息，请参阅实时命令部分。\]
***

## Grbl '$' Commands
   [Grbl`$`命令]
The `$` system commands provide additional controls for the user, such as printing feedback on the current G-code parser modal state or running the homing cycle. This section explains what these commands are and how to use them.

\[该`$`系统命令为用户提供额外的控制，如打印反馈对电流G-代码解析器模式状态或运行归位周期。本节说明的是这些命令是什么以及如何使用它们。\]
#### `$$`and `$x=val` - View and write Grbl settings
#### [`$$`和`$x=val`-查看和写入Grbl设置]
See [Grbl v1.1 Configuration](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Configuration#grbl-settings) for more details on how to view and write setting and learn what they are.

有关如何查看和编写设置以及了解它们是什么的更多详细信息，请参阅[GRBLV1.1配置](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Configuration#grbl-settings)

#### `$#` - View gcode parameters\[$# - 查看 gcode 参数\]
G-code parameters store the coordinate offset values for G54-G59 work coordinates, G28/G30 pre-defined positions, G92 coordinate offset, tool length offsets, and probing (not officially, but we added here anyway). Most of these parameters are directly written to EEPROM anytime they are changed and are persistent. Meaning that they will remain the same, regardless of power-down, until they are explicitly changed. The non-persistent parameters, which will are not retained when reset or power-cycled, are G92, G43.1 tool length offsets, and the G38.2 probing data.

\[G 代码参数存储: G54-G59 工作坐标、G28/G30 预定义位置、G92 坐标偏移、刀具长度偏移和探测的坐标偏移值（不是正式的，但我们在这里添加了）。这些参数中的大多数在更改时都会直接写入 EEPROM 并且是持久的。这意味着它们将保持不变，无论断电如何，直到它们被明确更改。G92、G43.1 刀具长度偏置和 G38.2 探测数据等非持久性参数在复位或重新上电时不会保留。\]

G54-G59 work coordinates can be changed via the `G10 L2 Px` or `G10 L20 Px` command defined by the NIST gcode standard and the EMC2 (linuxcnc.org) standard. G28/G30 pre-defined positions can be changed via the `G28.1` and the `G30.1` commands, respectively.

\[G54-G59工作坐标可通过NIST gcode标准和EMC2（linuxcnc.org）标准定义的`G10 L2 Px` 或 `G10 L20 Px`命令进行更改。G28/G30预定义位置可分别通过 `G28.1` 和 `G30.1`命令进行更改。\]

When `$#` is called, Grbl will respond with the stored offsets from machine coordinates for each system as follows. `TLO` denotes tool length offset (for the default z-axis), and `PRB` denotes the coordinates of the last probing cycle, where the suffix `:1` denotes if the last probe was successful and `:0` as not successful.

\[当`$`被调用时，Grbl 将使用存储的每个系统的机器坐标偏移量进行响应，如下所示。`TLO`表示刀具长度偏移（对于默认的 z 轴），`PRB`表示最后一次探测循环的坐标，其中后缀`:1`表示最后一次探测是成功还是`:0`不成功。\]
```
[G54:4.000,0.000,0.000]
[G55:4.000,6.000,7.000]
[G56:0.000,0.000,0.000]
[G57:0.000,0.000,0.000]
[G58:0.000,0.000,0.000]
[G59:0.000,0.000,0.000]
[G28:1.000,2.000,0.000]
[G30:4.000,6.000,0.000]
[G92:0.000,0.000,0.000]
[TLO:0.000]
[PRB:0.000,0.000,0.000:0]
```

#### `$G` - View gcode parser state
\[`$G`查看 gcode 解析器状态\]

This command prints all of the active gcode modes in Grbl's G-code parser. When sending this command to Grbl, it will reply with a message starting with an `[GC:` indicator like: 

\[此命令打印 Grbl 的 G 代码解析器中的所有活动 gcode 模式。将此命令发送到 Grbl 时，它将回复一条以`[GC:`指示符开头的消息，例如：\]
```
[GC:G0 G54 G17 G21 G90 G94 M0 M5 M9 T0 S0.0 F500.0]
```

These active modes determine how the next G-code block or command will be interpreted by Grbl's G-code parser. For those new to G-code and CNC machining, modes sets the parser into a particular state so you don't have to constantly tell the parser how to parse it. These modes are organized into sets called "modal groups" that cannot be logically active at the same time. For example, the units modal group sets whether your G-code program is interpreted in inches or in millimeters.

\[这些活动模式决定了 Grbl 的 G 代码解析器将如何解释下一个 G 代码块或命令。对于 G 代码和 CNC 加工的新手，模式将解析器设置为特定状态，因此您不必经常告诉解析器如何解析它。这些模式被组织成称为`模式组`的集合，它们不能同时在逻辑上处于活动状态。例如，单位模态组设置您的 G 代码程序是以英寸还是毫米为单位进行解释的。\]

A short list of the modal groups, supported by Grbl, is shown below, but more complete and detailed descriptions can be found at LinuxCNC's [website](http://www.linuxcnc.org/docs/2.4/html/gcode_overview.html#sec:Modal-Groups). The G-code commands in **bold** indicate the default modes upon powering-up Grbl or resetting it.

Grbl 支持的模态组的简短列表如下所示，但更完整和详细的描述可以在 LinuxCNC 的[网站上](http://www.linuxcnc.org/docs/2.4/html/gcode_overview.html#sec:Modal-Groups)找到。**粗体**的 G 代码命令表示 Grbl 上电或重置时的默认模式。

| Modal Group \[模式分组\] |  Member Words \[成员命名\] |
|:----:|:----:|
| Motion Mode \[运动模式\] | **G0**, G1, G2, G3, G38.2, G38.3, G38.4, G38.5, G80 |
|Coordinate System Select \[坐标系选择\]	| **G54**, G55, G56, G57, G58, G59|
|Plane Select \[平面选择\]	| **G17**, G18, G19|
|Distance Mode \[距离模式\]	| **G90**, G91|
|Arc IJK Distance Mode \[圆弧 IJK 距离模式\] | **G91.1** |
|Feed Rate Mode \[进给率模式\]	| G93, **G94**|
|Units Mode \[单位模式\]	| G20, **G21**|
|Cutter Radius Compensation \[刀具半径补偿\] | **G40** |
|Tool Length Offset \[刀具长度偏移\] |G43.1, **G49**|
|Program Mode \[程序模式\] | **M0**, M1, M2, M30|
|Spindle State \[主轴状态\] |M3, M4, **M5**|
|Coolant State \[冷却液状态\]	| M7, M8, **M9** |

In addition to the G-code parser modes, Grbl will report the active `T` tool number, `S` spindle speed, and `F` feed rate, which all default to 0 upon a reset. For those that are curious, these don't quite fit into nice modal groups, but are just as important for determining the parser state.

\[除了 G 代码解析器模式之外，Grbl 还将报告活动`T`刀具编号、`S`主轴速度和`F`进给率，这些在复位时均默认为 0。对于那些好奇的人来说，它们并不完全适合于好的模态组，但对于确定解析器状态同样重要。\]

Note that this list does not include the **non-modal** g-code commands group and they are not listed in the `$G` parser report, because they only affect the current line they are commanded in. For completeness, here are the non-modal commands supported by Grbl: 

\[请注意，此列表不包括**非模态**G代码命令组，并且它们未列在`$G`解析器报告中，因为它们只影响命令它们进入的当前线路。为完整起见，以下是Grbl支持的非模态命令：\]
| Supported Non-Modal Commands\[支持的非模态命令\]|
|:----:|
| G4, G10 L2, G10 L20, G28, G30, G28.1, G30.1, G53, G92, G92.1 |

#### `$I` - View build info\[`$I` - 查看构建信息\]
This prints feedback to the user the Grbl version and source code build date. Optionally, `$I` can also store a short string to help identify which CNC machine you are communicating with, if you have more than one machine using Grbl. To set this string, send Grbl `$I=xxx`, where `xxx` is your customization string that is less than 80 characters. This string will be saved as capitalized, white space removed, and can only contain alpha-numeric characters. The next time you query Grbl with a `$I` view build info, Grbl will print this string after the version and build date.

\[这会向用户打印 Grbl 版本和源代码构建日期的反馈。`$I`如果您有不止一台机器使用 Grbl ，也可以存储一个短字符串来帮助识别您正在与哪台 CNC 机器进行通信。要设置此字符串，请发送`$I=xxx`到 Grbl ，其中`xxx`是少于 80 个字符的自定义字符串。该字符串将被保存为大写，删除空格，并且只能包含字母数字字符。下次您使用`$I`视图构建信息查询 Grbl 时，Grbl 将在版本和构建日期之后打印此字符串。\]

NOTE: Some OEMs may block access to over-writing the build info string so they can store product information and codes there.

\[注意：一些 OEM 可能会阻止访问覆盖构建信息字符串，以便他们可以在那里存储产品信息和代码。\]
#### `$N` - View startup blocks\[`$N` - 查看启动块\]
`$Nx` are the startup blocks that Grbl runs every time you power on Grbl or reset Grbl. In other words, a startup block is a line of G-code that you can have Grbl auto-magically run to set your G-code modal defaults, or anything else you need Grbl to do everytime you start up your machine. Grbl can store two blocks of G-code as a system default.

\[`$Nx`是每次打开 Grbl 或重置 Grbl 时 Grbl 运行的启动块。换句话说，启动块是一行 G 代码，您可以让 Grbl 自动神奇地运行它来设置 G 代码模式默认值，或者每次启动机器时需要 Grbl 执行的任何其他操作。Grbl 可以存储两个 G 代码块作为系统默认值。\]

So, when connected to Grbl, type `$N` and then enter. Grbl should respond with something short like:

\[所以，当连接到 Grbl 时，输入`$N`然后回车。Grbl 应该以简短的方式回应，例如：\]
```
$N0=
$N1=
ok
```
Not much to go on, but this just means that there is no G-code block stored in line `$N0` for Grbl to run upon startup. `$N1` is the next line to be run.

\[=后面没什么，但这只是意味着Grbl在启动时运行`$N0`行中没有存储G代码块。`$N1`是要运行的下一行。\]

#### $Nx=line - Save startup block\[`$Nx=line` -保存启动块\]
**IMPORTANT: Be very careful when storing any motion (G0/1,G2/3,G28/30) commands in the startup blocks. These motion commands will run everytime you reset or power up Grbl, so if you have an emergency situation and have to e-stop and reset, a startup block move can and will likely make things worse quickly. Also, do not place any commands that save data to EEPROM, such as G10/G28.1/G30.1. This will cause Grbl to constantly re-write this data upon every startup and reset, which will eventually wear out your Arduino's EEPROM.**

**\[重要提示：在启动块中存储任何运动（G0/1、G2/3、G28/30）命令时要非常小心。这些运动命令将在您每次重置或启动 Grbl 时运行，因此，如果您遇到紧急情况，必须紧急停止并重置，启动块移动可能并且可能会使情况迅速恶化。另外，不要放置任何将数据保存到 EEPROM 的命令，例如 G10/G28.1/G30.1。这将导致 Grbl 在每次启动和重置时不断重写这些数据，最终会磨损 Arduino 的 EEPROM。\]

**Typical usage for a startup block is simply to set your preferred modal states, such as G20 inches mode, always default to a different work coordinate system, or, to provide a way for a user to run some user-written unique feature that they need for their crazy project.**

**启动块的典型用途是简单地设置您喜欢的模态状态，例如 G20 英寸模式，始终默认为不同的工作坐标系，或者为用户提供一种方式来运行他们需要的某些用户编写的独特功能为他们疯狂的项目。**

To set a startup block, type `$N0=` followed by a valid G-code block and an enter. Grbl will run the block to check if it's valid and then reply with an `ok` or an `error:` to tell you if it's successful or something went wrong. If there is an error, Grbl will not save it.

\[要设置启动块，请键入`$N0=`后跟有效的 G 代码块和回车。Grbl 将运行该块以检查它是否有效，然后用`ok`回复`error`:告诉您它是成功还是出错了。如果有错误，Grbl 不会保存。\]

For example, say that you want to use your first startup block `$N0` to set your G-code parser modes like G54 work coordinate, G20 inches mode, G17 XY-plane. You would type `$N0=G20 G54 G17` with an enter and you should see an `ok` response. You can then check if it got stored by typing `$N` and you should now see a response like `$N0=G20G54G17`.

\[例如，假设您想使用第一个启动块`$N0`来设置 G 代码解析器模式，如 G54 工作坐标、G20 英寸模式、G17 XY 平面。您将`$N0=G20 G54 G17`输入回车，您应该会看到`ok`响应。然后，您可以通过键入来检查它是否已存储，`$N`现在您应该会看到类似`$N0=G20G54G17`.\]

Once you have a startup block stored in Grbl's EEPROM, everytime you startup or reset you will see your startup block printed back to you, starting with an open-chevron `>`, and a `:ok` response from Grbl to indicate if it ran okay. So for the previous example, you'll see:

\[一旦您将启动块存储在 Grbl 的 EEPROM 中，每次启动或重置时，您都会看到您的启动块打印回给您，来自Grbl的响应的开头`>`和`:ok`结尾以指示它是否运行正常。因此，对于前面的示例，您将看到：\]
```
Grbl 1.1d ['$' for help]
>G20G54G17:ok

```
If you have multiple G-code startup blocks, they will print back to you in order upon every startup. And if you'd like to clear one of the startup blocks, (e.g., block 0) type `$N0=` without anything following the equal sign.

\[如果您有多个G代码启动块，它们将在每次启动时按顺序打印回您。如果要清除其中一个启动块（例如:块0），请键入`$N0=`，不在等号后面加任何内容。\]

NOTE: There are two variations on when startup blocks will not run. First, it will not run if Grbl initializes up in an ALARM state or exits an ALARM state via an `$X` unlock for safety reasons. Always address and cancel the ALARM and then finish by a reset, where the startup blocks will run at initialization. Second, if you have homing enabled, the startup blocks will execute immediately after a successful homing cycle, not at startup.

\[注意：启动块何时不运行有两种变体。首先，如果 Grbl 在 ALARM 状态初始化或`$X`出于安全原因通过解锁退出 ALARM 状态，它将不会运行。始终寻址和取消 ALARM，然后通过复位完成，其中启动块将在初始化时运行。其次，如果您启用了归位，启动块将在成功归位循环后立即执行，而不是在启动时执行。\]

#### `$C` - Check gcode mode\[`$C` - 检查 gcode 模式\]
This toggles the Grbl's gcode parser to take all incoming blocks and process them completely, as it would in normal operation, but it does not move any of the axes, ignores dwells, and powers off the spindle and coolant. This is intended as a way to provide the user a way to check how their new G-code program fares with Grbl's parser and monitor for any errors (and checks for soft limit violations, if enabled).

\[这会切换 Grbl 的 gcode 解析器以获取所有传入的块并完全处理它们，就像在正常操作中一样，但它不会移动任何轴，忽略停顿，并关闭主轴和冷却剂。这旨在为用户提供一种方法来检查他们的新 G 代码程序如何使用 Grbl 的解析器运行并监视任何错误（并检查是否违反软限制，如果启用）。\]

When toggled off, Grbl will perform an automatic soft-reset (^X). This is for two purposes. It simplifies the code management a bit. But, it also prevents users from starting a job when their G-code modes are not what they think they are. A system reset always gives the user a fresh, consistent start.

\[关闭时，Grbl 将执行自动软重置 (^X)。这是出于两个目的。它稍微简化了代码管理。但是，当用户的 G 代码模式不是他们认为的那样时，它也会阻止用户开始工作。系统重置始终为用户提供全新、一致的开始。\]

#### `$X` - Kill alarm lock\[终止警报锁\]
Grbl's alarm mode is a state when something has gone critically wrong, such as a hard limit or an abort during a cycle, or if Grbl doesn't know its position. By default, if you have homing enabled and power-up the Arduino, Grbl enters the alarm state, because it does not know its position. The alarm mode will lock all G-code commands until the `$H` homing cycle has been performed. Or if a user needs to override the alarm lock to move their axes off their limit switches, for example, `$X` kill alarm lock will override the locks and allow G-code functions to work again.

\[Grbl 的警报模式是出现严重错误时的状态，例如硬限制或循环中的中止，或者 Grbl 不知道其位置。默认情况下，如果您启用了归位并为 Arduino 加电，Grbl 会进入警报状态，因为它不知道自己的位置。报警模式将锁定所有 G 代码命令，直到执行`$H`归位循环。或者，如果用户需要覆盖警报锁定以将他们的轴移出限位开关，例如，`$X`终止警报锁定将覆盖锁定并允许 G 代码功能再次工作。\]

But, tread carefully!! This should only be used in emergency situations. The position has likely been lost, and Grbl may not be where you think it is. So, it's advised to use G91 incremental mode to make short moves. Then, perform a homing cycle or reset immediately afterwards.

\[但是，请谨慎行事！！这应该只在紧急情况下使用。位置可能已经丢失，Grbl 可能不在您认为的位置。因此，建议使用 G91 增量方式进行短距离移动。然后，执行归位循环或之后立即重置。\]

As noted earlier, startup lines do not execute after a `$X` command. Always reset when you have cleared the alarm and fixed the scenario that caused it. When Grbl resets to idle, the startup lines will then run as normal.

\[如前所述，启动行不会在`$X`命令之后执行。清除警报并修复导致警报的场景后，请始终重置。当 Grbl 重置为空闲时，启动线将正常运行。\]
#### `$H` - Run homing cycle\[`$H`-运行归位循环\]
This command is the only way to perform the homing cycle in Grbl. Some other motion controllers designate a special G-code command to run a homing cycle, but this is incorrect according to the G-code standards. Homing is a completely separate command handled by the controller.

\[此命令是在 Grbl 中执行归位循环的唯一方法。其他一些运动控制器指定一个特殊的 G 代码命令来运行归位循环，但根据 G 代码标准，这是不正确的。归位是由控制器处理的完全独立的命令。\]

TIP: After running a homing cycle, rather jogging manually all the time to a position in the middle of your workspace volume. You can set a G28 or G30 pre-defined position to be your post-homing position, closer to where you'll be machining. To set these, you'll first need to jog your machine to where you would want it to move to after homing. Type G28.1 (or G30.1) to have Grbl store that position. So then after '$H' homing, you could just enter 'G28' (or 'G30') and it'll move there auto-magically. In general, I would just move the XY axis to the center and leave the Z-axis up. This ensures that there isn't a chance the tool in the spindle will interfere and that it doesn't catch on anything.

\[提示：在运行一个归位循环后，而不是一直手动慢跑到工作区体积中间的位置。您可以将 G28 或 G30 预定义位置设置为您的归位后位置，更靠近您将要加工的位置。要设置这些，您首先需要将您的机器慢跑到您希望它在归位后移动到的位置。键入 G28.1（或 G30.1）让 Grbl 存储该位置。因此，在`$H`归位后，您只需输入`G28`（或`G30`），它就会自动神奇地移动到那里。一般来说，我只会将 XY 轴移动到中心，而让 Z 轴向上。这可确保主轴中的刀具不会发生干涉，并且不会卡住任何东西。\]
#### `$J=line` - Run jogging motion\[`$J=直线`-跑步慢跑运动\]

New to Grbl v1.1, this command will execute a special jogging motion. There are three main differences between a jogging motion and a motion commanded by a g-code line. 

\[Grbl v1.1 的新功能，此命令将执行特殊的点动运动。慢跑运动和由 g 代码线命令的运动之间存在三个主要区别。\]

- Like normal g-code commands, several jog motions may be queued into the planner buffer, but the jogging can be easily canceled by a jog-cancel or feed-hold real-time command. Grbl will immediately hold the current jog and then automatically purge the buffers of any remaining commands. 
- \[与普通的 g 代码命令一样，多个点动运动可能会排队进入计划缓冲区，但点动可以通过点动取消或进给保持实时命令轻松取消。Grbl 将立即保持当前的点动，然后自动清除任何剩余命令的缓冲区。\]
- Jog commands are completely independent of the g-code parser state. It will not change any modes like `G91` incremental distance mode. So, you no longer have to make sure that you change it back to `G90` absolute distance mode afterwards. This helps reduce the chance of starting with the wrong g-code modes enabled.
- \[慢跑命令完全独立于 g 代码解析器状态。它不会改变任何模式，如`G91`增量距离模式。因此，您不再需要确保之后将其更改回`G90`绝对距离模式。这有助于减少以错误的 g 代码模式启动的机会。\]
- If soft-limits are enabled, any jog command that exceeds a soft-limit will simply return an error. It will not throw an alarm as it would with a normal g-code command. This allows for a much more enjoyable and fluid GUI or joystick interaction.
- \[如果启用了软限制，则任何超过软限制的点动命令都将简单地返回错误。它不会像使用普通的 g 代码命令那样发出警报。这允许更加愉快和流畅的 GUI 或操纵杆交互。\]

Executing a jog requires a specific command structure, as described below:

\[执行点动需要特定的命令结构，如下所述：\]
 - The first three characters must be `$J=` to indicate the jog.
 - \[前三个字符必须是 `$J=` 以指示点动。\]
 - The jog command follows immediate after the `=` and works like a normal G1 command.
 - \[点动命令紧跟在`=`之后，就像普通的 G1 命令一样工作。\]
 - Feed rate is only interpreted in G94 units per minute. A prior G93 state is ignored during jog.
 - \[进给率仅以每分钟 G94 单位解释。在点动期间忽略先前的 G93 状态。\]
 - Required words:
 - \[必填词：\]
   - XYZ: One or more axis words with target value.
   - \[XYZ：一个或多个具有目标值的轴字。\]
   - F - Feed rate value. NOTE: Each jog requires this value and is not treated as modal.
   - \[F - 进给率值。注意：每个点动都需要此值，并且不被视为模态。\]
 - Optional words: Jog executes based on current G20/G21 and G90/G91 g-code parser state. If one of the following optional words is passed, that state is overridden for one command only.
 - \[可选字：Jog 根据当前 G20/G21 和 G90/G91 g 代码解析器状态执行。如果传递了以下可选词之一，则仅针对一个命令覆盖该状态\]
   - G20 or G21 - Inch and millimeter mode
   - \[G20 或 G21 - 英寸和毫米模式\]
   - G90 or G91 - Absolute and incremental distances
   - \[G90 或 G91 - 绝对距离和增量距离\]
   - G53 - Move in machine coordinates
   - \[G53 - 在机器坐标中移动\]
 - All other g-codes, m-codes, and value words are not accepted in the jog command.
 - \[jog 命令不接受所有其他 g 代码、m 代码和值字。\]
 - Spaces and comments are allowed in the command. These are removed by the pre-parser.
 - \[命令中允许使用空格和注释。这些由预解析器删除。\]
 - Example: G21 and G90 are active modal states prior to jogging. These are sequential commands.
 -\[示例：G21 和 G90 是点动前的有效模态状态。这些是顺序命令。\]
    - `$J=X10.0 Y-1.5 F100` will move to X=10.0mm and Y=-1.5mm in work coordinate frame (WPos) at a feed rate of 100.
    - \[`$J=X10.0 Y-1.5 F100` 将以 100 的进给速度在工作坐标系 (WPos) 中移动到 X=10.0mm 和 Y=-1.5mm。\]
    - `$J=G91 G20 X0.5 F10` will move +0.5 inches (12.7mm) to X=22.7mm (WPos) at a feed rate of 10. Note that G91 and G20 are only applied to this jog command.
    - \[`$J=G91 G20 X0.5 F10` 将以 10 的进给速率移动 +0.5 英寸（12.7 毫米）到 X=22.7 毫米（WPos）。注意 G91 和 G20 仅适用于该点动命令。\]
    - `$J=G53 Y5.0 F10` will move the machine to Y=5.0mm in the machine coordinate frame (MPos) at a feed rate of 10. If the work coordinate offset for the y-axis is 2.0mm, then Y is 3.0mm in (WPos).
    - \[`$J=G53 Y5.0 F10` 将以 10 的进给速率将机器移动到机器坐标系 (MPos) 中的 Y=5.0mm。如果 y 轴的工件坐标偏移为 2.0mm，则 Y 为 (WPos) 中的 3.0mm。\]

Jog commands behave almost identically to normal g-code streaming. Every jog command will
return an `ok` when the jogging motion has been parsed and is setup for execution. If a
command is not valid or exceeds a soft-limit, Grbl will return an `error:`. Multiple jogging commands may be queued in sequence.

\[Jog 命令的行为几乎与普通的 g 代码流相同。当点动运动被解析并准备执行时，每个点动命令都将返回一个`ok`。如果命令无效或超出软限制，Grbl 将返回`error：`。多个点动命令可以按顺序排队。\]

NOTE: See [jogging documentation](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Jogging) for details on using this command to create a low-latency joystick or rotary dial interface.

\[注意：有关使用此命令创建低延迟操纵杆或旋转拨号界面的详细信息，请参阅[慢跑文档 ](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Jogging)。\]

#### `$RST=$`, `$RST=#`, and `$RST=*`- Restore Grbl settings and data to defaults
#### [`$RST=$`, `$RST=#`, 和 `$RST=*`-将 Grbl 设置和数据恢复为默认值\]
These commands are not listed in the main Grbl `$` help message, but are available to allow users to restore parts of or all of Grbl's EEPROM data. Note: Grbl will automatically reset after executing one of these commands to ensure the system is initialized correctly.

\[这些命令未列在主 Grbl`$`帮助消息中，但可用于允许用户恢复部分或全部 Grbl 的 EEPROM 数据。注意：执行这些命令之一后，Grbl 将自动重置，以确保系统正确初始化。\]

- `$RST=$` : Erases and restores the `$$` Grbl settings back to defaults, which is defined by the default settings file used when compiling Grbl. Often OEMs will build their Grbl firmwares with their machine-specific recommended settings. This provides users and OEMs a quick way to get back to square-one, if something went awry or if a user wants to start over.
- \[`$RST=$`：擦除$$Grbl 设置并将其恢复为默认值，这是由编译 Grbl 时使用的默认设置文件定义的。通常 OEM 会使用他们特定于机器的推荐设置来构建他们的 Grbl 固件。如果出现问题或用户想重新开始，这为用户和原始设备制造商提供了一种快速恢复原状的方法。\]
- `$RST=#` : Erases and zeros all G54-G59 work coordinate offsets and G28/30 positions stored in EEPROM. These are generally the values seen in the `$#` parameters printout. This provides an easy way to clear these without having to do it manually for each set with a `G20 L2/20` or `G28.1/30.1` command.
- \[`$RST=#`：擦除并归零所有存储在 EEPROM 中的 G54-G59 工件坐标偏移和 G28/30 位置。这些通常是在$#参数打印输出中看到的值。这提供了一种简单的方法来清除这些，而无需使用G20 L2/20或G28.1/30.1命令为每个集合手动执行此操作。\]
- `$RST=*` : This clears and restores all of the EEPROM data used by Grbl. This includes `$$` settings, `$#` parameters, `$N` startup lines, and `$I` build info string. Note that this doesn't wipe the entire EEPROM, only the data areas Grbl uses. To do a complete wipe, please use the Arduino IDE's EEPROM clear example project.
- \[`$RST=*`：这会清除并恢复 Grbl 使用的所有 EEPROM 数据。这包括$$设置、$#参数、$N启动行和$I构建信息字符串。请注意，这不会擦除整个 EEPROM，只会擦除 Grbl 使用的数据区域。要进行完整擦除，请使用 Arduino IDE 的 EEPROM 清除示例项目。\]

NOTE: Some OEMs may restrict some or all of these commands to prevent certain data they use from being wiped. 

\[注意：某些 OEM 可能会限制部分或全部这些命令，以防止擦除他们使用的某些数据。\]

#### `$SLP` - Enable Sleep Mode\[`$SLP` - 启用睡眠模式\]

This command will place Grbl into a de-powered sleep state, shutting down the spindle, coolant, and stepper enable pins and block any commands. It may only be exited by a soft-reset or power-cycle. Once re-initialized, Grbl will automatically enter an ALARM state, because it's not sure where it is due to the steppers being disabled.

\[此命令会将 Grbl 置于断电睡眠状态，关闭主轴、冷却剂和步进器启用引脚并阻止任何命令。它只能通过软复位或电源循环退出。一旦重新初始化，Grbl 将自动进入 ALARM 状态，因为由于步进器被禁用，它不确定它在哪里。\]

This feature is useful if you need to automatically de-power everything at the end of a job by adding this command at the end of your g-code program, BUT, it is highly recommended that you add commands to first move your machine to a safe parking location prior to this sleep command. It also should be emphasized that you should have a reliable CNC machine that will disable everything when its supposed to, like your spindle. Grbl is not responsible for any damage it may cause. It's never a good idea to leave your machine unattended. So, use this command with the utmost caution!

\[如果您需要通过在 g 代码程序的末尾添加此命令来在作业结束时自动关闭所有内容，则此功能非常有用，但是，强烈建议您添加命令以首先将您的机器移动到在此睡眠命令之前的安全停车位置。还应该强调的是，您应该拥有一台可靠的 CNC 机器，它可以在应该禁用的时候禁用一切，例如您的主轴。Grbl 对其可能造成的任何损害概不负责。让您的机器无人看管从来都不是一个好主意。因此，请务必谨慎使用此命令！\]

***

## Grbl v1.1 Realtime commands \[Grbl v1.1 实时命令\]

Realtime commands are single control characters that may be sent to Grbl to command and perform an action in real-time. This means that they can be sent at anytime, anywhere, and Grbl will immediately respond, regardless of what it is doing at the time. These commands include a reset, feed hold, resume, status report query, and overrides (in v1.1).

  \[实时命令是单个控制字符，可以发送到 Grbl 以实时命令和执行操作。这意味着它们可以随时随地发送，Grbl 会立即响应，不管它当时在做什么。这些命令包括重置、馈送保持、恢复、状态报告查询和覆盖（在 v1.1 中）。\]

A realtime command\[实时命令\]:

- Will execute within tens of milliseconds.
- \[将在几十毫秒内执行。\]
- Is a single character that may be sent to Grbl at any time.
- \[是可以随时发送到 Grbl 的单个字符。\]
- Does not require a line feed or carriage return after them.
- \[在它们之后不需要换行或回零。\]
- Is not considered a part of the streaming protocol. 
- \[不被视为流协议的一部分。\]
- Are intercepted when they are received and never placed in a buffer to be parsed by Grbl.
- \[当它们被接收时被拦截，并且永远不会被放置在一个缓冲区中以供 Grbl 解析。\]
- Will ignore multiple commands until it has executed the first received command.
- \[将忽略多个命令，直到它执行了第一个接收到的命令。\]
- May be tied to an input pin and may be operated with a button or switch.
- \[可以连接到输入引脚，可以通过按钮或开关进行操作。。\]
- Actions depends on state or what Grbl is doing. It may not do anything.
- \[动作取决于状态或 Grbl 正在做什么。它可能什么也做不了。\]
- Descriptions explain how they work and what to expect.
- \[描述解释了它们的工作原理和预期。\]

#### ASCII Realtime Command Descriptions\[ASCII实时命令说明\]
Four realtime commands are type-able by users on a keyboard and shown in the `$` Grbl help message. These realtime command characters control some of Grbl's basic functions.

\[用户可以在键盘上键入四个实时命令，并显示在`$`Grbl`帮助消息中。这些实时命令字符控制Grbl的一些基本功能。\]

- `0x18` (ctrl-x) : Soft-Reset\[`0x18`（ctrl-x）：软重置\]
  - Immediately halts and safely resets Grbl without a power-cycle.
  - \[ 立即停止并安全重置Grbl，无需电源循环。\]
  - Accepts and executes this command at any time.
  - \[ 随时接受并执行此命令。\]
  - If reset while in motion, Grbl will throw an alarm to indicate position may be lost from the motion halt.
  - \[ 如果在运动中复位，Grbl将发出警报，指示位置可能因运动停止而丢失。\]
  - If reset while not in motion, position is retained and re-homing is not required.
  - \[ 如果在不运动时复位，则位置保持不变，无需重新归位。\]
  - An input pin is available to connect a button or switch.
  - \[ 输入引脚可用于连接按钮或开关。\]


- `?` : Status Report Query\[`？`：状态报告查询\]
  - Immediately generates and sends back runtime data with a status report.
  - \[立即生成并发回带有状态报告的运行时数据。\]
  - Accepts and executes this command at any time, except during a homing cycle and when critical alarm (hard/soft limit error) is thrown.
  - \[立即生成并发回带有状态报告的运行时数据。\]


- `~` : Cycle Start / Resume\[`~`：循环开始/恢复\]

  - Resumes a feed hold, a safety door/parking state when the door is closed, and the M0 program pause states.
  - \[门关闭时恢复进纸保持、安全门/停车状态以及M0程序暂停状态。\]
  - Command is otherwise ignored.
  - \[否则将忽略该命令。\]
  - If the parking compile-time option is enabled and the safety door state is ready to resume, Grbl will re-enable the spindle and coolant, move back into position, and then resume.
  - \[如果启用了驻车编译时选项且安全门状态准备恢复，Grbl将重新启用主轴和冷却液，移回原位，然后恢复。\]
  - An input pin is available to connect a button or switch.
  - \[输入引脚可用于连接按钮或开关。\]


- `!` : Feed Hold\[`!` :进料舱\]

  - Places Grbl into a suspend or HOLD state. If in motion, the machine will decelerate to a stop and then be suspended.
  - \[将Grbl置于挂起或保持状态。如果处于运动状态，机器将减速至停止，然后暂停。\]
  - Command executes when Grbl is in an IDLE, RUN, or JOG state. It is otherwise ignored.
  - \[命令在Grbl处于空闲、运行或JOG状态时执行。否则就会被忽略。\]
  - If jogging, a feed hold will cancel the jog motion and flush all remaining jog motions in the planner buffer. The state will return from JOG to IDLE or DOOR, if was detected as ajar during the active hold.
  - \[如果点动，进给保持将取消点动运动，并刷新planner缓冲区中所有剩余的点动运动。如果在激活保持期间检测到微开，则状态将从JOG返回IDLE或DOOR。\]
  - By machine control definition, a feed hold does not disable the spindle or coolant. Only motion.
  - \[根据机器控制定义，进给保持不会禁用主轴或冷却液。只有动议。\]
  - An input pin is available to connect a button or switch.
  - \[输入引脚可用于连接按钮或开关。\]


#### Extended-ASCII Realtime Command Descriptions\[扩展ASCII实时命令说明\]

Grbl v1.1 installed more than a dozen new realtime commands to control feed, rapid, and spindle overrides. To help prevent users from inadvertently altering overrides with a keystroke and allow for more commands later on, all of the new control characters have been moved to the extended ASCII character set. These are not easily type-able on a keyboard, but, depending on the OS, they may be entered using specific keystroke and code. GUI developers will need to be able to send extended ASCII characters, values `128 (0x80)` to `255 (0xFF)`, to Grbl to take advantage of these new features.

\[GRBLV1.1安装了十多个新的实时命令来控制进给、快速和主轴超控。为了帮助防止用户无意中通过击键更改覆盖，并允许以后执行更多命令，所有新的控制字符都已移动到扩展ASCII字符集。在键盘上很难输入这些内容，但根据操作系统的不同，可以使用特定的按键和代码输入这些内容。GUI开发人员需要能够向Grbl发送扩展的ASCII字符，即值为`128（0x80）`到`255（0xFF）`，以利用这些新功能。\]

- `0x84` : Safety Door \[`0x84`：安全门\]

  - Although typically connected to an input pin to detect the opening of a safety door, this command allows a GUI to enact the safety door behavior with this command.
  - \[虽然通常连接到输入引脚以检测安全门的打开，但此命令允许GUI使用此命令设定安全门行为。\]
  - Immediately suspends into a DOOR state and disables the spindle and coolant. If in motion, the machine will decelerate to a stop and then be suspended.
  - \[立即暂停进入车门状态，并禁用主轴和冷却液。如果处于运动状态，机器将减速至停止，然后暂停。\]
  - If executed during homing, Grbl will instead halt motion and throw a homing alarm.
  - \[如果在归零报警期间执行归零，则会抛出。\]
  - If already in a suspend state or HOLD, the DOOR state supersedes it.
  - \[如果已处于挂起状态或保持状态，则门状态将取代它\]
  - If the parking compile-time option is enabled, Grbl will park the spindle to a specified location.
  - \[如果启用了驻车编译时选项，Grbl将主轴停在指定位置。\]
  - Command executes when Grbl is in an IDLE, HOLD, RUN, HOMING, or JOG state. It is otherwise ignored.
  - \[命令在Grbl处于空闲、保持、运行、归位或JOG状态时执行。否则就会被忽略。\]
  - If jogging, a safety door will cancel the jog and all queued motions in the planner buffer. When the safety door is closed and resumed, Grbl will return to an IDLE state.
  - \[如果点动，安全门将取消点动和planner缓冲区中的所有排队运动。当安全门关闭并恢复时，Grbl将返回空闲状态。\]
  - An input pin is available to connect a button or switch, if enabled with a compile-time option.
  - \[如果使用编译时选项启用，则可以使用输入引脚连接按钮或开关。\]
  - Some builds of Grbl v0.9 used the `@` character for this command, but it was undocumented. Moved to extended-ASCII to prevent accidental commanding.
  - \[Grbl v0.9的某些版本对此命令使用了`@`字符，但没有记录在案。移动到扩展ASCII以防止意外命令。\]


- `0x85` : Jog Cancel \[`0x85`:点动取消\]

  - Immediately cancels the current jog state by a feed hold and automatically flushing any remaining jog commands in the buffer.
  - \[通过进给保持立即取消当前点动状态，并自动刷新缓冲区中的任何剩余点动命令。\]
  - Command is ignored, if not in a JOG state or if jog cancel is already invoked and in-process.
  - \[如果未处于点动状态，或者已经调用并正在执行点动取消，则忽略该命令。\]
  - Grbl will return to the IDLE state or the DOOR state, if the safety door was detected as ajar during the cancel.
  - \[如果在取消过程中检测到安全门未关，Grbl将返回怠速状态或车门状态。\]
  

- Feed Overrides \[馈送覆盖\]

  - Immediately alters the feed override value. An active feed motion is altered within tens of milliseconds.
  - \[立即更改馈送覆盖值。主动进给运动在数十毫秒内改变。\]
  - Does not alter rapid rates, which include G0, G28, and G30, or jog motions.
  - \[不改变快速速率，包括G0、G28和G30或点动运动。\]
  - Feed override value can not be 10% or greater than 200%.
  - \[馈送覆盖值不能为10%或大于200%。\]
  - If feed override value does not change, the command is ignored.
  - \[如果馈送覆盖值未更改，则忽略该命令。\]
  - Feed override range and increments may be changed in config.h.
  - \[进给覆盖范围和增量可在config.h中更改。\]
  - The commands are:
  - \[这些命令是：\]
    - `0x90` : Set 100% of programmed rate.
    - \[`0x90`：设置编程速率的100%。\]
    - `0x91` : Increase 10%
    - \[`0x91`：增加10%\]
    - `0x92` : Decrease 10%
    - \[`0x92`：减少10%\]
    - `0x93` : Increase 1%
    - \[`0x93`：增加1%\]
    - `0x94` : Decrease 1%
    - \[`0x94`：减少1%\]


- Rapid Overrides \[快速覆盖\]

  - Immediately alters the rapid override value. An active rapid motion is altered within tens of milliseconds.
  - \[立即改变快速超越值。活动的快速运动在数十毫秒内改变。\]
  - Only effects rapid motions, which include G0, G28, and G30.
  - \[仅影响快速运动，包括G0、G28和G30。\]
  - If rapid override value does not change, the command is ignored.
  - \[如果`快速覆盖`值未更改，则忽略该命令。\]
  - Rapid override set values may be changed in config.h.
  - \[快速覆盖设置值可以在config.h中更改。\]
  - The commands are:
  - \[这些命令是：\]
    - `0x95` : Set to 100% full rapid rate.
    - \[`0x95`：设置为100%完全快速速率。\]
    - `0x96` : Set to 50% of rapid rate.
    - \[`0x96`：设置为快速速率的50%。\]
    - `0x97` : Set to 25% of rapid rate.
    - \[`0x97`：设置为快速速率的25%。\]


- Spindle Speed Overrides \[主轴速度超控\]

  - Immediately alters the spindle speed override value. An active spindle speed is altered within tens of milliseconds.
  - \[立即改变主轴速度超越值。激活的主轴转速在数十毫秒内改变。\]
  - Override values may be changed at any time, regardless of if the spindle is enabled or disabled.
  - \[超越值可以随时更改，无论主轴是启用还是禁用。\]
  - Spindle override value can not be 10% or greater than 200%
  - \[主轴超控值不能为10%或大于200%\]
  - If spindle override value does not change, the command is ignored.
  - \[如果主轴超控值未更改，则忽略该命令。\]
  - Spindle override range and increments may be altered in config.h.
  - \[主轴超控范围和增量可在config.h中更改。\]
  - The commands are:
  - \[这些命令是：\]
    - `0x99` : Set 100% of programmed spindle speed
    - \[`0x99`：设置编程主轴转速的100%\]
    - `0x9A` : Increase 10%
    - \[`0x9A`：增加10%\]
    - `0x9B` : Decrease 10%
    - \[`0x9B`：减少10%\]
    - `0x9C` : Increase 1%
    - \[`0x9C`：增加1%\]
    - `0x9D` : Decrease 1%
    - \[`0x9D`：减少1%\]


- `0x9E` : Toggle Spindle Stop \[`0x9E`：切换主轴止动块\]

  - Toggles spindle enable or disable state immediately, but only while in the HOLD state.
  - \[立即切换主轴启用或禁用状态，但仅在保持状态下切换。\]
  - The command is otherwise ignored, especially while in motion. This prevents accidental disabling during a job that can either destroy the part/machine or cause personal injury. Industrial machines handle the spindle stop override similarly.
  - \[否则将忽略该命令，尤其是在运动时。这可防止在工作期间意外禁用，从而损坏零件/机器或造成人身伤害。工业机器处理主轴停止超越类似。\]
  - When motion restarts via cycle start, the last spindle state will be restored and wait 4.0 seconds (configurable) before resuming the tool path. This ensures the user doesn't forget to turn it back on.
  - \[通过循环启动重新启动运动时，最后一个主轴状态将恢复，并等待4.0秒（可配置），然后再恢复刀具路径。这确保了用户不会忘记重新打开它。\]
  - While disabled, spindle speed override values may still be altered and will be in effect once the spindle is re-enabled.
  - \[禁用时，主轴速度超控值仍可更改，并在主轴重新启用后生效。\]
  - If a safety door is opened, the DOOR state will supersede the spindle stop override, where it will manage the spindle re-energizing itself upon closing the door and resuming. The prior spindle stop override state is cleared and reset.
  - \[如果安全门打开，门状态将取代主轴停止超控，在关闭门并恢复时，它将管理主轴重新通电。先前的主轴停止超越状态被清除并复位。\]


- `0xA0` : Toggle Flood Coolant \[`0xA0`：切换溢流冷却液\]

  - Toggles flood coolant state and output pin until the next toggle or g-code command alters it.
  - \[切换溢流冷却液状态和输出引脚，直到下一个切换或g-code命令改变它。\]
  - May be commanded at any time while in IDLE, RUN, or HOLD states. It is otherwise ignored.
  - \[可在怠速、运行或保持状态下随时发出指令。否则就会被忽略。\]
  - This override directly changes the coolant modal state in the g-code parser. Grbl will continue to operate normally like it received and executed an `M8` or `M9` g-code command.
  - \[此覆盖直接更改g代码解析器中的冷却剂模式状态。Grbl将继续正常运行，就像它接收并执行了`M8`或`M9`g-code命令一样。\]
  - When `$G` g-code parser state is queried, the toggle override change will be reflected by an `M8` enabled or disabled with an `M9` or not appearing when `M7` is present.
  - \[当查询`$G`G-code解析器状态时，切换覆盖更改将通过启用或禁用带有`M9`的`M8`来反映，或者在出现`M7`时不显示。\]


- `0xA1` : Toggle Mist Coolant \[`0xA1`：切换喷雾冷却液\]

  - Enabled by `ENABLE_M7` compile-time option. Default is disabled.
  - \[由`ENABLE_M7`编译时选项启用。默认设置为禁用。\]
  - Toggles mist coolant state and output pin until the next toggle or g-code command alters it.
  - \[切换喷雾冷却液状态和输出引脚，直到下一个切换或g-code命令改变它。\]
  - May be commanded at any time while in IDLE, RUN, or HOLD states. It is otherwise ignored.
  - \[可在怠速、运行或保持状态下随时发出指令。否则就会被忽略。\]
  - This override directly changes the coolant modal state in the g-code parser. Grbl will continue to operate normally like it received and executed an `M7` or `M9` g-code command.
  - \[此覆盖直接更改g代码解析器中的冷却剂模式状态。Grbl将继续正常运行，就像它接收并执行`M7`或`M9`g-code命令一样。\]
  - When `$G` g-code parser state is queried, the toggle override change will be reflected by an `M7` enabled or disabled with an `M9` or not appearing when `M8` is present.
  - \[当查询`$G`G-code解析器状态时，切换覆盖更改将通过启用或禁用`M7`来反映，并带有`M9`或在出现`M8`时不出现。\]