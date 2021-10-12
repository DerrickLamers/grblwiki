_This wiki is intended to provide information on known bugs or issues. Please feel free to contribute more known bugs and their work arounds as they are discovered._

\[_此wiki旨在提供有关已知错误或问题的信息。请随时贡献更多已知的错误和他们的工作，因为他们被发现_\]

# Character-counting Streaming Protocol Issues \[字符计数流协议问题\]

 
* While the character-counting streaming protocol is indeed very efficient and effective, there are a couple of newly uncovered issues interface writers should aware of.

  \[虽然字符计数流式传输协议确实非常高效，但接口编写人员应该注意一些新发现的问题。\]


 * Since the GUI is preloading Grbl's serial RX buffer with commands, Grbl will continually execute all of the queued g-code in the RX serial buffer. The first problem is if there is an error at the beginning of the RX buffer, Grbl will continue to execute the remaining buffered g-code and the GUI won't be able to control what happens. The interim solution is to check all of the g-code via the $C check mode, so all errors are vetted prior to streaming.
 
   \[由于GUI使用命令预加载Grbl的串行RX缓冲区，Grbl将继续执行RX串行缓冲区中所有排队的g代码。第一个问题是，如果RX缓冲区的开头出现错误，Grbl将继续执行剩余的缓冲g代码，GUI将无法控制发生的情况。临时解决方案是通过$C检查模式检查所有g代码，因此在流媒体之前检查所有错误。\]

 * When Grbl stores data to EEPROM, the AVR requires all interrupts to be disabled during this write process, including the serial RX ISR. This means that if a g-code or Grbl $ command writes to EEPROM, the data sent during the write may be lost. This is usually rare and typically occurs when streaming a G10 command inappropriately inside a program. For robustness, GUIs should track and detect these EEPROM write commands and handle them appropriately by waiting for the queue to finish executing before sending more data. Note that the simple send-response protocol doesn't not suffer from this issue.
 
   \[当Grbl将数据存储到EEPROM时，AVR要求在此写入过程中禁用所有中断，包括串行RX ISR。这意味着，如果g代码或Grbl$命令写入EEPROM，则写入期间发送的数据可能会丢失。这通常很少见，通常在程序中不适当地传输G10命令时发生。为了健壮性，GUI应该跟踪和检测这些EEPROM写入命令，并在发送更多数据之前等待队列完成执行，从而适当地处理这些命令。请注意，简单发送响应协议不存在此问题。\]

# USB to Serial transmission errors \[USB到串行传输错误\]

* It has been found that both genuine Arduino boards, and clone Arduino boards experience occasional errors in transmission of data from the host computer to Grbl. The related errors could result in unwanted motion of the machine. The problem was originally discovered on a an Arduino clone that used a CH340G USB to serial chip, but the problem is also present to a lesser degree on genuine Arduino's and clones that use the Atmel 16U2 chip.  There is no fix for the CH340G USB to serial chip. An updated firmware is required to eliminate the problem on the boards using the 16U2 chip. 

   \[已经发现，无论是真正的Arduino板还是克隆的Arduino板，在将数据从主机传输到Grbl时都会偶尔出现错误。相关错误可能导致机器不必要的运动。问题最初是在使用CH340G USB到串行芯片的Arduino克隆上发现的，但在使用Atmel 16U2芯片的正版Arduino和克隆上也存在较小程度的问题。CH340G USB到串行芯片没有修复程序。需要更新固件以消除使用16U2芯片的主板上的问题。\]

* Clone Arduino boards using the CH340G USB to Serial chip experience occasional transmission errors and their use is at your own risk. There is no current fix for this problem on CH340G equipped boards.

   \[使用CH340G USB到串行芯片的克隆Arduino板偶尔会出现传输错误，使用这些板的风险由您自己承担。对于配备CH340G的电路板上的此问题，目前没有解决方案。\]

