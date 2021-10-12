***
_Welcome to the Grbl wiki! Please feel free to modify these pages to help keep Grbl up-to-date !_

 \[_欢迎来到Grbl维基！请随意修改这些页面，以帮助Grbl保持最新_\]

![Grbl Logo \[GRBL图标\]](https://github.com/gnea/gnea-Media/blob/master/Grbl%20Logo/Grbl%20Logo%20250px.png?raw=true)

### About Grbl \[关于Grbl\]
Grbl is a free, open source, high performance software for controlling the motion of machines that move, that make things, or that make things move, and will run on a straight Arduino. If the maker movement was an industry, Grbl would be the industry standard.

 \[Grbl是一款免费的、开源的、高性能的软件，用于控制移动、制造或移动机器的运动，并在直线上运行。如果创客运动是一个行业，Grbl将是行业标准。\]

Most open source 3D printers have Grbl in their hearts. It has been adapted for use in hundreds of projects including laser cutters, automatic hand writers, hole drillers, graffiti painters and oddball drawing machines. Due to its performance, simplicity and frugal hardware requirements Grbl has grown into a little open source phenomenon.

 \大多数开源3D打印机心中都有Grbl。它已被改造用于数百个项目，包括激光切割机、自动手写笔、钻孔机、涂鸦画家和古怪绘图机。由于它的性能、简单性和节省的硬件需求，Grbl已经发展成为一种开源现象。[\]

In 2009, **Simen Svale Skogsrud** (**http://bengler.no/grbl**) graced the open-source community by writing and releasing the early versions of Grbl to everyone ([inspired by the Arduino GCode Interpreter by Mike Ellery](http://www.contraptor.org/grbl-gcode-interpreter)). Since 2011, Grbl is pushing ahead as a community-driven open-source project under the pragmatic leadership of **Sungeun "Sonny" Jeon Ph.D.** (**@chamnit**).

 \[2009年，**Simen Svale Skogsrud**(**http://bengler.no/grbl**)通过编写Grbl的早期版本并将其发布给每个人（受Mike Ellery的Arduino GCode解释器启发），为开源社区增光添彩(http://www.contraptor.org/grbl-gcode-interpreter)). 自2011年以来，Grbl在**Sungeun“Sonny”Jeon Ph.D.**（***chamnit***)的务实领导下，作为一个社区驱动的开源项目，正在向前推进。\]

-----
### Official Supporters of the Grbl CNC Project \[Grbl CNC项目的官方支持者\]

![Official Supporters \[官方支持者\]](https://github.com/gnea/gnea-Media/blob/master/Contributors.png?raw=true)

-----

### Who should use Grbl \[谁应该使用Grbl\]

Makers who do milling or laser cutting and need a nice, simple controller for their system that will run on   the ubiquitous Arduino Uno. People who loathe to clutter their space with legacy PC-towers just for the parallel-port. Tinkerers who need a controller written in tidy, modular C as a basis for their project.

 \[从事铣削或激光切割的制造商需要一个漂亮、简单的控制器来控制他们的系统，该系统将在无处不在的Arduino Uno上运行。那些不愿意为了并行端口而用传统的PC塔架把他们的空间弄得杂乱无章的人。需要一个整洁、模块化C语言编写的控制器作为项目基础的修补者。\]

### Nice features \[好的特性\]

Grbl is great for light duty production. We use it for all our milling, running it from our laptops or Raspberry Pis using superb GUIs written for Grbl to stream G-code jobs. Grbl is written in highly optimized C utilizing all the clever features of the Arduino's Atmega328p chips to achieve precise timing and asynchronous operation. It is able to maintain more than **30kHz** step rate and delivers a clean, jitter free stream of control pulses.

 \[Grbl适用于轻型生产。我们将其用于所有的铣削，使用为Grbl编写的优秀GUI从笔记本电脑或Raspberry Pis上运行，以流式处理G代码作业。Grbl采用高度优化的C语言编写，利用Arduino的Atmega328p芯片的所有巧妙功能，实现精确定时和异步操作。它能够保持超过**30kHz**的步进速率，并提供干净、无抖动的控制脉冲流。\]

Grbl is for three axis machines. No rotation axes (yet) – just X, Y, and Z.

 \[Grbl适用于三轴机器。没有旋转轴（目前）-只有X、Y和Z。\]
 
The G-code interpreter implements a subset of the LinuxCNC standard and is supported by most CAM-tools with no issues. For descriptions of these G-codes, see LinuxCNC's superb documentation for their G-code descriptions, [(G-code Quick Reference)](http://linuxcnc.org/docs/html/gcode.html), and the [Shapeoko wiki](http://www.shapeoko.com/wiki/index.php/G-Code) which attempts to list all codes supported by Grbl with appropriate commentary. Note that there are only a handful of deviations from the written G-code standard listed below. If you notice any other discrepancies, please let use know!

 \[G代码解释器实现了LinuxCNC标准的一个子集，大多数CAM工具都支持它，没有任何问题。有关这些G代码的说明，请参阅LinuxCNC的超级文档，了解其G代码说明，[（G代码快速参考）](http://linuxcnc.org/docs/html/gcode.html)，以及[Shapeoko wiki](http://www.shapeoko.com/wiki/index.php/G-Code)它试图列出Grbl支持的所有代码以及适当的注释。请注意，与下面列出的书面G代码标准仅有少量偏差。如果您发现任何其他差异，请让我们知道！\]

- Multiple full circle arcs with G2 and G3 arcs with a P word is not supported.
- \[-不支持带有G2和带有P字的G3圆弧的多个整圆圆弧。\]
- Laser mode alters the operation of M3, M4, and spindle speed S word changes. See the [Laser Mode](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Laser-Mode) page for details.
- \[-激光模式改变M3、M4和主轴速度的操作。参见[激光模式](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Laser-Mode)详情请参阅第页。\]
- Grbl-specific parking motion override control with an M56 command, where `M56 P0` temporarily disables parking motions and `M56`/`M56 Px` with `x` greater than zero re-enables them.
- \[-带有M56命令的Grbl特定停车运动超越控制，其中“M56 P0”暂时禁用停车运动，“M56`/`M56 Px`，`x`大于零时重新启用它们。\]

Supported G-Codes in **v1.1**  \[v1.1版本支撑的G-codes\]
 * **G0, G1:** _Linear Motions_ \[_直线运动_ \]
 * **G2, G3:** _Arc and Helical Motions_  \[_圆弧和螺旋运动_ \]
 * **G4:** _Dwell_  \[_停止_ \]
 * **G10 L2, G10 L20:** _Set Work Coordinate Offsets_ \[_设置工作坐标偏移_ \]
 * **G17, G18, G19:** _Plane Selection_  \[_平面选择_ \]
 * **G20, G21:** _Units_  \[_单位_ \]
 * **G28, G30:** _Go to Pre-Defined Position_  \[_转到预定义位置_ \]
 * **G28.1, G30.1:** _Set Pre-Defined Position_ \[_设置预定义的位置_ \]
 * **G38.2:** _Probing_  \[_探查_ \]
 * **G38.3, G38.4, G38.5:** _Probing_  \[_探查_ \]
 * **G40:** _Cutter Radius Compensation Modes OFF (Only)_ \[_刀具半径补偿模式关闭（仅限）_ \]
 * **G43.1, G49:** _Dynamic Tool Length Offsets_ \[_动态刀具长度偏移_ \]
 * **G53:** _Move in Absolute Coordinates_ \[_以绝对坐标移动_ \]
 * **G54, G55, G56, G57, G58, G59:** _Work Coordinate Systems_ \[_工作坐标系_ \]
 * **G61:** _Path Control Modes_ \[_路径控制模式_ \]
 * **G80:** _Motion Mode Cancel_  \[_运动模式取消_ \]
 * **G90, G91:** _Distance Modes_  \[_距离模式_ \]
 * **G91.1:** _Arc IJK Distance Modes_ \[_弧IJK距离模式_ \]
 * **G92:** _Coordinate Offset_  \[_坐标偏移_ \]
 * **G92.1:** _Clear Coordinate System Offsets_  \[_清除坐标系偏移_ \]
 * **G93, G94:** _Feedrate Modes_  \[_进给速度模式_ \]
 * **M0, M2, M30:** _Program Pause and End_  \[_程序暂停和结束_\]
 * **M3, M4, M5:** _Spindle Control_  \[_主轴控制_ \]
 * **M7*** **, M8, M9:** _Coolant Control_  \[_冷却剂控制_ \]
 * **M56*** **:** _Parking Motion Override Control_ \[_停车运动超越控制_ \]

(*) denotes commands not enabled in config.h by default. \[(*)表示默认情况下未在config.h中启用的命令\]

### Acceleration management \[加速管理\]

In the early days, Arduino-based CNC controllers did not have acceleration planning and couldn't run at full speed without some kind of easing. Grbl’s constant acceleration-management with look ahead planner solved this issue and has been replicated everywhere in the micro controller CNC world, from Marlin to TinyG. Grbl intentionally uses a simpler constant acceleration model, which is more than adequate for home CNC use. Because of this, we were able to invest our time optimizing our planning algorithms and making sure motions are solid and reliable. When the installation of all the feature sets we think are critical are complete and no longer requires us to modify our planner to accommodate them, we intend to research and implement more-advanced motion control algorithms, which are usually reserved for machines only with very high feed rates (i.e. pick-and-place) or in production environments. Lastly, here's a [link](http://onehossshay.wordpress.com/2011/09/24/improving_grbl_cornering_algorithm/) describing the basis of our high speed cornering algorithm so motions ease into the fastest feed rates and brake before sharp corners for fast, yet jerk free operation. 

 \[早期，基于Arduino的CNC控制器没有加速规划，如果没有某种缓解措施，就无法全速运行。Grbl的持续加速管理和前瞻计划解决了这一问题，并在微控制器CNC世界的任何地方得到了复制，从Marlin到TinyG。Grbl有意使用一个更简单的恒加速度模型，这对于家用CNC来说已经足够了。正因为如此，我们才得以投入时间优化规划算法，确保运动稳定可靠。当我们认为至关重要的所有功能集的安装完成，不再需要我们修改规划器以适应它们时，我们打算研究并实施更先进的运动控制算法，这些算法通常只适用于进给率非常高的机器（即拾取和放置）或者在生产环境中。最后，这里有一个[链接](http://onehossshay.wordpress.com/2011/09/24/improving_grbl_cornering_algorithm/)描述我们的高速转弯算法的基础，使运动轻松进入最快的进给速度，并在急转弯前制动，以实现快速但无急动的操作。\]

### Limitations by design \[设计的局限性\]

We have limited G-code-support by design. This keeps the Grbl source code simple, lightweight, and flexible, as we continue to develop, improve, and maintain stability with each new feature. Grbl supports all the common operations encountered in output from CAM-tools, but leave some human G-coders frustrated. No variables, no tool databases, no functions, no canned cycles, no arithmetic and no control structures. Just the basic machine operations and capabilities. Anything more complex, we think interfaces can handle those quite easily and translate them for Grbl.

\[我们的G代码设计支持有限。这使Grbl源代码保持简单、轻量级和灵活性，因为我们将继续开发、改进和维护每个新特性的稳定性。Grbl支持CAM工具输出中遇到的所有常见操作，但使一些G代码编写人员感到沮丧。没有变量，没有工具数据库，没有函数，没有固定循环，没有算法和控制结构。只是基本的机器操作和功能。任何更复杂的东西，我们认为接口都可以很容易地处理它们，并将它们转换为Grbl。\]

### New features in v1.1! \[v1.1版本中的新功能！\]
Another **HUGE** update! This version includes the last remaining priority features that have been long been on the to-do list. Here's a summary of the new changes:

 \[另一个**巨大**更新！此版本包括长期以来一直在待办事项列表上的最后剩余优先权功能。以下是新变化的摘要：\]

* **Real-time Overrides** : Alters the machine running state immediately with feed, rapid, spindle speed, spindle stop, and coolant toggle controls. This awesome new feature is common only on industrial machines, often used to optimize speeds and feeds while a job is running. Most hobby CNCs try to mimic this behavior, but usually have large amounts of lag. Grbl executes overrides in realtime and within tens of milliseconds.

* \[**实时覆盖**：使用进给、快速、主轴速度、主轴停止和冷却液切换控制立即改变机器运行状态。这一令人敬畏的新功能仅在工业机器上常见，通常用于在作业运行时优化速度和进给。大多数爱好CNC试图模仿这种行为，但通常有大量的滞后。Grbl在数十毫秒内实时执行覆盖。\]

* **Jogging Mode** : The new jogging commands are independent of the G-code parser, so that the parser state doesn't get altered and cause a potential crash if not restored properly. Documentation is included on how this works and how it can be used to control your machine via a joystick or rotary dial with a low-latency, satisfying response.

* \[**慢跑模式**：新的Jogging命令独立于G代码解析器，因此解析器状态不会改变，如果没有正确恢复，可能会导致崩溃。文档包括如何工作，以及如何使用它通过操纵杆或旋转拨号控制您的机器，具有低延迟、令人满意的响应。\]

* **Laser Mode** : The new "laser" mode will cause Grbl to move continuously through consecutive G1, G2, and G3 commands with spindle speed changes. When "laser" mode is disabled, Grbl will instead come to a stop to ensure a spindle comes up to speed properly. Spindle speed overrides also work with laser mode so you can tweak the laser power, if you need to during the job. Switch between "laser" mode and "normal" mode via a $ setting.

* \[**激光模式**：新的“激光”模式将使Grbl在主轴转速变化的情况下连续移动G1、G2和G3指令。当“激光”模式被禁用时，Grbl将停止，以确保主轴正常加速。主轴速度超控也适用于激光模式，因此，如果作业期间需要，您可以调整激光功率。通过$设置在“激光”模式和“正常”模式之间切换。\]

* **Dynamic Laser Power Scaling with Speed** : If your machine has low accelerations, this option will automagically scale the laser power based on how fast Grbl is traveling, so you won't have burnt corners when your CNC has to make a turn! Enabled by the `M4` spindle CCW command when laser mode is enabled.

* \[**随速度动态激光功率缩放**：如果您的机器加速度较低，此选项将根据Grbl移动的速度自动缩放激光功率，这样当您的CNC必须转弯时，您就不会有烧焦的拐角！激光模式启用时，由“M4”主轴逆时针命令启用。\]

* **Sleep Mode** : Grbl may now be put to "sleep" via a $SLP command. This will disable everything, including the stepper drivers. Nice to have when you are leaving your machine unattended and want to power down everything automatically. Only a reset exits the sleep state.

* \[**睡眠模式**：现在可以通过$SLP命令将Grbl置于“睡眠”状态。这将禁用一切，包括步进驱动程序。很高兴当你离开你的机器无人看管，并想关闭所有自动电源。只有重置退出睡眠状态。\]

* **Significant Interface Improvements**: Tweaked to increase overall performance, include lots more real-time data, and to simplify maintaining and writing GUIs. Based on direct feedback from multiple GUI developers and bench performance testing. NOTE: GUIs need to specifically update their code to be compatible with v1.1 and later.

* \[**显著的界面改进**：调整以提高整体性能，包括更多的实时数据，并简化GUI的维护和编写。基于多个GUI开发人员的直接反馈和台架性能测试。注意：GUI需要专门更新其代码以与v1.1及更高版本兼容。\]

* **New Status Reports**: To account for the additional override data, status reports have been tweaked to cram more data into it, while still being smaller than before. Documentation is included, outlining how it has been changed.
Improved Error/Alarm Feedback : All Grbl error and alarm messages have been changed to providing a code. Each code is associated with a specific problem, so users will know exactly what is wrong without having to guess. Documentation and an easy to parse CSV is included in the repo.

* \[**新的状态报告**：为了考虑额外的覆盖数据，状态报告经过了调整，将更多的数据塞进其中，同时仍然比以前小。包括文档，概述了它是如何更改的。
改进的错误/警报反馈：所有Grbl错误和警报消息都已更改为提供代码。每个代码都与一个特定的问题相关联，因此用户无需猜测就可以准确地知道错误所在。回购协议中包含文档和易于解析的CSV。\]

* **Extended-ASCII realtime commands** : All overrides and future real-time commands are defined in the extended-ASCII character space. Unfortunately not easily type-able on a keyboard, but helps prevent accidental commands from a G-code file having these characters and gives lots of space for future expansion.

* \[**扩展ASCII实时命令**：所有覆盖和未来实时命令都在扩展ASCII字符空间中定义。不幸的是，在键盘上不容易输入，但有助于防止G代码文件中包含这些字符的意外命令，并为将来的扩展提供大量空间。\]

* **Message Prefixes** : Every message type from Grbl has a unique prefix to help GUIs immediately determine what the message is and parse it accordingly without having to know context. The prior interface had several instances of GUIs having to figure out the meaning of a message, which made everything more complicated than it needed to be.
New OEM specific features, such as safety door parking, single configuration file build option, EEPROM restrictions and restoring controls, and storing product data information.

* \[**消息前缀**：Grbl中的每种消息类型都有一个唯一的前缀，以帮助GUI立即确定消息是什么，并相应地解析它，而无需知道上下文。先前的接口有几个GUI实例，它们必须弄清楚消息的含义，这使得一切变得比需要的更复杂。
新的OEM特定功能，如安全门停车、单一配置文件构建选项、EEPROM限制和恢复控制，以及存储产品数据信息。\]

* **New safety door parking motion as a compile-option**. Grbl will retract, disable the spindle/coolant, and park near Z max. When resumed, it will perform these tasks in reverse order and continue the program. Highly configurable, even to add more than one parking motion. See config.h for details.

* \[**新的安全门停车运动作为一个选项**。Grbl将收回、禁用主轴/冷却液并停在Z max附近。恢复时，它将以相反顺序执行这些任务并继续程序。高度可配置，甚至可以添加多个停车动作。有关详细信息，请参见config.h。\]

* **New '$' Grbl settings for max and min spindle rpm**. Allows for tweaking the PWM output to more closely match true spindle rpm. When max rpm is set to zero or less than min rpm, the PWM pin D11 will act like a simple enable on/off output.

* \[**主轴最大和最小转速的新“$”Grbl设置**。允许调整PWM输出以更接近真实主轴转速。当最大转速设置为零或小于最小转速时，PWM引脚D11将起到简单启用开/关输出的作用。\]

* **Updated G28 and G30 behavior** from NIST to LinuxCNC G-code description. In short, if an intermediate motion is specified, only the axes specified will move to the stored coordinates, not all axes as before.

* \[**将G28和G30行为**从NIST更新为LinuxCNC G代码说明。简而言之，如果指定了中间运动，则只有指定的轴将移动到存储的坐标，而不是像以前那样移动所有轴。\]

* Lots of minor bug fixes and refactoring to make the code more efficient and flexible.

* \[大量的小错误修复和重构使代码更加高效和灵活。\]

* NOTE: Arduino Mega2560 support has been moved to an active, official Grbl-Mega project. All new developments here and there will be synced when it makes sense to.

* \[注：Arduino Mega2560支持已转移到一个活跃的官方Grbl大型项目。所有新的发展在这里和那里将同步时，它是有意义的。\]


### New features in v0.9! \[v0.9版本中的新功能！\]
* **IMPORTANT \[重要的\]:**
 * **Default serial baudrate is now 115200! (Up from 9600)**
 *  \[**默认串行波特率现在是115200！（从9600增加）**\]
 * **Z-Axis limit input on D11 has swapped with spindle enable D12 to support variable spindle PWM output.**
 *  \[**D11上的Z轴限制输入已与主轴启用D12交换，以支持可变主轴PWM输出**\]
 * **No QUEUE state: Queue was removed due to it being redundant. Holds now suspend Grbl and only allow realtime commands. Cycle start resumes, and reset exits.**
 *  \[**无队列状态：队列因冗余而被删除。现在保留挂起Grbl并仅允许实时命令。循环开始恢复，并重置退出**\]
* **_NEW_ Super Smooth Stepper Algorithm:**  Complete overhaul of the handling of the stepper driver to simplify and reduce task time per ISR tick. Much smoother operation with the new Adaptive Multi-Axis Step Smoothing (AMASS) algorithm which does what its name implies (see stepper.c source for details). Users should immediately see significant improvements in how their machines move and overall performance!
* \[**_新的超级平滑步进算法：*彻底检修步进驱动程序的处理，以简化和减少每次ISR的任务时间。使用新的自适应多轴步进平滑（AMASS）算法进行更平滑的操作，该算法符合其名称的含义（有关详细信息，请参阅stepper.c源代码）。用户应该立即看到他们的机器移动方式和整体性能的显著改进！\]
* **Stability and Robustness Updates:** Grbl's overall stability has been focused on for this version. The planner and step-execution interface has been completely re-written for robustness and incorruptibility by the introduction of an intermediate step segment buffer that "checks-out" steps from the planner buffer in real-time. This means we can now fearlessly drive Grbl to its highest limits. Combined with the new stepper algorithm and planner optimizations, this translated to **5x to 10x** overall performance increases in our testing! Also, stability and robustness tests have been reported to easily take 1.4 million (yes, **million**) line G-code programs like a champ!
* \[**稳定性和鲁棒性更新**：*Grbl的整体稳定性一直是本版本关注的焦点。通过引入中间步骤段缓冲区，planner和step execution接口已完全重新编写，以实现健壮性和不可破坏性，中间步骤段缓冲区可实时“检出”planner缓冲区中的步骤。这意味着我们现在可以无所畏惧地将Grbl推向最高极限。结合新的步进算法和规划器优化，这在我们的测试中转化为**5倍到10倍**的总体性能提升！此外，据报道，稳定性和健壮性测试可以轻松地像champ一样使用140万（是的，**百万**）行G代码程序！\]
* **(x4)+ Faster Planner:** Planning computations improved four-fold or more by optimizing end-to-end operations, which included streamlining the computations and introducing a planner pointer to locate un-improvable portions of the buffer and not waste cycles recomputing them.
* \[**（x4）+更快的规划器：**通过优化端到端操作，规划计算提高了四倍或更多，包括简化计算并引入规划器指针来定位缓冲区中不可改进的部分，而不是浪费重新计算的周期。\]
* **Variable Spindle Speed Output:** Enables a hardware PWM output for 'S' G-code commands. **NOTE:** This feature requires a pin swap with the Z-limit D11 pin and spindle enable D12 pin to access the hardware PWM on pin D12. The Z-limit pin, now on D12, should work just as it did before.
* \[**可变主轴转速输出：**为“S”G代码命令启用硬件PWM输出**注：**此功能需要与Z限制D11引脚和主轴启用D12引脚进行引脚交换，以访问引脚D12上的硬件PWM。Z限位销现在位于D12上，应能像以前一样工作。\]
* **Compile-able via Arduino IDE!:** Grbl's source code may now be downloaded and altered, and then be compiled and flashed directly through the Arduino IDE, which should work on all platforms. See the Wiki for details on how to do it.
* \[**可通过Arduino IDE编译！：**Grbl的源代码现在可以下载和修改，然后直接通过Arduino IDE编译和闪现，该IDE可以在所有平台上运行。有关如何操作的详细信息，请参见Wiki。\]
* **G-Code Parser Overhaul:** Completely re-written from the ground-up for 100%-compliance* to the G-code standard. (* Parts of the NIST standard are a bit out-dated and arbitrary, so we altered some minor things to make more sense. Differences are outlined in the source code.) We also took steps to allow us to break up the G-code parser into distinct separate tasks, which is key for some future development ideas and improvements.
* \[**G代码解析器大修：**完全从头重写，100%符合G代码标准。（*NIST标准的某些部分有点过时和武断，因此我们修改了一些小东西以使其更有意义。源代码中概述了差异。）我们还采取了一些步骤，使我们能够将G代码解析器分解为不同的独立任务，这对于一些未来的开发想法和改进至关重要。\]
* **Independent Acceleration and Velocity Settings:** Each axis may be defined with unique acceleration and velocity parameters and Grbl will automagically calculate the maximum acceleration and velocity through a path depending on the direction traveled.
* \[ **独立的加速度和速度设置：**每个轴可以定义唯一的加速度和速度参数，Grbl将根据行驶方向自动计算通过路径的最大加速度和速度。\]
* **Soft Limits:** Checks if any motion command exceeds workspace limits before executing it, and alarms out, if detected. 
* \[**软限制：**执行前检查是否有任何运动命令超出工作空间限制，如果检测到，则发出警报。\]
* **Probing:** The G38.2, G38.3, G38.4, & G38.5 straight probe G-code commands are now supported and connected through the A5 pin.
* \[**探测：**现在支持G38.2、G38.3、G38.4和G38.5直探头G代码命令，并通过A5引脚连接。\]
* **Tool Length Offsets:** Probing doesn't make sense without tool length offsets (TLO), so we added it! The G43.1 dynamic TLO (described by linuxcnc.org) and G49 TLO cancel commands are now supported. G43.1 dynamic TLO works like the normal G43 TLO (NOT SUPPORTED) but requires an additional axis word with the offset value attached. We did this so Grbl does not have to track and maintain a tool offset database in its memory. Perhaps in the future, we will support a tool database, but not for this version.
* \[**刀具长度偏移：**没有刀具长度偏移（TLO），探测就没有意义，所以我们添加了它！现在支持G43.1动态TLO（由linuxcnc.org描述）和G49 TLO取消命令。G43.1动态TLO的工作原理与普通G43 TLO类似（不受支持），但需要附加带有偏移值的轴字。我们这样做是为了Grbl不必在其内存中跟踪和维护刀具偏移数据库。也许在将来，我们将支持工具数据库，但不支持此版本。\]
* **Improved Arc Performance:** The larger the arc radius, the faster Grbl will trace it! We are now defining arcs in terms of arc chordal tolerance, rather than a fixed segment length. This automatically scales the arc segment length such that maximum radial error of the segment from the true arc is never more than the chordal tolerance value of a super-accurate default of 0.002 mm.
* \[**改进的电弧性能：**电弧半径越大，Grbl跟踪它的速度越快！我们现在根据圆弧弦公差定义圆弧，而不是固定的线段长度。这将自动缩放弧段长度，以便弧段与真实弧的最大径向误差永远不会超过超精确默认值0.002 mm的弦公差值。\]
* **New Grbl SIMULATOR! (by @jgeisler and @ashelly):** A completely independent wrapper of the Grbl main source code that may be compiled as an executable on a computer. No Arduino required. Simply simulates the responses of Grbl as if it was on an Arduino.
* \[**全新Grbl模拟器！（@jgeisler和@ashelly）：**Grbl主要源代码的完全独立包装，可在计算机上编译为可执行文件。不需要Arduino。简单地模拟Grbl的响应，就像它在Arduino上一样。\]
* **CPU Pin Mapping:** In an effort for Grbl to be compatible with other AVR architectures, such as the 1280 or 2560, a new cpu_map.h pin configuration file has been created to allow Grbl to be compiled for them. This is currently user supported, so your mileage may vary. If you run across a bug, please let us know or better, send us a fix! Thanks in advance!
* \[**CPU引脚映射：**为了使Grbl与其他AVR架构（如1280或2560）兼容，已创建一个新的CPU_map.h引脚配置文件，以允许为它们编译Grbl。这是目前用户支持的，因此您的里程数可能会有所不同。如果您遇到一个错误，请让我们知道或更好，给我们一个修复！提前谢谢！\]
* **Configurable Real-time Status Reporting:** Users can now customize the type of real-time data Grbl reports back when they issue a '?' status report. This includes data such as: machine position, work position, planner buffer usage, serial RX buffer usage.
* \[**可配置的实时状态报告：**用户现在可以在发布“？”状态报告时自定义实时数据Grbl报告的类型。这包括以下数据：机器位置、工作位置、计划器缓冲区使用情况、串行接收缓冲区使用情况。\]
* **Updated Homing Routine:** Sets workspace volume in all negative space regardless of limit switch position. Common on pro CNCs. But, the behavior may be changed by a compile-time option though. Now tied directly into the main planner and stepper modules to reduce flash space and allow maximum speeds during seeking.
* \[**更新的归位例程：**设置所有负空间中的工作空间体积，而不考虑限位开关的位置。在亲CNC上常见。但是，编译时选项可能会改变行为。现在直接连接到主规划器和步进器模块，以减少闪存空间，并在搜索过程中实现最高速度。\]
* **CoreXY Support:** Grbl now supports CoreXY kinematics on an introductory-level. Most functions have been verified to work, but there may be bugs here or there. Please report any problems you find!
* \[**CoreXY支持：**Grbl现在在入门级支持CoreXY运动学。大多数功能都已经过验证，可以正常工作，但这里或那里可能有bug。请报告您发现的任何问题！\]
* **Safety Door Support:** Safety door switches are now supported. Grbl will force a feed hold, shutdown the spindle and coolant, and wait until the door switch has closed and the user has issued a resume. Upon resuming, the spindle and coolant will re-energize after a configurable delay and continue. Useful for OEMs that require this feature.
* \[**安全门支持：**现在支持安全门开关。Grbl将强制进给保持，关闭主轴和冷却液，并等待门开关关闭且用户发出恢复。恢复后，主轴和冷却液将在可配置的延迟后重新通电并继续。适用于需要此功能的OEM。\]
* **Full Limit and Control Pin Configurability:** Limits and control pins operation can now be interpreted by Grbl however you'd like, with the internal pull-up resistors enabled or disabled, or reading a high or low as a trigger. This should cover all wiring and NO or NC switch scenarios.
* \[**完全限位和控制引脚配置：**限位和控制引脚操作现在可以由Grbl解释，但您愿意，内部上拉电阻器启用或禁用，或读取高或低作为触发器。这应涵盖所有接线和NO或NC开关情况。\]
* **Optional Limit Pin Sharing:** Limit switches can be combined to share the same pins to free up precious I/O pins for other purposes. When combined, users must adjust the homing cycle mask in config.h to not home the axes on a shared pin at the same time. Don't worry; hard limits and the homing cycle still work just like they did before.
* \[**可选限位管脚共享：**限位开关可以组合在一起共享相同的管脚，以释放宝贵的I/O管脚用于其他用途。组合时，用户必须在config.h中调整归位循环掩码，以避免同时将轴归位在共享管脚上。别担心；硬限制和归位循环仍然像以前一样有效。\]
* **Additional Compile-Time Feature Options:** Limit/control pin state reporting, line number tracking, real-time feed rate reporting, and more.
* \[**其他编译时功能选项：**限制/控制引脚状态报告、行号跟踪、实时进给速率报告等。\]


### New features in v0.8! \[v0.8版本中的新功能！\]
A lot has happened since the v0.7. We're pushing real hard to create a simple, yet powerful CNC controller for the venerable Arduino. Here's a list of the new things that have come to v0.8.

 \[自v0.7版本以来发生了很多事情。我们正在努力为久负盛名的Arduino开发一款简单但功能强大的CNC控制器。下面是v0.8版本中的新事物列表。\]
* **Multi-Tasking Run-time Commands:** Feed hold with controlled deceleration for no loss of location, Resume after feed hold, Reset, and Status Reporting.
* \[**多任务运行时命令：**进给保持，带控制减速，无位置丢失，进给保持、复位和状态报告后恢复。\]
* **Advanced Homing Cycle:** Lots of configuration options for different types of machines from which axes to move when to their search directions. Limit switches may also be used as hard limits as well.
* \[**高级归位循环：**针对不同类型的机器提供了大量配置选项，当轴移动到其搜索方向时，可从这些机器移动轴。限位开关也可用作硬限位。\]
* **Persistent Coordinate System Data:** Work Coordinate Systems (G54-G59) and Pre-Defined Positions (G28,G30) are held in EEPROM, so they'll always be set as you last set them.
* \[**永久坐标系数据：**工作坐标系（G54-G59）和预定义位置（G28、G30）保存在EEPROM中，因此它们将始终在上次设置时进行设置。\]
* **Check G-Code Mode:** Sets up Grbl to run through your program without moving anything, so you can check whether or not you have any errors that Grbl may not like.
* \[**检查G代码模式：**将Grbl设置为在不移动任何内容的情况下运行程序，以便您可以检查是否存在Grbl可能不喜欢的错误。\]
* **Improved Feedback:** Reports real-time position, what Grbl is doing, the G-code parser state, and stored coordinate offset values.
* \[**改进的反馈：**报告实时位置、Grbl正在做什么、G代码解析器状态和存储的坐标偏移值。\]
* **Startup blocks:** Auto-magically runs user G-code blocks at startup or reset. Can be used to set your defaults.
* \[**启动块：**自动在启动或重置时神奇地运行用户G代码块。可用于设置默认值。\]
* **Pin-outs:** Cycle start, feed hold, and abort are now pinned-out to the A0, A1, and A2 pins. Just connect a normally-open switch to the pin and ground. That's it!
* \[**引脚输出：**循环开始、进给保持和中止现在固定在A0、A1和A2引脚上。只需将常开开关连接到引脚和接地。就这样！\]
* More Robust G-Code Parser with Error Checking Feedback.
* \[具有错误检查反馈的更健壮的G代码解析器。\]
* And much more!
* \[还有更多！\]

***
_Grbl v0.8 (and prior) is distributed under the MIT-License and was developed by Simen Svale Skogsrud, Sungeun K. Jeon Ph.D., and Jens Geisler._

\[Grbl v0.8（及之前版本）根据麻省理工学院许可证发行，由Simen Svale Skogsrud、Sungeun K.Jeon博士和Jens Geisler开发_\]

_Grbl is distributed under the GPLv3 license and is developed by Sungeun K. Jeon Ph.D.. See [Licensing](https://github.com/gnea/grbl/wiki/Licensing) for more details._

\[_Grbl根据GPLv3许可证发行，由Sungeun K.Jeon博士开发。。见[发牌](https://github.com/gnea/grbl/wiki/Licensing)更多细节_\]

***

## Documentation for Previous Versions \[以前版本的文档\]

* **[Old Wiki Homepage \[旧版本主页\]](https://github.com/grbl/grbl/wiki)**

* **[Configuring Grbl v0.9 \[配置Grbl v0.9\]](https://github.com/grbl/grbl/wiki/Configuring-Grbl-v0.9)**

* **[Configuring Grbl v0.8 \[配置Grbl v0.8\]](https://github.com/grbl/grbl/wiki/Configuring-Grbl-v0.8)**

* **[Configuring Grbl v0.7 \[配置Grbl v0.7\]](https://github.com/grbl/grbl/wiki/Configuring-Grbl-v0.7)**