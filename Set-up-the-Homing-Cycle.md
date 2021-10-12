# Prerequisites \[先决条件\]:

 * Correctly configured axes.
 
   \[正确配置的轴。\]
# Home switches pins and wiring \[归为用开关针脚和接线\]

3 digital input pins are used for signaling Grbl \[数字输入引脚用于发送Grbl信号\]:

- *Pin 9* X Axis limit/Home input pin \[*引脚9*X轴限制/主输入引脚\]
- *Pin 10* Y Axis limit/Home input pin \[*引脚10*Y轴限制/主输入引脚\]
- *Pin 12* Z Axis limit/Home input pin \[*引脚12*Z轴限制/主输入引脚\]

Another place that explains the Limit switch configuration: [Wiring-Limit-Switches](https://github.com/gnea/grbl/wiki/Wiring-Limit-Switches) 

\[另一个解释限位开关配置的地方：[限位开关接线](https://github.com/gnea/grbl/wiki/Wiring-Limit-Switches)\]

Limit switches usually have three terminals \[限位开关通常有三个端子\] :

* One is common terminal (`COM`), one is normally open (`NO`) to the common terminal and another one is normally closed (`NC`) to common.  

  \[一个是公共端子（`COM`），一个是公共端子的常开（`NO`），另一个是公共端子的常闭（`NC`）。\]

In this case, we are going to use two terminals, normally open (`NO`) and common (`COM`). 

\[在本例中，我们将使用两个端子，常开（`NO`）和公共（`COM`）\]

Use of (`NC`) instead of (`NO`) is enabled by configuring `$5=1` .  

\[通过配置“$5=1”，可以使用（`NC`）代替（`NO`）。\]

All the common lines go to the arduino's GND, the `NO` lines go to the pin for that axis. This will result in this wiring 

\[所有的公共线都指向arduino的GND，'NO'线指向该轴的引脚。这将导致这种接线\]:


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

# Enable Home Cycle and Setup Home Parameters \[启用主循环并设置主参数\]

Homing is controlled by parameter `$22`. Type `$22=1` to enable it, `$22=0` to disable it. Homing can be triggered by typing `$H`.

\[归位由参数“$22”控制。键入“$22=1”启用它，键入“$22=0”禁用它。键入“$H”可以触发归位。\]

# Homing direction \[归航方向\]

The homing directions are controlled by setting `$23` setting it to a value defined below \[通过将“$23”设置为下面定义的值来控制归位方向\] :

| Homing direction \[归为方向\] | Value \[值\]  |
| ---------------- | ------ |
| X+ Y+ Z+         | 0      |
| X- Y+ Z+         | 1      |
| X+ Y- Z+         | 2      |
| X- Y- Z+         | 3      |
| X+ Y+ Z-         | 4      |
| X- Y+ Z-         | 5      |
| X+ Y- Z-         | 6      |
| X- Y- Z-         | 7      |

- Default setting (`$23=0`), the home location is the top right of your work area, with the spindle all the way up.

   \[-默认设置（`$23=0`），主位置在工作区的右上角，主轴一直向上。\]
- `$23=1` Top left home location.

   \[-`$23=1`左上角主位置。\]
- `$23=2` Bottom right of your work area to be the home location.

   \[-`$23=2`工作区右下角为家。\]
- `$23=3` Bottom left.

   \[`$23=3`左下角。\]
- `$23=4` Spindle down home location.

   \[`$23=4` 主轴向下的主位置。\]

# Homing Cycle Steps \[归位循环步数\]

By default, the homing cycle goes through the following steps \[默认情况下，归位循环经过以下步骤\]:

- Z axis \[Z轴\]
  1.    Z Axis will move up (positive) with Fast Rate (`$25`)
  
        \[Z轴将快速(`$25`)向上移动（正）\]

  1.    When Z home switch triggered, Z stop for a short time (`$26`) and back off a distance (`$27`)
  
        \[当Z home开关触发时，Z停止一小段时间（`$26`），并后退一段距离（`$27`）\]

  1.    Z Axis will move up slowly util it touches the Z home switch again (`$24`)
  
        \[Z轴将缓慢向上移动，直到再次接触Z home开关（`$24`）\]

  1.    Z Axis backs off a small distance (`$27`)
  
        \[Z轴后退一小段距离（`$27`）\]

- X and Y axis \[X轴和Y轴\]
  1.    X, Y Axis move both to Homing direction at fast rate  (`$25`)

        \[X、 Y轴以较快的速度（`$25`)向回零方向移动\]

  1.    The first Axis triggers the switch will stop and wait for the second axis to trigger
  
        \[第一个轴触发开关将停止并等待第二个轴触发\]

  1.    When second axis triggers the switch, both axis back off a distance  (`$27`)
  
        \[当第二个轴触发开关时，两个轴后退一段距离（`$27`）\]

  1.    Both X and Y axis will move toward switches again slowly, until both switches triggered again  (`$24`)
  
        \[X轴和Y轴将再次缓慢地向开关移动，直到两个开关再次触发（`$24`）\]

  1.    Both X and Y axis will back off a small distance (`$27`)
  
        \[X轴和Y轴都会后退一小段距离（`$27`)\]


# Homing speed \[归位速度\]


As described above, homing is done in two distinct phases per axis: feed and seek. The feed speed is controlled by setting `$25`. In this phase, GRBL is just trying to find the limit switch within a reasonable amount of time.

 \[如上所述，归位在每个轴的两个不同阶段完成：进给和寻道。进给速度由设置“$25”控制。在这个阶段，GRBL只是试图在合理的时间内找到限位开关。\]

After the feed phase, the seek phase does exactly the same thing, but at a low speed, controlled by setting `$24`. This phase is all about accurately finding the trigger point for the limit switch.

 \[在馈送阶段之后，寻道阶段执行完全相同的操作，但速度较低，由设置“$24”控制。该阶段的全部内容是准确找到限位开关的触发点。\]

# Homing travel \[归位旅行\]


GRBL will give up searching for a limit switch after 1.5x the max travel distance. The max travel distance is controlled by `$130`, (for x), `$131` (for y) and `$132` (for z). These numbers are also used for soft-limits, and should be set slightly below the length of your axes.

 \[在1.5倍最大行程距离后，GRBL将放弃搜索限位开关。最大移动距离由“$130”（对于x）、“$131”（对于y）和“$132”（对于z）控制。这些数字也用于软限制，应设置为略低于轴的长度。\]

After the feed phase, the axis moves back a little, to un-trigger the switch. This distance is controlled by setting `$27`. Set this number high enough so the limit switch is cleared, even when the feed phase overshoots.

 \[在进给阶段后，轴稍微向后移动，以取消触发开关。此距离由设置“$27”控制。将该数字设置得足够高，以便即使在馈电相位超调时，也能清除限位开关。\]

# TODO (documentation) \[全部（文件）\]


- How to configure for custom homing cycles.

   \[如何配置自定义归位循环。\]
- Common issues.

   \[共同问题。\]
- What's the homing cycle for? How do you use it? Is it really that useful? YES.

   \[归位周期是什么？你如何使用它？真的那么有用吗？对\]
  - Explain work coordinate systems(WCS) and G28/30 move to predefined locations
  
    \[解释工作坐标系（WCS）和G28/30移动到预定义位置\]
  - Explain how to save WCS and G28/30 coordinate frames using G10 and G28.1/30.1.
  
    \[解释如何使用G10和G28.1/30.1保存WCS和G28/30坐标系。\]
  - Explain how WCS and G28/30 are used in common scenarios  (All above explained perfectly in this video https://www.youtube.com/watch?v=fGtbkVJBXyE )
  
    \[解释WCS和G28/30如何在常见场景中使用（本视频完美地解释了以上所有内容）https://www.youtube.com/watch?v=fGtbkVJBXyE )\]
  - Provide clear link to LinuxCNC g-code descriptions.
  
    \[提供到LinuxCNC g代码说明的清晰链接。\]