* Genuine Arduino boards and clone Arduino boards using the Atmel 16U2 USB to Serial chip also experience occasional transmission errors and it is recommended that users re-flash the 16U2 chip with updated firmware. You can use the instructions here: https://www.arduino.cc/en/Hacking/DFUProgramming8U2 to flash the new firmware, and the new firmware can be found here:https://github.com/AlexHolden/Arduino/tree/master/hardware/arduino/avr/firmwares/atmegaxxu2/arduino-usbserial  Many thanks to user AlexHolden for taking the time to edit the firmware to solve this problem.

   \[使用Atmel 16U2 USB转串行芯片的正版Arduino板和克隆Arduino板也会偶尔出现传输错误，建议用户使用更新的固件重新闪存16U2芯片。您可以使用此处的说明：https://www.arduino.cc/en/Hacking/DFUProgramming8U2 要刷新新固件，新固件可在此处找到：https://github.com/AlexHolden/Arduino/tree/master/hardware/arduino/avr/firmwares/atmegaxxu2/arduino-usbserial  非常感谢用户AlexHolden花时间编辑固件以解决此问题。\]

* You can read more about the problem in the issue here: https://github.com/grbl/grbl/issues/845

   \[您可以在本期中阅读有关此问题的更多信息：https://github.com/grbl/grbl/issues/845\]

# Baud rate limitations \[波特率限制\]

* Using low baud rates can result in unpredictable behavior. This unpredictable behavior can be exacerbated by other factors such as enabled echoes. Discussion with Sonny indicates that the serial communication in GRBL is designed and tested at 115200, so at baud rates of 115200 and above there should be no problem. In short, unless there is a very good reason, just don't run below 115200 baud and this should not be a problem. 

   \[使用低波特率可能导致不可预测的行为。这种不可预测的行为可能会因其他因素（如启用的回声）而加剧。与Sonny的讨论表明，GRBL中的串行通信是在115200下设计和测试的，因此在115200及以上的波特率下应该没有问题。简而言之，除非有很好的理由，否则不要在115200波特以下运行，这应该不是问题。\]
# Doesn't go slower than X mm/min! Or limitations at very slow step rates.  \[速度不会低于X毫米/分钟！或在非常慢的步进速率下的限制。\] 

* Grbl is limited to about 30 steps/sec (or 4 steps/sec if AMASS is disabled). This isn't really a bug, but a problem due to Arduino 328p 16-bit timer is only capable of timing at its slowest at 4ms increments (max prescaler and max count). Sure, Grbl can try to code in some conditions to emulate even slower step rates, but it's not worth the investment in precious flash space. It's far better to keep things simple and just move onto a processor with a better timer.

   \[Grbl限制为大约30步/秒（如果禁用了AMASS，则限制为4步/秒）。这并不是一个真正的错误，但由于Arduino 328p 16位计时器只能以4ms的增量（最大预分频器和最大计数）以最慢的速度计时而导致的问题。当然，Grbl可以尝试在某些条件下编写代码，以模拟更慢的步进速率，但这不值得在宝贵的闪存空间上投资。最好保持事情简单化，然后使用一个定时器更好的处理器。\]
* Most typical CNC applications do not come close to this low step rate. For example, a user wanted to build a star tracker machine with Grbl, but it needed to move at rates well below 1mm/min. If you have a special use case like this, you can try a few things. First, disable AMASS in config.h. This will bring down lowest step rate to 4 steps/sec. Second, increase your micro stepping on your stepper driver. This will increase the step/mm and increase your step rate without going any faster. Finally, increase your mechanical gear ratios, like switching out to a leadscrew with a lower pitch or a timing belt gear with a smaller radius. 

   \[大多数典型的CNC应用程序都没有达到如此低的步进速率。例如，一个用户想用Grbl构建一个星体跟踪器机器，但它需要以远低于1mm/min的速度移动。如果你有这样一个特殊的用例，你可以尝试一些事情。首先，在config.h中禁用AMASS。这将使最低步进速率降至4步/秒。第二，增加步进驱动程序的微步进。这将增加步长/mm，并在不加快速度的情况下提高步速。最后，增加你的机械传动比，比如改用小螺距的丝杠或小半径的同步带齿轮。\]
* To compute your lowest mm/min of your machine, use this equation: `(lowest mm/min) = (30 steps/sec) * (60 sec/min) / (axis steps/mm setting)`. Use `(4 steps/sec)` if you have AMASS disabled.

   \[要计算机器的最低毫米/分钟，请使用以下等式：`（最低毫米/分钟）=（30步/秒）*（60秒/分钟）/（轴步/毫米设置）`。如果禁用了聚合，请使用`（4步/秒）`。\]