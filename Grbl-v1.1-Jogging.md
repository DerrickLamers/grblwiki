
#### _Quick-Links \[快速链接\]:_
* [Joystick Implementation \[操纵杆实现\]](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Jogging#joystick-implementation)
* [Example: Joystick-Pendant Instructable \[示例：操纵杆吊坠可指示\]](http://www.instructables.com/id/GRBL-CNC-Joystick-Pendant/)

This document outlines how to use Grbl v1.1's new jogging commands. These commands differ because they can be cancelled and all queued motions are automatically purged with a simple jog-cancel (0x85 as explained in [Extended Ascii Realtime Command](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#extended-ascii-realtime-command-descriptions) ) or feed hold real-time command. Jogging commands do not alter the g-code parser state in any way, so you no longer have to worry if you remembered to set the distance mode back to `G90` prior to starting a job. Also, jogging works well with an analog joysticks and rotary dials! See the implementation notes below.

 \[本文档概述了如何使用GRBLV1.1版的新点动命令。这些命令不同，因为它们可以被取消，并且所有排队的运动都可以通过简单的点动取消（0x85，如[Extended Ascii Realtime Command]中所述）自动清除(https://github.com/gnea/grbl/wiki/Grbl-v1.1-Commands#extended-ascii实时命令描述）或feed hold实时命令。点动命令不会以任何方式改变g代码解析器的状态，因此，如果在开始作业之前记得将距离模式设置回'G90'，则无需担心。此外，慢跑工作与模拟操纵杆和旋转拨号！请参见下面的实施说明。\]

## How to Use \[如何使用\]
Executing a jog requires a specific command structure, as described below:

 \[执行jog需要特定的命令结构，如下所述：\]

 - The first three characters must be '$J=' to indicate the jog.
 - \[前三个字符必须为`$J=`以指示点动。\]
 - The jog command follows immediately after the '=' and works like a normal G1 command.
 - \[jog命令紧跟在`=`之后，与普通G1命令类似。\]
 - Feed rate is only interpreted in G94 units per minute. A prior G93 state is ignored during jog.
 - \[进给速度仅以G94单位/分钟进行解释。点动期间忽略先前的G93状态。\]
 - Required words:
 - \[必填词：\]
   - XYZ: One or more axis words with target value.
   - \[XYZ：具有目标值的一个或多个轴字。\] 
   - F - Feed rate value. NOTE: Each jog requires this value and is not treated as modal.
   - \[F-进给速度值。注意：每个点动都需要此值，不作为模态处理。\] 
 - Optional words: Jog executes based on current G20/G21 and G90/G91 g-code parser state. If one of the following optional words is passed, that state is overridden for one command only.
 - \[可选字：Jog根据当前G20/G21和G90/G91 g代码解析器状态执行。如果传递了以下可选字之一，则仅为一个命令覆盖该状态。\]
   - G20 or G21 - Inch and millimeter mode
   - \[G20或G21-英寸和毫米模式\] 
   - G90 or G91 - Absolute and incremental distances
   - \[G90或G91-绝对距离和增量距离\] 
   - G53 - Move in machine coordinates
   - \[G53-在机器坐标中移动\] 
   - N line numbers are valid. Will show in reports, if enabled, but are otherwise ignored.
   - \[N行编号有效。如果启用，将显示在报告中，但在其他情况下将被忽略。\] 
 - All other g-codes, m-codes, and value words (including S and T) are not accepted in the jog command.
 - \[jog命令中不接受所有其他g代码、m代码和值字（包括S和T）。\]
 - Spaces and comments are allowed in the command. These are removed by the pre-parser.
 - \[命令中允许使用空格和注释。这些由预解析器删除。\]

 - Example: G21 and G90 are active modal states prior to jogging. These are sequential commands.
 - \[示例：G21和G90是慢跑前的激活模式状态。这些是顺序命令。\]
    - `$J=X10.0 Y-1.5 F100` will move to X=10.0mm and Y=-1.5mm in work coordinate frame (WPos) at a feed rate of 100.
    - \[`$J=X10.0 Y-1.5 F100`将以100的进给速率在工作坐标系（WPos）中移动到X=10.0mm和Y=-1.5mm。\] 
    - `$J=G91 G20 X0.5 F10` will move +0.5 inches (12.7mm) to X=22.7mm (WPos) at a feed rate of 10. Note that G91 and G20 are only applied to this jog command.
    - \[\] 
    - `$J=G53 Y5.0 F10` will move the machine to Y=5.0mm in the machine coordinate frame (MPos) at a feed rate of 10. If the work coordinate offset for the y-axis is 2.0mm, then Y is 3.0mm in (WPos).
    - \[`$J=G53 Y5.0 F10`将以10的进给速度在机器坐标系（MPos）中将机器移动到Y=5.0mm。如果y轴的工作坐标偏移为2.0mm，则y为3.0mm in（WPos）。\] 

Jog commands behave almost identically to normal g-code streaming. Every jog command will
return an 'ok' when the jogging motion has been parsed and is setup for execution. If a
command is not valid, Grbl will return an 'error:'. Multiple jogging commands may be
queued in sequence.
  
 \[Jog命令的行为与普通g代码流几乎相同。当慢跑运动被解析并设置为执行时，每个慢跑命令将返回`ok`。如果命令无效，Grbl将返回`错误：`。多个点动命令可以按顺序排队。\] 

The main differences are \[主要区别是\] :  

- During a jog, Grbl will report a 'Jog' state while executing the jog.
- \[在点动期间，Grbl将在执行点动时报告`点动`状态。\] 
- A jog command will only be accepted when Grbl is in either the 'Idle' or 'Jog' states.
- \[只有当Grbl处于`空闲`或`jog`状态时，才会接受jog命令。\] 
- Jogging motions may not be mixed with g-code commands while executing, which will return a lockout error, if attempted.
- \[执行时，点动动作不得与g代码命令混合，如果尝试，将返回锁定错误。\] 
- All jogging motion(s) may be cancelled at anytime with a simple jog cancel realtime command or a feed hold or safety door event. Grbl will automatically flush Grbl's internal buffers of any queued jogging motions and return to the 'Idle' state. No soft-reset required.
- \[通过简单的点动取消实时命令或进给保持或安全门事件，可随时取消所有点动动作。Grbl将自动刷新Grbl的任何排队慢跑运动的内部缓冲区，并返回到`空闲`状态。无需软复位。\] 
- If soft-limits are enabled, jog commands that exceed the machine travel simply do not execute the command and return an error, rather than throwing an alarm as in normal operation.
- \[如果启用了软限制，超过机器行程的点动命令不会执行该命令并返回错误，而不会像正常操作中那样发出警报。\] 
- IMPORTANT: Jogging does not alter the g-code parser state. Hence, no g-code modes need to be explicitly managed, unlike previous ways of implementing jogs with commands like 'G91G1X1F100'. Since G91, G1, and F feed rates are modal and if they are not changed back prior to resuming/starting a job, a job may not run how its was intended and result in a crash.
- \[要点：点动不会改变g代码解析器的状态。因此，不需要显式管理g代码模式，这与以前使用`G91G1X1F100`等命令实现JOG的方法不同。由于G91、G1和F进给速率是模态的，并且如果在恢复/启动作业之前没有将它们更改回去，则作业可能无法按预期方式运行，并导致崩溃。\] 

For GUIs and joysticks, there are times when the UI needs to know when Grbl has completed a jog cancel realtime command with minimal latency so it can immediately resume sending more jog commands. Unfortunately, Grbl currently does not provide feedback when a realtime command has been executed and completed. Often, just checking if status report state has changed from `Jog` to `Idle` will do the job, but this can be complicated to implement and isn't the most effective. Instead, after sending a jog cancel command, stop streaming and issue a `G4P0` command to Grbl. This will be executed immediately after the jog cancel has been completed with zero delay and return an 'ok' when done. This provides the least amount of latency and simplest syncing implementation.  

 \[对于GUI和操纵杆，有时UI需要知道Grbl何时以最小延迟完成jog cancel实时命令，以便可以立即恢复发送更多jog命令。不幸的是，Grbl当前在执行和完成实时命令时不提供反馈。通常，只需检查状态报告状态是否已从`Jog`更改为`Idle`，就可以完成这项工作，但实现起来可能很复杂，而且不是最有效的。相反，在发送jog cancel命令后，停止流式传输并向Grbl发出`G4P0`命令。这将在点动取消以零延迟完成后立即执行，并在完成后返回`ok`。这提供了最少的延迟和最简单的同步实现。\] 

------

## Joystick Implementation \[操纵杆实现\] 

Jogging in Grbl v1.1 is generally intended to address some prior issues with old bootstrapped jogging methods. Unfortunately, the new Grbl jogging is not a complete solution. Flash and memory restrictions prevent the original envisioned implementation, but most of these can be mimicked by the following suggested methodology. 

 \[GRBLV1.1中的慢跑通常旨在解决旧的自举慢跑方法之前的一些问题。不幸的是，新的Grbl慢跑不是一个完整的解决方案。闪存和内存限制阻止了最初设想的实现，但大多数限制可以通过以下建议的方法进行模仿。\] 

With a combination of the new jog cancel and moving in `G91` incremental mode, the following implementation can create low latency feel for an analog joystick or similar control device.

 \[通过将新的jog cancel（点动取消）和`G91`增量模式下的移动相结合，以下实现可为模拟操纵杆或类似控制设备创造低延迟感觉。\] 

- Basic Implementation Overview: 
- \[基本实施概述：\] 
  - Create a loop to read the joystick signal and translate it to a desired jog motion vector.
  - \[创建一个循环以读取操纵杆信号并将其转换为所需的jog运动矢量。\] 
  - Send Grbl a very short `G91` incremental distance jog command with a feed rate based on the joystick throw.
  - \[向Grbl发送一个非常短的`G91`递增距离jog命令，该命令的进给速度基于操纵杆摆度。\] 
  - Wait for an 'ok' acknowledgement before restarting the loop.
  - \[在重新启动循环之前，等待`确定`确认。\] 
  - Continually read the joystick input and send Grbl short jog motions to keep Grbl's planner buffer full.
  - \[持续读取操纵杆输入并发送Grbl短点动动作，以保持Grbl的planner缓冲区满。\] 
  - If the joystick is returned to its neutral position, stop the jog loop and simply send Grbl a jog cancel real-time command. This will stop motion immediately somewhere along the programmed jog path with virtually zero-latency and automatically flush Grbl's planner queue. It's not advised to use a feed hold to cancel a jog, as it can lead to inadvertently suspending Grbl if its sent after returning to the IDLE state.
  - \[如果操纵杆返回到空档位置，停止点动回路，只需向Grbl发送点动取消实时命令。这将立即停止沿编程jog路径的某处运动，几乎没有延迟，并自动刷新Grbl的planner队列。不建议使用进给保持来取消点动，因为如果Grbl在返回空闲状态后发送，可能会导致意外暂停Grbl。\] 


The overall idea is to minimize the total distance in the planner queue to provide a low-latency feel to joystick control. The main trick is ensuring there is just enough distance in the planner queue, such that the programmed feed rate is always met. How to compute this will be explain later. In practice, most machines will have a 0.5-1.0 second latency. When combined with the immediate jog cancel command, joystick interaction can be quite enjoyable and satisfying.

\[总体思路是最小化planner队列中的总距离，以提供操纵杆控制的低延迟感觉。主要的技巧是确保计划器队列中有足够的距离，以便始终满足编程的进给速度。如何计算将在后面解释。实际上，大多数机器将有0.5-1.0秒的延迟。当与即时点动取消命令相结合时，操纵杆交互会非常愉快和令人满意。\] 

However, please note, if a machine has a low acceleration and is being asked to move at a high programmed feed rate, joystick latency can get up to a handful of seconds. It may sound bad, but this is how long it'll take for a low acceleration machine, traveling at a high feed rate, to slow down to a stop. The argument can be made for a low acceleration machine that you really shouldn't be jogging at a high feed rate. It is difficult for a user to gauge where the machine will come to a stop. You risk overshooting your target destination, which can result in an expensive or dangerous crash. 

\[但是，请注意，如果机器的加速度较低，并且被要求以高编程进给速率移动，操纵杆延迟可能长达几秒钟。这听起来可能很糟糕，但这是一台低加速度机器，以高进给速度行驶，减速到停止所需的时间。对于一台低加速度的机器，你真的不应该在高进给速度下慢跑。用户很难判断机器将停在哪里。您有可能超出您的目标目的地，这可能导致昂贵或危险的撞车。\] 

One of the advantages of this approach is that a GUI can deterministically track where Grbl will go by the jog commands it has already sent to Grbl. As long as a jog isn't cancelled, every jog command is guaranteed to execute. In the event a jog cancel is invoked, the GUI would just need to refresh their internal position from a status report after Grbl has cleared planner buffer and returned to the IDLE (or DOOR, if ajar) state from the JOG state. This stopped position will always be somewhere along the programmed jog path. If desired, jogging can then be quickly and easily restarted with a new tracked path.

\[这种方法的优点之一是GUI可以通过它已经发送给Grbl的jog命令确定地跟踪Grbl将去哪里。只要没有取消点动，就保证执行每个点动命令。如果调用jog cancel，则在Grbl清除planner缓冲区并从jog状态返回空闲（或门未关，如果未关）状态后，GUI只需从状态报告中刷新其内部位置。该停止位置将始终位于编程点动路径的某个位置。如果需要，可以使用新的跟踪路径快速轻松地重新开始慢跑。\] 

In combination with `G53` move in machine coordinates, a GUI can restrict jogging from moving into "keep-out" zones inside the machine space. This can be very useful for avoiding crashing into delicate probing hardware, workholding mechanisms, or other fixed features inside machine space that you want to avoid.

\[结合机器坐标中的'G53'移动，GUI可以限制点动进入机器空间内的`禁止进入`区域。这对于避免碰撞到精密的探测硬件、工件夹持机构或机器空间内您想要避免的其他固定功能非常有用。\] 

#### How to compute incremental distances\[如何计算增量距离\] 

The quickest and easiest way to determine what the length of a jog motion needs to be to minimize latency are defined by the following equations.

\[确定慢跑运动长度以最小化延迟的最快和最简单方法由以下方程式定义。\] 

`s = v * dt` - Computes distance traveled for next jog command.

\[`s=v*dt`-计算下一个jog命令的行驶距离。\] 

where \[在哪里\] :  

- `s` - Incremental distance of jog command.
- \[`s`-点动命令的增量距离。\] 
- `dt` - Estimated execution time of a single jog command **in seconds**.
- \[`dt`-单个jog命令的估计执行时间**以秒为单位**。\]   
- `v` - Current jog feed rate in **mm/sec**, not mm/min. Less than or equal to max jog rate.
- \[`v`-当前点动进给速率，单位为**毫米/秒**，而不是毫米/分钟。小于或等于最大点动速率。\] 
- `N` - Number of Grbl planner blocks (`N=15`)
- \[`N`—Grbl规划块的数量（`N=15`）\] 
- `T = dt * N` - Computes total estimated latency in seconds.
- \[`T=dt*N`-以秒为单位计算总估计延迟。\] 
 
The time increment `dt` may be defined to whatever value you need. Obviously, you'd like the lowest value, since that translates to lower overall latency `T`. However, it is constrained by two factors.

\[时间增量'dt'可以定义为您需要的任何值。显然，您希望得到最低的值，因为这意味着总体延迟`T`更低。然而，它受到两个因素的制约。\] 

- `dt > 10ms` - The time it takes Grbl to parse and plan one jog command and receive the next one. Depending on a lot of factors, this can be around 1 to 5 ms. To be conservative, `10ms` is used. Keep in mind that on some systems, this value may still be greater than `10ms` due to round-trip communication latency.
- \[`dt>10ms`-Grbl解析和计划一个jog命令并接收下一个命令所需的时间。根据许多因素，这可能是1到5毫秒。保守地说，使用`10毫秒`。请记住，在某些系统上，由于往返通信延迟，此值可能仍然大于`10ms`。\] 

- `dt > v^2 / (2 * a * (N-1))` - The time increment needs to be large enough to ensure the jog feed rate will be achieved. Grbl always plans to a stop over the total distance queued in the planner buffer. This is primarily to ensure the machine will safely stop if a disconnection occurs. This equation simply ensures that `dt` is big enough to satisfy this constraint. 
- \[`dt>v^2/（2*a*（N-1））`-时间增量需要足够大，以确保实现点动进给速度。Grbl始终计划在planner缓冲区中排队的总距离上停车。这主要是为了确保机器在发生断开时安全停止。这个等式只是确保'dt'足够大以满足这个约束。\] 

	- For simplicity, use the max jog feed rate for `v` in mm/sec and the smallest acceleration setting between the jog axes being moved in mm/sec^2.
  - \[为简单起见，使用`v`的最大点动进给速率（单位：mm/sec），以及移动的点动轴之间的最小加速度设置（单位：mm/sec^2）。\] 

  - For a lower latency, `dt` can be computed for each jog motion, where `v` is the current rate and `a` is the max acceleration along the jog vector. This is very useful if traveling a very slow speeds to locate a part zero. The `v` rate would be much lower in this scenario and the total latency would decrease quadratically.
  - \[对于较低的延迟，可以为每个点动运动计算'dt'，其中'v'是当前速率，'a'是沿点动矢量的最大加速度。如果以非常慢的速度行驶以定位零件零，这非常有用。在这种情况下，`v`速率会低得多，总延迟会以二次方的方式减少。\] 

In practice, most CNC machines will operate with a jogging time increment of `0.025 sec` < `dt` < `0.06 sec`, which translates to about a `0.4` to `0.9` second total latency when traveling at the max jog rate. Good enough for most people. 

\[在实践中，大多数数控机床将以`0.025秒`<`dt`<`0.06秒`的慢跑时间增量运行，这意味着以最大慢跑率运行时总延迟约为`0.4秒`到`0.9秒`。对大多数人来说已经足够好了。\] 

However, if jogging at a slower speed and a GUI adjusts the `dt` with it, you can get very close to the 0.1 second response time by human-interface guidelines for "feeling instantaneous". Not too shabby!

\[但是，如果以较慢的速度慢跑，并且GUI使用它调整`dt`，则根据`感觉瞬间`的人机界面指南，您可以获得非常接近0.1秒的响应时间。不要太寒酸！\] 

With some ingenuity, this jogging methodology may be applied to different devices such as a rotary dial or touchscreen. An "inertial-feel", like swipe-scrolling on a smartphone or tablet, can be simulated by managing the jog rate decay and sending Grbl the associated jog commands. While this jogging implementation requires more initial work by a GUI, it is also inherently more flexible because you have complete deterministic control of how jogging behaves.

\[凭借一些独创性，这种慢跑方法可以应用于不同的设备，如旋转拨号盘或触摸屏。`惯性感觉`，如在智能手机或平板电脑上滑动滚动，可以通过管理jog速率衰减和发送Grbl相关jog命令来模拟。虽然这种慢跑实现需要GUI进行更多的初始工作，但它本身也更灵活，因为您可以完全确定地控制慢跑的行为。\] 
