#### _Quick-Links \[快速链接\]:_
* [Streaming G-Code to Grbl \[将G代码流式传输到Grbl\]](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Interface#streaming-a-g-code-program-to-grbl)
* [Interaction with Grbl's Systems \[将G代码流式传输到Grbl\]](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Interface#interacting-with-grbls-systems)
* [Grbl Message Summary \[Grbl消息摘要\]](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Interface#message-summary)
* [Grbl Response Messages \[Grbl响应消息\]](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Interface#grbl-response-messages)
* [Grbl Push Messages \[Grbl推送消息\]](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Interface#grbl-push-messages)

See [Using Grbl](https://github.com/gnea/grbl/wiki/Using-Grbl) to learn about existing projects offering GUI and non-GUI interfaces to GRBL.

 \[参见[使用Grbl](https://github.com/gnea/grbl/wiki/Using-Grbl)了解向GRBL提供GUI和非GUI接口的现有项目。\]

***
### Please help us maintain this page! If you spot a problem, feel free to update it or notify us \[请帮助我们维护此页面！如果您发现问题，请随时更新或通知我们\].
***

# Grbl Interface Basics \[Grbl接口基础\]

The interface for Grbl is fairly simple and straightforward. With Grbl v1.1, steps have been taken to try to make it even easier for new users to get started, and for GUI developers to write their own custom interfaces to Grbl.

 \[Grbl的接口相当简单直接。在GRBLV1.1中，已经采取了一些步骤，试图让新用户更容易开始使用，GUI开发人员也更容易为Grbl编写自己的自定义界面。\]

Grbl communicates through the serial interface on the Arduino. You just need to connect your Arduino to your computer with a USB cable. Use any standard serial terminal program to connect to Grbl, such as: the Arduino IDE serial monitor, Coolterm, puTTY, etc. Or use one of the many great Grbl GUIs out there in the Internet wild.

 \[Grbl的接口相当简单直接。在GRBLV1.1中，已经采取了一些步骤，试图让新用户更容易开始使用，GUI开发人员也更容易为Grbl编写自己的自定义界面。\]
 
The primary way to talk to Grbl is performed by sending it a string of characters, followed by a carriage return. Grbl will then process the string, set it up for execution, and then reply back with a **response message**, also terminated by a return, to tell you how it went. These command strings include sending Grbl: a G-code block to execute, commands to configure Grbl's system settings, to view how Grbl is doing, etc. 

 \[与Grbl对话的主要方式是向Grbl发送一个字符串，后跟回车符。然后，Grbl将处理该字符串，将其设置为执行，然后用**响应消息**回复，并以返回结束，以告诉您该字符串是如何运行的。这些命令字符串包括发送Grbl：要执行的G代码块、配置Grbl系统设置的命令、查看Grbl的运行情况等。\]
 
To stream a g-code program to Grbl, the basic interface is to send Grbl a line of g-code, then wait for the proper **response message** starting with an `ok` or `error`. This signals Grbl has completed the parsing and executing the command. At times, Grbl may not respond immediately. This happens when Grbl is busy doing something else or waiting to place a commanded motion into the look-ahead planner buffer. Other times, usually at the start of a program, Grbl may quickly respond to several lines, but nothing happens. This occurs when Grbl places a series of commanded motions directly in the planner queue and will try to fill it up completely before starting.

 \[要将g代码程序流式传输到Grbl，基本接口是向Grbl发送一行g代码，然后等待以`ok`或`error`开头的正确**响应消息**。这表明Grbl已完成解析并执行该命令。有时，Grbl可能不会立即响应。当Grbl忙于做其他事情或等待将指令运动放入前瞻计划缓冲区时，会发生这种情况。其他时候，通常在程序开始时，Grbl可能会快速响应几行，但什么也没有发生。当Grbl将一系列指令动作直接放入计划器队列中，并在开始之前尝试将其完全填满时，就会发生这种情况。\]
 
Along with **response messages**, Grbl has **push messages** to provide more feedback on what Grbl is doing and are also strings terminated by a return. These messages may be `pushed` from Grbl to the user in response to a query or to let the user know something important just happened. These can come at any time, but usually from something like a settings print out when asked to. **Push messages** are easily identified because they don't start with an `ok` or `error` like **response messages** do. They are typically placed in `[]` brackets, `<>` chevrons, start with a `$`, or a specific string of text. These are all defined and described later in this document.

 \[除了**响应消息**，Grbl还有**推送消息**，以提供关于Grbl正在做什么的更多反馈，并且也是以返回终止的字符串。这些消息可以从Grbl`推送`给用户，以响应查询或让用户知道刚刚发生的重要事情。这些信息可以在任何时候出现，但通常是在要求打印时从设置打印出来**推送消息**很容易识别，因为它们不像**响应消息**那样以`ok`或`error`开头。它们通常放在`[]`括号、`<>` V形符号中，以`$`或特定的文本字符串开头。这些都在本文档后面进行了定义和描述。\]
 
Finally, Grbl has **real-time commands** that are invoked by a set of special characters that may be sent at any time and are not part of the basic streaming send-response interface. These cause Grbl to immediately execute the command and typically don't generate a response. These include pausing the current motion, speed up/down everything, toggle the spindle during a job, reset Grbl, or query Grbl for a real-time status report. See the `Commands` document to see what they are and how they work.

 \[最后，Grbl有**实时命令**，由一组特殊字符调用，这些字符可以随时发送，并且不属于基本流发送响应接口的一部分。这会导致Grbl立即执行命令，通常不会生成响应。这些包括暂停当前运动、加速/减速、在作业期间切换主轴、重置Grbl或查询Grbl以获取实时状态报告。请参阅`命令`文档以了解它们是什么以及它们是如何工作的。\]
 
-------

# Writing an Interface for Grbl \[为Grbl编写接口\]

The general interface for Grbl has been described above, but what's missing is how to run an entire G-code program on Grbl, when it doesn't seem to have an upload feature. This is where this section fits in. Early on, users fiercely requested for flash drive, external RAM, LCD support, joysticks, or network support so they can upload a g-code program and run it directly on Grbl. The general answer to that is, good ideas, but Grbl doesn't need them. Grbl already has nearly all of the tools and features to reliably communicate with a graphical user interface (GUI) or a separate host interface that provides all those extra bells and whistles. Grbl's base philosophy is to minimize what Grbl should be doing, because, in the end, Grbl needs to be concentrating on producing clean, reliable motion. That's it.

 \[Grbl的通用接口已经在上面描述过，但是缺少的是如何在Grbl上运行整个G代码程序，而它似乎没有上传功能。这就是本节的内容。早期，用户强烈要求闪存驱动器、外部RAM、LCD支持、操纵杆或网络支持，以便上传g代码程序并直接在Grbl上运行。一般的答案是，好主意，但Grbl不需要它们。Grbl已经拥有几乎所有的工具和特性，可以与图形用户界面（GUI）或提供所有额外提示的独立主机界面进行可靠通信。Grbl的基本理念是尽量减少Grbl应该做的事情，因为Grbl最终需要专注于产生干净、可靠的运动。就这样。\]
 

## Streaming a G-Code Program to Grbl \[将G代码程序流式传输到Grbl\]

Here we will describe two different streaming methods for Grbl GUIs. One of the main problems with streaming to Grbl is the USB port itself. Arduinos and most all micro controllers use a USB-to-serial converter chip that, at times, behaves strangely and not typically how you'd expect, like USB packet buffering and delays that can wreak havoc to a streaming protocol. Another problem is how to deal with some of the latency and oddities of the PCs themselves, because none of them are truly real-time and always create micro-delays when executing other tasks. Regardless, we've come up with ways to ensure the G-code stream is reliable and simple. 

 \[这里我们将描述Grbl GUI的两种不同的流方法。流式传输到Grbl的主要问题之一是USB端口本身。Arduinos和大多数微控制器都使用USB到串行转换器芯片，这种芯片有时会表现出奇怪的行为，而不是通常您所期望的那样，比如USB数据包缓冲和延迟，它们会对流协议造成严重破坏。另一个问题是如何处理PC机本身的一些延迟和奇怪之处，因为它们都不是真正实时的，并且在执行其他任务时总是产生微延迟。无论如何，我们已经找到了确保G代码流可靠且简单的方法。\]
 
The following streaming protocols require tracking the **response messages** to determine when to send the next g-code line. All **push messages** are not counted toward the streaming protocol and should be handled separately. All real-time command characters can be sent at any time and are never placed in Grbl's RX serial buffer. They are intercepted as they come in and simply sets  flags for Grbl to execute them.

 \[以下流协议要求跟踪**响应消息**，以确定何时发送下一个g代码行。所有**推送消息**不计入流媒体协议，应单独处理。所有实时命令字符都可以随时发送，并且永远不会放在Grbl的RX串行缓冲区中。它们进来时被截获，只是为Grbl设置执行它们的标志。\]
 
#### Streaming Protocol: Simple Send-Response _[Recommended]_ \[\]
The send-response streaming protocol is the most fool-proof and simplest method to stream a G-code program to Grbl. The host PC interface simply sends a line of G-code to Grbl and waits for an `ok` or `error:` **response message** before sending the next line of G-code. So, no matter if Grbl needs to wait for room in the look-ahead planner buffer to finish parsing and executing the last line of G-code or if the host computer is busy doing something, this guarantees both to the host PC and Grbl, the programmed G-code has been sent and received properly. An example of this protocol is published in our `simple_stream.py` script in our repository.

 \[发送响应流协议是将G代码程序流式传输到Grbl的最简单、最简单的方法。主机PC接口只需向Grbl发送一行G代码，并在发送下一行G代码之前等待`确定`或`错误：`**响应消息**`。因此，无论Grbl是否需要等待前瞻计划缓冲区中的空间来完成G代码的最后一行的解析和执行，或者如果主机正在忙着做一些事情，这都保证了已编程的G代码已正确发送和接收到主机PC和Grbl。该协议的一个示例发布在我们存储库中的`simple_stream.py`脚本中。\]
 
However, it's also the slowest of three outlined streaming protocols. Grbl essentially has two buffers between the execution of steps and the host PC interface. One of them is the serial receive buffer. This briefly stores up to 128 characters of data received from the host PC until Grbl has time to fetch and parse the line of G-code. The other buffer is the look-ahead planner buffer. This buffer stores up to 16 line motions that are acceleration-planned and optimized for step execution. Since the send-response protocol receives a line of G-code while the host PC waits for a response, Grbl's serial receive buffer is usually empty and under-utilized. If Grbl is actively running and executing steps, Grbl will immediately begin to execute and empty the look-ahead planner buffer, while it sends the response to the host PC, waits for the next line from the host PC, upon receiving it, parse and plan it, and add it to the end of the look-ahead buffer.

 \[然而，它也是三种流媒体协议中最慢的一种。Grbl在执行步骤和主机PC接口之间基本上有两个缓冲区。其中之一是串行接收缓冲器。这将短暂存储从主机PC接收的多达128个字符的数据，直到Grbl有时间提取和解析G代码行。另一个缓冲区是前瞻计划缓冲区。该缓冲区最多可存储16条直线运动，这些直线运动是针对步进执行而规划和优化的。由于发送响应协议在主机PC等待响应时接收一行G代码，因此Grbl的串行接收缓冲区通常为空且未充分利用。如果Grbl正在积极运行和执行步骤，Grbl将立即开始执行并清空前瞻计划器缓冲区，同时将响应发送到主机PC，等待主机PC的下一行，在接收到响应后，对其进行解析和计划，并将其添加到前瞻缓冲区的末尾。\]
 
Although this communication lag may take only a fraction of a second, there is a cumulative effect, because there is a lag with every G-code block sent to Grbl. In certain scenarios, like a G-code program containing lots of sequential, very short, line segments with high feed rates, the cumulative lag can be large enough to empty and starve the look-ahead planner buffer within this time. This could lead to start-stop motion when the streaming can't keep up with G-code program execution. Also, since Grbl can only plan and optimize what's in the look-ahead planner buffer, the performance through these types of motions will never be full-speed, because look-ahead buffer will always be partially full when using this streaming method. If your expected application doesn't contain a lot of these short line segments with high feed rates, this streaming protocol should be more than adequate for a vast majority of applications, is very robust, and is a quick way to get started.

 \[尽管这种通信延迟可能只需要几分之一秒，但存在累积效应，因为发送到Grbl的每个G代码块都有延迟。在某些情况下，例如G代码程序包含大量连续的、非常短的、具有高进给率的线段，累积滞后可能会大到足以在这段时间内清空并耗尽前瞻计划器缓冲区。当流媒体无法跟上G代码程序的执行时，这可能会导致启动-停止运动。此外，由于Grbl只能规划和优化前瞻计划器缓冲区中的内容，因此这些类型的运动的性能永远不会达到全速，因为使用此流方法时，前瞻缓冲区将始终部分满。如果您预期的应用程序不包含大量具有高馈送率的短线段，那么此流式传输协议应该足以满足绝大多数应用程序的需要，非常健壮，并且是一种快速入门的方法。\]
 
#### Streaming Protocol: Character-Counting _[**Recommended with Reservation**]_ \[流媒体协议：字符计数[**建议保留**]_\]

To get the best of both worlds, the simplicity and reliability of the send-response method and assurance of maximum performance with software flow control, we came up with a simple character-counting protocol for streaming a G-code program to Grbl. It works like the send-response method, where the host PC sends a line of G-code for Grbl to execute and waits for a `response message`, but, rather than needing special XON/XOFF characters for flow control, this protocol simply uses Grbl's responses as a way to reliably track how much room there is in Grbl's serial receive buffer. An example of this protocol is outlined in the `stream.py` streaming script in our repo. This protocol is particular useful for very fast machines like laser cutters. 

 \[为了充分利用这两个方面，即发送-响应方法的简单性和可靠性以及软件流控制的最大性能保证，我们提出了一个简单的字符计数协议，用于将G代码程序流式传输到Grbl。它的工作原理与send-response方法类似，主机PC发送一行G代码供Grbl执行并等待`response message`，但是，该协议不需要特殊的XON/XOFF字符来进行流量控制，而是简单地使用Grbl的响应作为可靠跟踪Grbl串行接收缓冲区中有多少空间的方法。我们的repo中的`stream.py`流脚本概述了该协议的一个示例。该协议对于激光切割机等速度非常快的机器特别有用。\]
 
The main difference between this protocol and the others is the host PC needs to maintain a standing count of how many characters it has sent to Grbl and then subtract the number of characters corresponding to the line executed with each Grbl response. Suppose there is a short G-code program that has 5 lines with 25, 40, 31, 58, and 20 characters (counting the line feed and carriage return characters too). We know Grbl has a 128 character serial receive buffer, and the host PC can send up to 128 characters without overflowing the buffer. If we let the host PC send as many complete lines as we can without over flowing Grbl's serial receive buffer, the first three lines of 25, 40, and 31 characters can be sent for a total of 96 characters. When Grbl sends a **response message**, we know the first line has been processed and is no longer in the serial read buffer. As it stands, the serial read buffer now has the 40 and 31 character lines in it for a total of 71 characters. The host PC needs to then determine if it's safe to send the next line without overflowing the buffer. With the next line at 58 characters and the serial buffer at 71 for a total of 129 characters, the host PC will need to wait until more room has cleared from the serial buffer. When the next Grbl **response message** comes in, the second line has been processed and only the third 31 character line remains in the serial buffer. At this point, it's safe to send the remaining last two 58 and 20 character lines of the g-code program for a total of 110.

 \[此协议与其他协议之间的主要区别在于，主机PC需要保持其发送到Grbl的字符数的固定计数，然后减去与每个Grbl响应执行的行对应的字符数。假设有一个简短的G代码程序，它有5行25、40、31、58和20个字符（也包括换行符和回车符）。我们知道Grbl有一个128个字符的串行接收缓冲区，主机PC最多可以发送128个字符而不会使缓冲区溢出。如果我们让主机PC发送尽可能多的完整行，而不超过Grbl的串行接收缓冲区，那么前三行25、40和31个字符的行可以发送总共96个字符。当Grbl发送**响应消息**时，我们知道第一行已被处理，不再在串行读取缓冲区中。目前，串行读取缓冲区中有40和31个字符行，总共有71个字符。然后，主机PC需要确定在不溢出缓冲区的情况下发送下一行是否安全。下一行为58个字符，串行缓冲区为71个字符，总共129个字符，主机PC需要等待，直到串行缓冲区腾出更多空间。当下一条Grbl**响应消息**进入时，第二行已被处理，串行缓冲区中只剩下第三行31个字符。此时，可以安全地发送g代码程序的最后两行58和20个字符，总共110行。\]
 
While seemingly complicated, this character-counting streaming protocol is extremely effective in practice. It always ensures Grbl's serial read buffer is filled, while never overflowing it. It maximizes Grbl's performance by keeping the look-ahead planner buffer full by better utilizing the bi-directional data flow of the serial port, and it's fairly simple to implement as our `stream.py` script illustrates. We have stress-tested this character-counting protocol to extremes and it has not yet failed. Seemingly, only the speed of the serial connection is the limit.

 \[虽然看似复杂，但这种字符计数流协议在实践中非常有效。它始终确保Grbl的串行读取缓冲区被填满，而不会使其溢出。它通过更好地利用串行端口的双向数据流来保持前瞻规划器缓冲区的满度，从而最大限度地提高了Grbl的性能，而且它的实现非常简单，正如我们的'stream.py'脚本所示。我们已经对这个字符计数协议进行了极端的压力测试，它还没有失败。看起来，只有串行连接的速度才是极限。\]
 
_RESERVATION:_

 \[_预约：_\]
 
- _If a g-code line is parsed and generates an error **response message**, a GUI should stop the stream immediately. However, since the character-counting method stuffs Grbl's RX buffer, Grbl will continue reading from the RX buffer and parse and execute the commands inside it. A GUI won't be able to control this. The interim solution is to check all of the g-code via the $C check mode, so all errors are vetted prior to streaming. This will get resolved in later versions of Grbl._
-  \[由于字符计数方法填充Grbl的RX缓冲区，Grbl将继续从RX缓冲区读取并解析和执行其中的命令。GUI将无法控制这一点。临时解决方案是通过$C检查模式检查所有g代码，因此在流媒体之前检查所有错误。这将在Grbl的后续版本中得到解决_
#### XON/XOFF Flow control \[XON/XOFF流量控制\]

XON/XOFF flow control proved to be problematic and did not work at all on boards using the Atmel 8U2 and 16U2 USB converter chips. As such XON/XOFF flow control cannot be used on UNO or Mega 2560 platforms using these chips. For this reason the XON/XOFF flow control was removed from the code.

 \[XON/XOFF流量控制被证明是有问题的，在使用Atmel 8U2和16U2 USB转换器芯片的板上根本不起作用。因此，XON/XOFF流量控制无法在使用这些芯片的UNO或Mega 2560平台上使用。因此，代码中删除了XON/XOFF流控制。\]

## Interacting with Grbl's Systems \[与Grbl的系统交互\]

Along with streaming a G-code program, there a few more things to consider when writing a GUI for Grbl, such as how to use status reporting, real-time control commands, dealing with EEPROM, and general message handling.

 \[随着G-代码程序的流化，在为Grbl编写GUI时需要考虑更多的事情，例如如何使用状态报告、实时控制命令、处理EEPROM和一般的消息处理。\]
 
#### Status Reporting \[状态报告\]
When a `?` character is sent to Grbl (no additional line feed or carriage return character required), it will immediately (exception: while homing) respond with something like `<Idle|MPos:0.000,0.000,0.000|FS:0.0,0>` to report its state and current position. The `?` is always picked-off and removed from the serial receive buffer whenever Grbl detects one. So, these can be sent at any time. Also, to make it a little easier for GUIs to pick up on status reports, they are always encased by `<>` chevrons.

 \[当一个`?`字符被发送到Grbl（不需要额外的换行符或回车符）时，它将立即（异常：在归位时）响应类似于`<Idle | MPos:0.000,0.000 | FS:0.0,0>  `的内容以报告其状态和当前位置。每当Grbl检测到`?`时，总是从串行接收缓冲区中取出并移除它。所以，这些可以在任何时候发送。此外，为了让GUI更容易获取状态报告，它们总是被`<>`字形包围。\]
 
Developers can use this data to provide an on-screen position digital-read-out (DRO) for the user and/or to show the user a 3D position in a virtual workspace. We recommend querying Grbl for a `?` real-time status report at no more than 5Hz. 10Hz may be possible, but at some point, there are diminishing returns and you are taxing Grbl's CPU more by asking it to generate and send a lot of position data.

 \[开发人员可以使用这些数据为用户提供屏幕位置数字读数（DRO）和/或向用户显示虚拟工作区中的3D位置。我们建议以不超过5Hz的频率查询Grbl以获取`实时状态报告`。10Hz可能是可能的，但在某一点上，回报是递减的，通过要求Grbl生成并发送大量的位置数据，您会对其CPU造成更大的负担。\]
 
Grbl's status report is fairly simple in organization. It always starts with a word describing the machine state like `IDLE` (descriptions of these are available elsewhere in the Wiki). The following data values are usually in the order listed below and separated by `|` pipe characters, but may not be in the exact order or printed at all. For a complete description of status report formatting, read the _Real-time Status Reports_ section below.

 \[Grbl的状态报告在组织上相当简单。它总是以一个描述机器状态的单词开头，比如 `IDLE`（这些描述在Wiki的其他地方可以找到）。以下数据值通常按下面列出的顺序排列，并用`| `管道字符分隔，但可能完全不按顺序排列或打印。有关状态报告格式的完整说明，请阅读下面的`实时状态报告`部分。\]
 
#### Real-Time Control Commands \[实时控制命令\]
The real-time control commands, `~` cycle start/resume, `!` feed hold,  `^X` soft-reset, and all of the override commands, all immediately signal Grbl to change its running state. Just like `?` status reports, these control characters are picked-off and removed from the serial buffer when they are detected and do not require an additional line-feed or carriage-return character to operate.

 \[实时控制命令，`~`循环启动/恢复，`！`进给保持、`^X`软复位和所有超控命令都会立即向Grbl发出信号，以改变其运行状态。与`？`状态报告一样，当检测到这些控制字符时，这些控制字符会从串行缓冲区中提取和删除，并且不需要额外的换行符或回车符来操作。\]
 
One important note are the override command characters. These are defined in the extended-ASCII character space and are generally not type-able on a keyboard. A GUI must be able to send these 8-bit values to support overrides. 

 \[\]
 
#### EEPROM Issues \[EEPROM问题\]
EEPROM access on the Arduino AVR CPUs turns off all of the interrupts while the CPU _writes_ to EEPROM. This poses a problem for certain features in Grbl, particularly if a user is streaming and running a g-code program, since it can pause the main step generator interrupt from executing on time. Most of the EEPROM access is restricted by Grbl when it's in certain states, but there are some things that developers need to know.

 \[Arduino AVR CPU上的EEPROM访问在CPU写入EEPROM时关闭所有中断。这给Grbl中的某些功能带来了问题，特别是当用户正在流式传输和运行g代码程序时，因为它可能会暂停主步进生成器中断，使其无法按时执行。当处于某些状态时，大多数EEPROM访问都受到Grbl的限制，但开发人员需要知道一些事情。\]
 
* Settings should not be streamed with the character-counting streaming protocols. Only the simple send-response protocol works. This is because during the EEPROM write, the AVR CPU also shuts-down the serial RX interrupt, which means data can get corrupted or lost. This is safe with the send-response protocol, because it's not sending data after commanding Grbl to save data.
*  \[设置不应与字符计数流协议流式传输。只有简单的发送-响应协议起作用。这是因为在EEPROM写入期间，AVR CPU还关闭串行RX中断，这意味着数据可能会损坏或丢失。这对于发送响应协议是安全的，因为它在命令Grbl保存数据后不会发送数据。\]
For reference:
* Grbl's EEPROM write commands: `G10 L2`, `G10 L20`, `G28.1`, `G30.1`, `$x=`, `$I=`, `$Nx=`, `$RST=`
*  \[Grbl的EEPROM写入命令：`G10 L2`、`G10 L20`、`G28.1`、`G30.1`、`$x=`、`I=`、`Nx=`、`RST$=`\]
* Grbl's EEPROM read commands: `G54-G59`, `G28`, `G30`, `$$`, `$I`, `$N`, `$#`
*  \[Grbl的EEPROM读取命令：`G54-G59`、`G28`、`G30`、`$$`、`I`、`N`、`$#`\]
#### G-code Error Handling \[G代码错误处理\]

Grbl's g-code parser is fully standards-compliant with complete error-checking. When a G-code parser detects an error in a G-code block/line, the parser will dump everything in the block from memory and report an `error:` back to the user or GUI. This dump is absolutely the right thing to do, because a g-code line with an error can be interpreted in multiple ways. However, this dump can be problematic, because the bad G-code block may have contained some valuable positioning commands or feed rate settings that the following g-code depends on.

 \[Grbl的g代码解析器完全符合标准，具有完整的错误检查功能。当G代码解析器检测到G代码块/行中的错误时，解析器将从内存中转储该块中的所有内容，并向用户或GUI报告`error:`。这种转储绝对是正确的，因为带有错误的g代码行可以用多种方式解释。但是，此转储可能会有问题，因为坏G代码块可能包含一些有价值的定位命令或进给速度设置，这些设置是以下G代码所依赖的。\]
 
It's highly recommended to do what all professional CNC controllers do when they detect an error in the G-code program, _**halt**_. Don't do anything further until the user has modified the G-code and fixed the error in their program. Otherwise, bad things could happen.

 \[强烈建议所有专业CNC控制器在检测到G代码程序中的错误时，_**halt**_ 。执行所有专业CNC控制器所执行的操作。在用户修改G代码并修复程序中的错误之前，不要做任何进一步的操作。否则，坏事可能会发生。\]
 
As a service to GUIs, Grbl has a `check G-code` mode, enabled by the `$C` system command. GUIs can stream a G-code program to Grbl, where it will parse it, error-check it, and report `ok`'s and `errors:`'s without powering on anything or moving. So GUIs can pre-check the programs before streaming them for real. To disable the `check G-code` mode, send another `$C` system command and Grbl will automatically soft-reset to flush and re-initialize the G-code parser and the rest of the system. This perhaps should be run in the background when a user first loads a program, before a user sets up his machine. This flushing and re-initialization clears `G92`'s by G-code standard, which some users still incorrectly use to set their part zero.

 \作为GUI的一项服务，Grbl具有`检查G代码`模式，由`$C`系统命令启用。GUI可以将一个G代码程序流式传输到Grbl，在Grbl中它将解析它、检查它的错误并报告`ok`和`errors:`，而无需通电或移动。因此GUI可以在流式传输程序之前对其进行预检查。要禁用`检查G代码`模式，请发送另一个`$C`系统命令，Grbl将自动软重置以刷新并重新初始化G代码解析器和系统的其余部分。这可能应该在用户第一次加载程序时，在用户设置机器之前，在后台运行。此刷新和重新初始化通过G代码标准清除`G92`，一些用户仍然错误地使用G代码标准设置其零部。[\]
 
#### Jogging \[慢跑\]

As of Grbl v1.1, a new jogging feature is available that accepts incremental, absolute, or absolute override motions, along with a jog cancel real-time command that will automatically feed hold and purge the planner buffer. The most important aspect of the new jogging motion is that it is completely independent from the g-code parser, so GUIs no longer have to ensure the g-code modal states are set back correctly after jogging is complete. See the jogging document for more details on how it works and how you can use it with an analog joystick or rotary dial.

 \[从Grbl v1.1开始，新的点动功能可用，可接受增量、绝对或绝对覆盖运动，以及点动取消实时命令，该命令将自动进给保持并清除planner缓冲区。新的点动运动最重要的方面是，它完全独立于g代码解析器，因此GUI不再需要确保在点动完成后正确设置g代码模式状态。有关其工作原理以及如何将其与模拟操纵杆或转盘配合使用的更多详细信息，请参阅慢跑文档。\]
 
#### Synchronization\[同步\]

For situations when a GUI needs to run a special set of commands for tool changes, auto-leveling, etc, there often needs to be a way to know when Grbl has completed a task and the planner buffer is empty. The absolute simplest way to do this is to insert a `G4 P0.01` dwell command, where P is in seconds and must be greater than 0.0. This acts as a quick force-synchronization and ensures the planner buffer is completely empty before the GUI sends the next task to execute.

 \[对于GUI需要运行一组用于工具更改、自动调平等的特殊命令的情况，通常需要有一种方法来知道Grbl何时已完成任务且planner缓冲区为空。最简单的方法是插入一个'G4 P0.01`dwell命令，其中P以秒为单位，必须大于0.0。这起到快速强制同步的作用，并确保在GUI发送下一个要执行的任务之前，planner缓冲区完全为空。\]
 
-----
# Message Summary\[消息摘要\]

In v1.1, Grbl's interface protocol has been tweaked in the attempt to make GUI development cleaner, clearer, and hopefully easier. All messages are designed to be deterministic without needing to know the context of the message. Each can be inferred to a much greater degree than before just by the message type, which are all listed below.

 \[在v1.1中，Grbl的接口协议经过了调整，试图使GUI开发更干净、更清晰，并且希望更容易。所有消息都被设计为确定性的，而不需要知道消息的上下文。仅通过下面列出的消息类型，就可以在比以前更大的程度上推断每个消息。\]
 
- **Response Messages:** Normal send command and execution response acknowledgement. Used for streaming.
- \[**响应消息：**正常发送命令和执行响应确认。用于流媒体。\]

	- `ok` : Indicates the command line received was parsed and executed (or set to be executed).
  - \[`ok`：表示已解析并执行（或设置为执行）接收的命令行。\]
	- `error:x` : Indicated the command line received contained an error, with an error code `x`, and was purged. See error code section below for definitions.
  - \[`error：x`：表示收到的命令行包含错误，错误代码为'x`，已清除。有关定义，请参见下面的错误代码部分。\]

- **Push Messages:**
- \[**推送消息：**\]

	- `< >` : Enclosed chevrons contains status report data.
  - \[`<>`：封闭的V形符号包含状态报告数据。\]
  - `Grbl X.Xx ['$' for help]` : Welcome message indicates initialization.
  - \[`Grbl X.Xx['$'获取帮助]`：欢迎消息表示初始化。\]
  - `ALARM:x` : Indicates an alarm has been thrown. Grbl is now in an alarm state.
  - \[`ALARM：x`：表示已引发报警。Grbl现在处于报警状态。\]	
  - `$x=val` and `$Nx=line` indicate a settings printout from a `$` and `$N` user query, respectively.
  - \[`$x=val`和`$Nx=line`分别表示从`$`和`$N`用户查询打印出的设置。\]
  - `[MSG:]` : Indicates a non-queried feedback message.
  - \[`[MSG:]`：表示未查询的反馈消息。\]
  - `[GC:]` : Indicates a queried `$G` g-code state message.
  - \[`[GC:]`：表示查询到的`$G`G代码状态消息。\]
  - `[HLP:]` : Indicates the help message.
  - \[`[HLP:]`：表示帮助消息。\]
  - `[G54:]`, `[G55:]`, `[G56:]`, `[G57:]`, `[G58:]`, `[G59:]`, `[G28:]`, `[G30:]`, `[G92:]`, `[TLO:]`, and `[PRB:]` messages indicate the parameter data printout from a `$#` user query.
  - \[`[G54:]`, `[G55:]`, `[G56:]`, `[G57:]`, `[G58:]`, `[G59:]`, `[G28:]`, `[G30:]`, `[G92:]`, `[TLO:]`, and `[PRB:]`消息表示从`$#`用户查询打印出的参数数据。\]
  - `[VER:]` : Indicates build info and string from a `$I` user query.
  - \[`[VER:]`:表示生成信息和来自`$I`用户查询的字符串。\]
  - `[echo:]` : Indicates an automated line echo from a pre-parsed string prior to g-code parsing. Enabled by config.h option.
  - \[`[VER:]`:表示生成信息和来自`$I`用户查询的字符串。\]
  - `>G54G20:ok` : The open chevron indicates startup line execution. The `:ok` suffix shows it executed correctly without adding an unmatched `ok` response on a new line.
  - \[`>G54G20:ok`：打开的V形标志表示启动行执行。`：ok`后缀显示它正确执行，而不在新行中添加不匹配的`ok`响应。\]

In addition, all `$x=val` settings, `error:`, and `ALARM:` messages no longer contain human-readable strings, but rather codes that are defined in other documents. The `$` help message is also reduced to just showing the available commands. Doing this saves incredible amounts of flash space. Otherwise, the new overrides features would not have fit.

 \[此外，所有`$x=val`设置、`error:`和`ALARM:`消息不再包含人类可读的字符串，而是包含在其他文档中定义的代码。`$`帮助消息也简化为仅显示可用命令。这样做可以节省大量的闪存空间。否则，新的覆盖功能将不适用。\]

Other minor changes and bug fixes that may effect GUI parsing include:

 \[可能影响GUI解析的其他小更改和错误修复包括：\]
 
- Floating point values printed with zero precision do not show a decimal, or look like an integer. This includes spindle speed RPM and feed rate in mm mode.
- \[以零精度打印的浮点值不显示十进制或看起来像整数。这包括毫米模式下的主轴转速和进给速度。\]
- `$G` reports fixed a long time bug with program modal state. It always showed `M0` program pause when running. Now during a normal program run, no program modal state is given until an `M0`, `M2`, or `M30` is active and then the appropriate state will be shown.
- \[`$G`报告修复了程序模式状态的长时间错误。运行时总是显示'M0'程序暂停。现在，在正常程序运行期间，在`M0`、`M2`或`M30`处于活动状态并显示相应状态之前，不会给出任何程序模式状态。\]

On a final note, this interface tweak came about out of necessity, as more data is being sent back from Grbl and it is capable of doing many more things. It's not intended to be altered again in the near future, if at all. This is likely the only and last major change to this. If you have any comments or suggestions before Grbl v1.1 goes to master, please do immediately so we can all vet the new alteration before its installed.

 \[最后一点，这个接口调整是出于必要的，因为更多的数据从Grbl发送回来，它能够做更多的事情。它不打算在不久的将来再次被修改，如果有的话。这可能是唯一也是最后一次重大改变。如果您在GRBLV1.1版上主版之前有任何意见或建议，请立即提出，以便我们在安装新版本之前对其进行审查。\]
 




---------

# Grbl Response Messages \[Grbl响应消息\]

Every G-code block sent to Grbl and Grbl `$` system command that is terminated with a return will be parsed and processed by Grbl. Grbl will then respond either if it recognized the command with an `ok` line or if there was a problem with an `error` line.

 \[Grbl将解析和处理发送到Grbl和Grbl`$`系统命令的每个G代码块，这些命令以返回结束。如果Grbl识别出带有`ok`行的命令，或者`error`行有问题，Grbl将作出响应。\]
 
* **`ok`**: All is good! Everything in the last line was understood by Grbl and was successfully processed and executed.

* \[**`OK`**：一切都很好！Grbl理解了最后一行中的所有内容，并成功地处理和执行了这些内容。\]
 
  - If an empty line with only a return is sent to Grbl, it considers it a valid line and will return an `ok` too, except it didn't do anything.
  - \[如果向Grbl发送一个只有返回的空行，它会认为它是一个有效行，并且也会返回`ok`，除非它没有做任何操作。\]
 

* **`error:X`**: Something went wrong! Grbl did not recognize the command and did not execute anything inside that message. The `X` is given as a numeric error code to tell you exactly what happened. The table below decribes every one of them.

* \[**`error:X`**:出了点问题！Grbl无法识别该命令，并且没有执行该消息中的任何内容。`X`是一个数字错误代码，用于准确地告诉您发生了什么。下表列出了每一项。\]
 

| ID | Error Code Description \[错误代码说明\]|
|:-------------:|----|
| **`1`** | G-code words consist of a letter and a value. Letter was not found. \[G代码字由一个字母和一个值组成。找不到这封信。\] |
| **`2`** | Numeric value format is not valid or missing an expected value. \[数值格式无效或缺少预期值。\] |
| **`3`** | Grbl '$' system command was not recognized or supported. \[无法识别或支持Grbl`$`系统命令。\] |
| **`4`** | Negative value received for an expected positive value. \[收到预期正值的负值。\] |
| **`5`** | Homing cycle is not enabled via settings. \[未通过设置启用归位循环。\] |
| **`6`** | Minimum step pulse time must be greater than 3usec  \[最小步进脉冲时间必须大于3usec\]|
| **`7`** | EEPROM read failed. Reset and restored to default values. \[EEPROM读取失败。重置并恢复为默认值。\] |
| **`8`** | Grbl '$' command cannot be used unless Grbl is IDLE. Ensures smooth operation during a job. \[除非Grbl处于空闲状态，否则无法使用Grbl`$`命令。确保作业过程中的平稳运行。\] |
| **`9`** | G-code locked out during alarm or jog state \[报警或点动状态期间G代码锁定\] |
| **`10`** | Soft limits cannot be enabled without homing also enabled. \[如果未启用归位，则无法启用软限制。\] |
| **`11`** | Max characters per line exceeded. Line was not processed and executed. \[超出了每行的最大字符数。行未被处理和执行。\] |
| **`12`** | (Compile Option) Grbl '$' setting value exceeds the maximum step rate supported. \[（编译选项）Grbl`$`设置值超过了支持的最大步进速率。\] |
| **`13`** | Safety door detected as opened and door state initiated. \[检测到安全门已打开且门状态已启动。\] |
| **`14`** | (Grbl-Mega Only) Build info or startup line exceeded EEPROM line length limit. \[（仅限Grbl Mega）生成信息或启动线路超出EEPROM线路长度限制。\] |
| **`15`** | Jog target exceeds machine travel. Command ignored. \[点动目标超出机器行程。命令被忽略。\] |
| **`16`** | Jog command with no '=' or contains prohibited g-code.  \[不带`=`或包含禁止g代码的Jog命令。\]|
| **`17`** | Laser mode requires PWM output.  \[激光模式需要PWM输出。\]|
| **`20`** | Unsupported or invalid g-code command found in block. \[在块中找到不受支持或无效的g代码命令。\] |
| **`21`** | More than one g-code command from same modal group found in block. \[在块中找到来自同一模态组的多个g代码命令。\]|
| **`22`** | Feed rate has not yet been set or is undefined. \[进给速度尚未设置或未定义。\] |
| **`23`** | G-code command in block requires an integer value. \[块中的G代码命令需要一个整数值。\] |
| **`24`** | Two G-code commands that both require the use of the `XYZ` axis words were detected in the block. \[在块中检测到两个G代码命令，它们都需要使用`XYZ`轴字。\]|
| **`25`** | A G-code word was repeated in the block. \[块中重复了一个G代码字。\]|
| **`26`** | A G-code command implicitly or explicitly requires `XYZ` axis words in the block, but none were detected. \[G代码命令隐式或显式地要求块中有`XYZ`轴字，但未检测到任何字。\]|
| **`27`**| `N` line number value is not within the valid range of `1` - `9,999,999`.  \[`N`行号值不在'1'-'999999'的有效范围内。\]|
| **`28`** | A G-code command was sent, but is missing some required `P` or `L` value words in the line.  \[已发送G代码命令，但行中缺少一些必需的'P'或'L'值字。\]|
| **`29`** | Grbl supports six work coordinate systems `G54-G59`， `G59.1`, `G59.2`, and `G59.3` are not supported. \[Grbl支持六个工作坐标系`G54-G59`，`不支持G59.1`、`G59.2`和`G59.3`。\]|
| **`30`**| The `G53` G-code command requires either a `G0` seek or `G1` feed motion mode to be active. A different motion was active. \[`G53`G-code命令要求激活`G0`寻道或`G1`进给运动模式。一个不同的动议被激活。\]|
| **`31`** | There are unused axis words in the block and `G80` motion mode cancel is active. \[块中有未使用的轴字，`G80`运动模式取消处于活动状态。\]|
| **`32`** | A `G2` or `G3` arc was commanded but there are no `XYZ` axis words in the selected plane to trace the arc. \[命令了`G2`或`G3`弧，但所选平面中没有`XYZ`轴字来跟踪弧。\]|
| **`33`** | The motion command has an invalid target. `G2`, `G3`, and `G38.2` generates this error, if the arc is impossible to generate or if the probe target is the current position. \[`运动`命令的目标无效`G2`、`G3`和`G38.2`如果无法生成电弧，或者如果探针目标是当前位置，则会生成此错误。\]|
| **`34`** | A `G2` or `G3` arc, traced with the radius definition, had a mathematical error when computing the arc geometry. Try either breaking up the arc into semi-circles or quadrants, or redefine them with the arc offset definition. \[使用半径定义跟踪的`G2`或`G3`圆弧在计算圆弧几何图形时存在数学错误。尝试将圆弧拆分为半圆或象限，或使用圆弧偏移定义重新定义它们。\]|
| **`35`** | A `G2` or `G3` arc, traced with the offset definition, is missing the `IJK` offset word in the selected plane to trace the arc. \[使用偏移定义跟踪的`G2`或`G3`弧在所选平面中缺少`IJK`偏移字以跟踪弧。\]|
| **`36`** | There are unused, leftover G-code words that aren't used by any command in the block. \[块中的任何命令都不使用未使用的、剩余的G代码字。\]|
| **`37`** | The `G43.1` dynamic tool length offset command cannot apply an offset to an axis other than its configured axis. The Grbl default axis is the Z-axis. \[`G43.1`动态刀具长度偏移命令不能将偏移应用于配置轴以外的轴。Grbl默认轴为Z轴。\]|
| **`38`** | Tool number greater than max supported value. \[刀具编号大于支持的最大值。\]|

----------------------

# Grbl Push Messages \[Grbl推送消息\]

Along with the response message to indicate successfully executing a line command sent to Grbl, Grbl provides additional push messages for important feedback of its current state or if something went horribly wrong. These messages are `pushed` from Grbl and may appear at anytime. They are usually in response to a user query or some system event that Grbl needs to tell you about immediately. These push messages are organized into six general classes:

 \[除了表示成功执行发送给Grbl的行命令的响应消息外，Grbl还提供了额外的推送消息，用于对其当前状态的重要反馈，或者如果出现了严重错误。这些消息是从Grbl`推送`的，可能随时出现。它们通常是对Grbl需要立即告诉您的用户查询或某些系统事件的响应。这些推送消息分为六个一般类：\]
- **_Welcome message_** - A unique message to indicate Grbl has initialized.
-  \[**_欢迎消息**-表示Grbl已初始化的唯一消息。\]
- **_ALARM messages_** - Means an emergency mode has been enacted and shut down normal use.
-  \[**_报警信息**-表示已启动紧急模式并关闭正常使用。\]
- **_'$' settings messages_** - Contains the type and data value for a Grbl setting.
-  \[**_`$`设置消息\u**-包含Grbl设置的类型和数据值。\]
- **_Feedback messages_** - Contains general feedback and can provide useful data.
-  \[**_反馈信息**-包含一般性反馈，可提供有用的数据。\]
- **_Startup line execution_** - Indicates a startup line as executed with the line itself and how it went.
-  \[**_（启动行执行）**-表示使用该行本身执行的启动行及其运行方式。\]
- **_Real-time status reports_** - Contains current run data like state, position, and speed.
-  \[**_实时状态报告**-包含当前运行数据，如状态、位置和速度。\]

------

#### Welcome Message  \[欢迎消息\]

**`Grbl X.Xx ['$' for help]`  \[`Grbl X.Xx[`$`以获取帮助]`\]**

The start up message always prints upon startup and after a reset. Whenever you see this message, this also means that Grbl has completed re-initializing all its systems, so everything starts out the same every time you use Grbl.

 \[启动消息总是在启动和重置后打印。每当您看到此消息时，这也意味着Grbl已完成对其所有系统的重新初始化，因此每次使用Grbl时，一切都是一样的。\]

* `X.Xx` indicates the major version number, followed by a minor version letter. The major version number indicates the general release, while the letter simply indicates a feature update or addition from the preceding minor version letter.
* \[`X.Xx`表示主要版本号，后跟次要版本字母。主要版本号表示一般版本，而字母仅表示功能更新或之前次要版本字母的添加。\]
* Bug fix revisions are tracked by the build info version number, printed when an `$I` command is sent. These revisions don't update the version number and are given by date revised in year, month, and day, like so `20161014`.
* \[Bug fix修订版由生成信息版本号跟踪，在发送`$I`命令时打印。这些修订不会更新版本号，而是按年、月、日的修订日期给出，如`20161014`。\]

-----

#### Alarm Message \[报警信息\]

Alarm is an emergency state. Something has gone terribly wrong when these occur. Typically, they are caused by limit error when the machine has moved or wants to move outside the machine travel and crash into the ends. They also report problems if Grbl is lost and can't guarantee positioning or a probe command has failed. Once in alarm-mode, Grbl will lock out all g-code functionality and accept only a small set of commands. It may even stop everything and force you to acknowledge the problem until you issue Grbl a reset. While in alarm-mode, the user can override the alarm manually with a specific command, which then re-enables g-code so you can move the machine again. This ensures the user knows about the problem and has taken steps to fix or account for it.

 \[警报是一种紧急状态。当这些事情发生时，有些事情已经出了严重的问题。通常，当机器已经移动或想要移动到机器行程之外并撞到末端时，它们是由极限误差引起的。如果Grbl丢失且无法保证定位或探测命令失败，他们也会报告问题。一旦进入报警模式，Grbl将锁定所有g代码功能，只接受一小部分命令。它甚至可能停止一切，迫使您确认问题，直到您发出Grbl重置。在报警模式下，用户可以使用特定命令手动覆盖报警，然后重新启用g代码，以便再次移动机器。这确保用户了解问题，并已采取措施修复或解决问题。\]

Similar to error messages, all alarm messages are sent as  **`ALARM:X`**, where `X` is an alarm code to tell you exacly what caused the alarm. The table below describes the meaning of each alarm code.

 \[与错误消息类似，所有报警消息都以 **`alarm:X`**的形式发送，其中`X`是一个报警代码，用于详细告知报警原因。下表描述了每个报警代码的含义。\]

| ID | Alarm Code Description \[报警代码说明\] |
|:-------------:|----|
| **`1`** | Hard limit triggered. Machine position is likely lost due to sudden and immediate halt. Re-homing is highly recommended.  \硬限制触发。机器位置可能因突然和立即停止而丢失。强烈建议重新归航。[\]|
| **`2`** | G-code motion target exceeds machine travel. Machine position safely retained. Alarm may be unlocked.  \[G代码运动目标超出机器行程。安全地保持机器位置。警报可以解锁。\]|
| **`3`** | Reset while in motion. Grbl cannot guarantee position. Lost steps are likely. Re-homing is highly recommended.  \[在运动中重置。Grbl不能保证位置。可能会迷失方向。强烈建议重新归航。\]|
| **`4`** | Probe fail. The probe is not in the expected initial state before starting probe cycle, where G38.2 and G38.3 is not triggered and G38.4 and G38.5 is triggered. \[探测失败。在开始探头循环之前，探头未处于预期初始状态，其中G38.2和G38.3未触发，G38.4和G38.5已触发。\] |
| **`5`** | Probe fail. Probe did not contact the workpiece within the programmed travel for G38.2 and G38.4.  \[探测失败。在G38.2和G38.4的编程行程内，探针未接触到工件。\]|
| **`6`** | Homing fail. Reset during active homing cycle.  \[归位失败。在主动回零循环中复位。\]|
| **`7`** | Homing fail. Safety door was opened during active homing cycle.  \[归位失败。安全门在主动归位循环期间打开。\]|
| **`8`** | Homing fail. Cycle failed to clear limit switch when pulling off. Try increasing pull-off setting or check wiring.  \[归位失败。拔出时，循环未能清除限位开关。尝试增加拔出设置或检查接线。\]|
| **`9`** | Homing fail. Could not find limit switch within search distance. Defined as `1.5 * max_travel` on search and `5 * pulloff` on locate phases. \[归位失败。在搜索距离内找不到限位开关。在搜索阶段定义为`1.5*max_travel`，在定位阶段定义为`5*pulloff`。\] |
| **`10`** | Homing fail. On dual axis machines, could not find the second limit switch for self-squaring.  \[归位失败。在双轴机器上，找不到第二个限位开关用于自动调平。\]|
-------

#### Grbl `$` Settings Message \[组`$`设置消息\]

When a push message starts with a `$`, this indicates Grbl is sending a setting and its configured value. There are only two types of settings messages: a single setting and value `$x=val` and a startup string setting `$Nx=line`. See [Configuring Grbl v1.x] document if you'd like to learn how to write these values for your machine.

\[当推送消息以`$`开头时，这表示Grbl正在发送设置及其配置值。只有两种类型的设置消息：单个设置和值`$x=val`和启动字符串设置`$Nx=line`。如果您想了解如何为您的机器编写这些值，请参阅[Configuring Grbl v1.x]文档。\]

- `$x=val` will only appear when the user queries to print all of Grbl's settings via the `$$` print settings command. It does so sequentially and completes with an `ok`.
- \`$x=val`仅当用户通过`$$`打印设置命令查询打印所有Grbl设置时才会出现。它按顺序执行，并以`ok`结束。[\]

  - In prior versions of Grbl, the `$` settings included a short description of the setting immediately after the value. However, due to flash restrictions, most human-readable strings were removed to free up flash for the new override features in Grbl v1.1. In short, it was these strings or overrides, and overrides won. Keep in mind that once these values are set, they usually don't change, and GUIs will likely provide the assistance of translating these codes for users.
  - \[在Grbl的早期版本中，`$`设置包括紧跟在值后面的设置的简短描述。然而，由于闪存限制，大多数人类可读的字符串被删除，以释放闪存，用于GRBLV1.1中的新覆盖功能。简言之，就是这些字符串或覆盖，覆盖获胜。请记住，一旦设置了这些值，它们通常不会更改，GUI可能会为用户提供翻译这些代码的帮助。\]
  - _**NOTE for GUI developers:**_ _As with the error and alarm codes, settings codes are available in an easy to parse CSV file in the `/doc/csv` folder. These are continually updated._
  - \[ **GUI开发人员注意：**.\u与错误代码和报警代码一样，设置代码位于`/doc/CSV`文件夹中易于解析的CSV文件中。这些都是不断更新的_ \]
  - The `$$` settings print out is shown below and the following describes each setting.
  - \[下面显示了`$$`设置打印输出，下面介绍了每个设置。\]

```
$0=10
$1=25
$2=0
$3=0
$4=0
$5=0
$6=0
$10=255
$11=0.010
$12=0.002
$13=0
$20=0
$21=0
$22=0
$23=0
$24=25.000
$25=500.000
$26=250
$27=1.000
$30=1000
$31=0
$32=0
$100=250.000
$101=250.000
$102=250.000
$110=500.000
$111=500.000
$112=500.000
$120=10.000
$121=10.000
$122=10.000
$130=200.000
$131=200.000
$132=200.000
ok
```

| `$x` Code \[`$x'代码\] | Setting Description, Units  \[设置说明，单位\]|
|:-------------:|----|
| **`0`** | Step pulse time, microseconds  \[步进脉冲时间，微秒\]|
| **`1`** | Step idle delay, milliseconds \[步进空闲延迟，毫秒\]|
| **`2`** | Step pulse invert, mask  \[阶跃脉冲反转，掩模\]|
| **`3`** | Step direction invert, mask  \[阶跃方向反转，掩码\]|
| **`4`** | Invert step enable pin, boolean  \[反转步进启用引脚，布尔值\]|
| **`5`** | Invert limit pins, boolean  \[反转限制引脚，布尔型\]|
| **`6`** | Invert probe pin, boolean  \[反向探针引脚，布尔型\]|
| **`10`** | Status report options, mask  \[状态报告选项，掩码\]|
| **`11`** | Junction deviation, millimeters  \[接头偏差，毫米\]|
| **`12`** | Arc tolerance, millimeters  \[圆弧公差，毫米\]|
| **`13`** | Report in inches, boolean  \[以英寸为单位报告，布尔值\]|
| **`20`** | Soft limits enable, boolean  \[启用软限制，布尔值\]|
| **`21`** | Hard limits enable, boolean  \[硬限制启用，布尔值\]|
| **`22`** | Homing cycle enable, boolean  \[复位循环启用，布尔值\]|
| **`23`** | Homing direction invert, mask  \归位方向反转，掩码[\]|
| **`24`** | Homing locate feed rate, mm/min  \[归位定位进给速度，mm/min\]|
| **`25`** | Homing search seek rate, mm/min   \[回零搜索寻道率，mm/min\]|
| **`26`** | Homing switch debounce delay, milliseconds  \[归位开关去抖动延迟，毫秒\]|
| **`27`** | Homing switch pull-off distance, millimeters  \[归位开关偏移距离，毫米\]|
| **`30`** | Maximum spindle speed, RPM  \[最大主轴转速，RPM\]|
| **`31`** | Minimum spindle speed, RPM  \[最小主轴转速\]|
| **`32`** | Laser-mode enable, boolean  \[激光模式启用，布尔值\]|
| **`100`** | X-axis steps per millimeter  \[X轴每毫米步数\]|
| **`101`** | Y-axis steps per millimeter  \[Y轴每毫米步数\]|
| **`102`** | Z-axis steps per millimeter  \[Z轴每毫米步数\]|
| **`110`** | X-axis maximum rate, mm/min  \[X轴最大速率，mm/min\]|
| **`111`** | Y-axis maximum rate, mm/min  \[Y轴最大速率，mm/min\]|
| **`112`** | Z-axis maximum rate, mm/min  \[Z轴最大速率，mm/min\]|
| **`120`** | X-axis acceleration, mm/sec^2  \[X轴加速度，毫米/秒^2\]|
| **`121`** | Y-axis acceleration, mm/sec^2  \[Y轴加速度，毫米/秒^2\]|
| **`122`** | Z-axis acceleration, mm/sec^2  \[Z轴加速度，毫米/秒^2\]|
| **`130`** | X-axis maximum travel, millimeters  \[X轴最大行程，毫米\]|
| **`131`** | Y-axis maximum travel, millimeters  \[Y轴最大行程，毫米\]|
| **`132`** | Z-axis maximum travel, millimeters  \[Z轴最大行程，毫米\]|


- The other `$Nx=line` message is the print-out of a user-defined startup line, where `x` denotes the startup line order and ranges from `0` to `1` by default. The `line` denotes the startup line to be executed by Grbl upon reset or power-up, except during an ALARM.
- \[另一条`$Nx=line`消息是用户定义的启动行的打印输出，其中`x`表示启动行顺序，默认范围为`0`到`1`。`行`表示Grbl在复位或通电时执行的启动行，报警期间除外。\]

  - When a user queries for the startup lines via a `$N` command, the following is sent by Grbl and completed by an `ok` response. The first line sets the initial startup work coordinate system to `G54`, while the second line is empty and does not execute.
  - \[当用户通过`$N`命令查询启动行时，Grbl将发送以下内容，并通过`ok`响应完成。第一行将初始启动工作坐标系设置为`G54`，而第二行为空且不执行。\]

```
$N0=G54
$N1=
ok
```


------

#### Feedback Messages \[反馈信息\]

Feedback messages provide non-critical information on what Grbl is doing, what it needs, and/or provide some non-real-time data for the user when queried. Not too complicated. Feedback message are always enclosed in `[]` brackets, except for the startup line execution message which begins with an open chevron character `>`.

 \[反馈消息提供有关Grbl正在做什么、需要什么的非关键信息，和/或在查询时为用户提供一些非实时数据。不太复杂。反馈消息始终包含在`[]`括号内，但启动行执行消息除外，该消息以开放的V形字符`>`开头。\]

- **Non-Queried Feedback Messages:** These feedback messages that may appear at any time and is not part of a query are listed and described below. They are usually sent as an additional helpful acknowledgement of some event or command executed. These always start with a `[MSG:` to denote their type.
- \[**未查询的反馈消息：**以下列出并描述了这些可能随时出现且不属于查询的反馈消息。它们通常作为对执行的某些事件或命令的额外有用确认发送。它们总是以`[MSG:`开头，表示它们的类型。\]

  - `[MSG:Reset to continue]` - Critical event message. Reset is required before Grbl accepts any other commands. This prevents ongoing command streaming and risking a motion before the alarm is acknowledged. Only hard or soft limit errors send this message immediately after the ALARM:x code.
  - \[`[MSG:Reset to continue]`-严重事件消息。在Grbl接受任何其他命令之前，需要重置。这可以防止正在进行的命令流，并在确认警报之前避免移动的风险。只有硬限制或软限制错误会在报警：x代码后立即发送此消息。\]
  - `[MSG:'$H'|'$X' to unlock]`- Alarm state is active at initialization. This message serves as a reminder note on how to cancel the alarm state. All g-code commands and some ‘$’ are blocked until the alarm state is cancelled via homing `$H` or unlocking `$X`. Only appears immediately after the `Grbl` welcome message when initialized with an alarm. Startup lines are not executed at initialization if this message is present and the alarm is active.
  - \[`[信息：'$H'|'$X'解锁]`-初始化时报警状态处于活动状态。此消息用作有关如何取消报警状态的提醒说明。所有g代码命令和一些`$`都会被阻止，直到通过归位`$H`或解锁`$X`取消报警状态。仅在使用报警初始化时，在`Grbl`欢迎消息之后立即显示。如果出现此消息且报警激活，则初始化时不会执行启动行。\]
  - `[MSG:Caution: Unlocked]` - Appears as an alarm unlock `$X` acknowledgement. An 'ok' still appears immediately after to denote the `$X` was parsed and executed. This message reminds the user that Grbl is operating under an unlock state, where startup lines have still not be executed and should be cautious and mindful of what they do. Grbl may not have retained machine position due to an alarm suddenly halting the machine.  A reset or re-homing Grbl is highly recommended as soon as possible, where any startup lines will be properly executed.
  - \[`[MSG:Caution:Unlocked]`-显示为报警解锁`$X`确认。在解析并执行`$X`之后，仍然会立即出现一个`ok`。此消息提醒用户Grbl在解锁状态下运行，此时启动行尚未执行，应谨慎并注意其操作。由于警报突然停止机器，Grbl可能没有保留机器位置。强烈建议尽快重置或重新归位Grbl，以便正确执行任何启动线路。\]
  - `[MSG:Enabled]` - Appears as a check-mode `$C` enabled acknowledgement. An 'ok' still appears immediately after to denote the `$C` was parsed and executed.
  - \[`[MSG:Enabled]`-显示为检查模式`$C`已启用确认。在解析并执行`$C`之后，仍然会立即出现一个'ok'。\]
  - `[MSG:Disabled]` - Appears as a check-mode `$C` disabled acknowledgement. An 'ok' still appears immediately after to denote the `$C` was parsed and executed. Grbl is automatically reset afterwards to restore all default g-code parser states changed by the check-mode.
  - \[`[MSG:Disabled]`-显示为检查模式`$C`已禁用确认。在解析并执行`$C`之后，仍然会立即出现一个'ok'。Grbl随后会自动重置，以恢复检查模式更改的所有默认g代码解析器状态。\]
  - `[MSG:Check Door]` - This message appears whenever the safety door detected as open. This includes immediately upon a safety door switch detects a pin change or appearing after the welcome message, if the safety door is ajar when Grbl initializes after a power-up/reset.
  - \[`[MSG:Check Door]`-只要检测到安全门打开，就会显示此消息。这包括当Grbl在通电/复位后初始化时，如果安全门未关，则安全门开关检测到针脚变化或欢迎信息后出现时立即启动。\]
    - If in motion and the safety door switch is triggered, Grbl will immediately send this message, start a feed hold, and place Grbl into a suspend with the DOOR state.
    - \[如果处于运动状态且安全门开关被触发，Grbl将立即发送此消息，启动进给保持，并将Grbl置于门状态下的暂停状态。\]
    - If not in motion and not at startup, the same process occurs without the feed hold.
    - \[如果不在运动中，也不在启动时，相同的过程会在没有进给保持的情况下发生。\]
    - If Grbl is in a DOOR state and already suspended, any detected door switch pin detected will generate this message, including a door close.
    - \[如果Grbl处于车门状态且已暂停，则检测到的任何车门开关引脚将生成此消息，包括车门关闭。\]
    - If this message appears at startup, Grbl will suspended into immediately into the DOOR state. The startup lines are executed immediately after the DOOR state is exited by closing the door and sending Grbl a resume command.
    - \[如果此消息在启动时出现，Grbl将立即挂起进入DOOR状态。通过关闭门并发送Grbl恢复命令，在门状态退出后立即执行启动行。\]

  - `[MSG:Check Limits]` - If Grbl detects a limit switch as triggered after a power-up/reset and hard limits are enabled, this will appear as a courtesy message immediately after the Grbl welcome message.
  - \[`[MSG:Check Limits]`-如果Grbl在通电/复位和硬限制启用后检测到限位开关触发，这将在Grbl欢迎信息后立即显示为礼貌信息。\]
  - `[MSG:Pgm End]` - M2/30 program end message to denote g-code modes have been restored to defaults according to the M2/30 g-code description.
  - \[`[MSG:Pgm End]`-M2/30程序结束消息表示g代码模式已根据M2/30 g代码说明恢复为默认值。\]
  - `[MSG:Restoring defaults]` - Appears as an acknowledgement message when restoring EEPROM defaults via a `$RST=` command. An 'ok' still appears immediately after to denote the `$RST=` was parsed and executed.
  - \[`[MSG:Restoring defaults]`-通过`$RST=`命令恢复EEPROM默认值时，显示为确认消息。在解析并执行`$RST=`之后，仍然会立即显示`ok`。\]
  - `[MSG:Restoring spindle]` - Appears when the spindle has been stopped during a feed hold via a spindle stop override command and when the cycle is resumed or the spindle stop is disabled.
  - \[`[MSG:恢复主轴]`-当主轴在进给保持期间通过主轴停止超越命令停止，并且循环恢复或主轴停止被禁用时出现。\]
  - `[MSG:Sleeping]` - Appears as an acknowledgement message when Grbl's sleep mode is invoked by issuing a `$SLP` command when in IDLE or ALARM states. Note that Grbl-Mega may invoke this at any time when the sleep timer option has been enabled and the timeout has been exceeded. Grbl may only be exited by a reset in the sleep state and will automatically enter an alarm state since the steppers were disabled.
  - \[`[MSG:Sleeping]`-在空闲或报警状态下，通过发出`$SLP`命令调用Grbl的睡眠模式时，显示为确认消息。请注意，Grbl Mega可以在启用睡眠计时器选项且超过超时的任何时间调用此选项。Grbl只能在睡眠状态下通过重置退出，并将自动进入报警状态，因为步进器已禁用。\]
	  - NOTE: Sleep will also invoke the parking motion, if it's enabled. However, if sleep is commanded during an ALARM, Grbl will not park and will simply de-energize everything and go to sleep.
    - \[注意：如果启用了`睡眠`，则`睡眠`也将调用驻车运动。然而，如果在警报期间指令睡眠，Grbl将不会停车，只会断开所有电源并进入睡眠状态。\]

- **Queried Feedback Messages:**   \[**查询的反馈信息：**\]

	- `[GC:]` G-code Parser State Message 
  - \[`[gc:]` G-代码解析器状态消息\]

		```
		[GC:G0 G54 G17 G21 G90 G94 M5 M9 T0 F0.0 S0]
		ok
		```
		
   		- Initiated by the user via a `$G` command. Grbl replies as follows, where the `[GC:` denotes the message type and is followed by a separate `ok` to confirm the `$G` was executed.	(由用户通过`$G`命令启动。Grbl回复如下，其中，`[GC:`表示消息类型，后跟一个单独的'ok'，以确认执行了`$G'。)

     	- The shown g-code are the current modal states of Grbl's g-code parser. This may not correlate to what is executing since there are usually several motions queued in the planner buffer.(所示的g代码是Grbl的g代码解析器的当前模式状态。这可能与正在执行的操作无关，因为在planner缓冲区中通常有多个运动排队。)
     	
     	- NOTE: Program modal state has been fixed and will show `M0`, `M2`, or `M30` when they are active. During a run state, nothing will appear for program modal state.(注意：程序模式状态已被修复，当它们处于活动状态时，将显示`M0`、`M2`或`M30`。在运行状态期间，程序模式状态不会显示任何内容。)
     	 


  - `[HLP:]` : Initiated by the user via a `$` print help command. The help message is shown below with a separate `ok` to confirm the `$` was executed.
  - \[`[HLP:::`：由用户通过`$`打印帮助命令启动。下面显示了帮助消息，其中包含一个单独的`ok`，以确认执行了`$`。\]
    ```
    [HLP:$$ $# $G $I $N $x=val $Nx=line $J=line $C $X $H ~ ! ? ctrl-x]
    ok
    ```
	- _**NOTE:** Grbl v1.1's new override real-time commands are not included in the help message. They use the extended-ASCII character set, which are not easily type-able, and require a GUI that supports them. This is for two reasons: Establish enough characters for all of the overrides with extra for later growth, and prevent accidental keystrokes or characters in a g-code file from enacting an override inadvertently._
  - \[_**注：**Grbl v1.1的新覆盖实时命令未包含在帮助消息中。它们使用扩展的ASCII字符集，而扩展的ASCII字符集不容易输入，并且需要支持它们的GUI。这有两个原因：为所有覆盖建立足够的字符，并为以后的增长提供额外的字符，以及防止意外击键或g代码文件中的字符无意中执行覆盖_\]


  - The `$#` print parameter data query produces a large set of data which shown below and completed by an `ok` response message.
  - \[`$#`打印参数数据查询生成一组大数据，如下所示，并由`ok`响应消息完成。\]

     - Each line of the printout is starts with the data type, a `:`, and followed by the data values. If there is more than one, the order is XYZ axes, separated by commas.
     - \-打印输出的每一行都以数据类型a`:`开始，然后是数据值。如果有多个，则顺序为XYZ轴，用逗号分隔。[\]

     ```
      [G54:0.000,0.000,0.000]
      [G55:0.000,0.000,0.000]
      [G56:0.000,0.000,0.000]
      [G57:0.000,0.000,0.000]
      [G58:0.000,0.000,0.000]
      [G59:0.000,0.000,0.000]
      [G28:0.000,0.000,0.000]
      [G30:0.000,0.000,0.000]
      [G92:0.000,0.000,0.000]
      [TLO:0.000]
      [PRB:0.000,0.000,0.000:0]
      ok
      ```

    - The `PRB:` probe parameter message includes an additional `:` and suffix value is a boolean. It denotes whether the last probe cycle was successful or not.
    - \[`PRB:`probe参数消息包含一个附加的`:`，后缀值是布尔值。它表示上一次探测循环是否成功。\]

  - `[VER:]` and `[OPT:]`: Indicates build info data from a `$I` user query. These build info messages are followed by an `ok` to confirm the `$I` was executed, like this:
  - \[`[VER::`和`[OPT:`:表示来自`$I`用户查询的生成信息数据。这些生成信息消息后面跟着一个`ok`，以确认执行了`$I`，如下所示：\]

      ```
      [VER:1.1d.20161014:]
      [OPT:,15,128]
      ok
      ```

      ```
      [VER:1.1d.20161014:Some string]
      [OPT:VL,15,128]
      ok
      ```
      
  	- `[VER:]` contains build version info and build date.
    - \[`[VER::`包含生成版本信息和生成日期。\]
      - a string may appear after the second `:` colon. It is a stored EEPROM string a user via a `$I=line` command or OEM can place there for personal use or tracking purposes.
      - \[\第二个`：`冒号后面可能会出现一个字符串。它是一个存储的EEPROM字符串，用户可以通过`$I=line`命令或OEM将其放置在该字符串中以供个人使用或跟踪。]

  	- `[OPT:]` line follows immediately after and contains character codes for compile-time options that were either enabled or disabled. 
    - \[`[OPT::`行紧跟在后面，包含启用或禁用的编译时选项的字符代码。\]
      - The codes are defined below and a CSV file is also provided for quick parsing. This is generally only used for diagnosing firmware bugs or compatibility issues. 
      - \[下面定义了代码，并提供了一个CSV文件用于快速解析。这通常仅用于诊断固件错误或兼容性问题。\]
      - The value after the first comma contains the `blockBufferSize`, as int.
      - \[第一个逗号后的值包含`blockBufferSize`，即int。\]
      - The value after the second comma contains the `rxBufferSize`, as int.
      - \[第二个逗号后的值包含'rxBufferSize'，如int。\]

| `OPT` Code \[`OPT`编程码\] | Setting Description, Units \[设置说明，单位\] |
|:-------------:|----|
| **`V`** | Variable spindle enabled  \[可变主轴启用\]|
| **`N`** | Line numbers enabled  \[行号已启用\]|
| **`M`** | Mist coolant enabled  \[雾冷却液启用\]|
| **`C`** | CoreXY enabled  \[启用CoreXY\]|
| **`P`** | Parking motion enabled  \[停车运动已启用\]|
| **`Z`** | Homing force origin enabled  \[归航力原点已启用\]|
| **`H`** | Homing single axis enabled  \[启用单轴回零\]|
| **`T`** | Two limit switches on axis enabled  \[轴上的两个限位开关已启用\]|
| **`A`** | Allow feed rate overrides in probe cycles  \[轴上的两个限位开关已启用\]|
| **`*`** | Restore all EEPROM disabled  \[恢复所有已禁用的EEPROM\]|
| **`$`** | Restore EEPROM `$` settings disabled  \[恢复EEPROM`$`设置已禁用\]|
| **`#`** | Restore EEPROM parameter data disabled  \[恢复EEPROM参数数据已禁用\]|
| **`I`** | Build info write user string disabled  \[生成信息写入用户字符串已禁用\]|
| **`E`** | Force sync upon EEPROM write disabled  \[禁用EEPROM写入时强制同步\]|
| **`W`** | Force sync upon work coordinate offset change disabled  \[禁用工作坐标偏移更改时强制同步\]|
| **`L`** | Homing init lock sets Grbl into an alarm state upon power up  \[开机时，复位初始锁将Grbl设置为报警状态\]|
| **`2`** | Dual axis motors with self-squaring enabled  \[启用自平方功能的双轴电机\]|

  - `[echo:]` : Indicates an automated line echo from a command just prior to being parsed and executed. May be enabled only by a config.h option. Often used for debugging communication issues. A typical line echo message is shown below. A separate `ok` will eventually appear to confirm the line has been parsed and executed, but may not be immediate as with any line command containing motions.
  - \[`[echo:]`：表示在解析和执行命令之前，来自命令的自动行回显。只能通过config.h选项启用。通常用于调试通信问题。典型的线路回波信息如下所示。最后会出现一个单独的`ok`来确认该行已被解析和执行，但可能不会像任何包含运动的行命令那样立即执行。\]
      ```
      [echo:G1X0.540Y10.4F100]
      ```
      - NOTE: The echoed line will have been pre-parsed a bit by Grbl. No spaces or comments will appear and all letters will be capitalized.(注：Grbl将对回波线进行一点预分析。不会出现空格或注释，所有字母都将大写。)

------

#### Startup Line Execution \[启动行执行\]

  - `>G54G20:ok` : The open chevron indicates a startup line has just executed. The startup line `G54G20` immediately follows the `>` character and is followed by an `:ok` response to indicate it executed successfully.
  - \[`>G54G20:ok`：打开的V形标志表示刚刚执行了启动行。启动行`G54G20`紧跟在`>`字符后面，后面跟着一个`：ok`响应，表示它已成功执行。\]

    - A startup line will execute upon initialization only if a line is present and if Grbl is not in an alarm state.
    - \[只有在存在行且Grbl未处于报警状态时，才会在初始化时执行启动行。\]
    - The `:ok` on the same line is intentional. It prevents this `ok` response from being counted as part of a stream, because it is not tied to a sent command, but an internally-generated one.
    - \[同一行上的`：ok`是故意的。它防止将此`ok`响应作为流的一部分进行计数，因为它不绑定到已发送的命令，而是内部生成的命令。\]
    - There should always be an appended `:ok` because the startup line is checked for validity before it is stored in EEPROM. In the event that it's not, Grbl will print `>G54G20:error:X`, where `X` is the error code explaining why the startup line failed to execute.
    - \[应始终附加一个`：ok`，因为在将启动行存储在EEPROM中之前，会检查启动行的有效性。如果不是，Grbl将打印`>G54G20:error:X`，其中`X`是解释启动行无法执行原因的错误代码。\]
    - In the rare chance that there is an EEPROM read error, the startup line execution will print `>:error:7` with no startup line and a error code `7` for a read fail. Grbl will also automatically wipe and try to restore the problematic EEPROM.
    - \[极有可能出现EEPROM读取错误，启动行执行将打印`>:error:7`，没有启动行，并且读取失败时会显示错误代码`7`。Grbl还将自动擦除并尝试恢复有问题的EEPROM。\]

------

#### Real-time Status Reports \[实时状态报告\]

- Contains real-time data of Grbl’s state, position, and other data required independently of the stream.
-  \[包含Grbl状态、位置的实时数据，以及独立于流所需的其他数据\]
- Categorized as a real-time message, where it is a separate message that should not be counted as part of the streaming protocol. It may appear at any given time.
-  \[分类为实时消息，其中它是不应被视为流协议一部分的单独消息。它可能在任何给定的时间出现。\]
- A status report is initiated by sending Grbl a '?' character.
-  \[通过向Grbl发送`？`字符启动状态报告。\]

  - Like all real-time commands, the '?' character is intercepted and never enters the serial buffer. It's never a part of the stream and can be sent at any time.
  -  \[与所有实时命令一样，`？`字符被截取，永远不会进入串行缓冲区。它从来不是流的一部分，可以随时发送。\]
  - Grbl will generate and transmit a report within ~5-20 milliseconds.
  -  \[Grbl将在约5-20毫秒内生成并传输报告。\]
  - Every ’?’ command sent by a GUI is not guaranteed with a response. The following are the current scenarios when Grbl may not immediately or ignore a status report request. _NOTE: These may change in the future and will be documented here._
  -  \[GUI发送的每个`？`命令都不保证有响应。以下是Grbl可能不会立即或忽略状态报告请求的当前场景_注：这些可能在未来发生变化，并将在此处记录_\]

    - If two or more '?' queries are sent before the first report is generated, the additional queries are ignored.(如果在生成第一个报告之前发送了两个或多个`？`查询，则会忽略其他查询。)

    - A soft-reset commanded clears the last status report query.(软复位命令清除最后的状态报告查询。)

    - When Grbl throws a critical alarm from a limit violation. A soft-reset is required to resume operation.(当Grbl因违反限制引发严重警报时。恢复操作需要软复位。)

    - During a homing cycle.(在回零周期中。)

- **Message Construction:**  \[消息构造：\]

  - A message is a single line of ascii text, completed by a carriage return and line feed.
  -  \[消息是一行ascii文本，由回车符和换行符完成。\]
  - `< >` Chevrons uniquely enclose reports to indicate message type.
  -  \[`<>` V形符号唯一地将报告括起来以指示消息类型。\]
  - `|` Pipe delimiters separate data fields inside the report.
  -  \[`|`管道分隔符用于分隔报表中的数据字段。\]
  - The first data field is an exception to the following data field rules. See 'Machine State' description for details.
  -  \[第一个数据字段是以下数据字段规则的例外。有关详细信息，请参阅`机器状态`说明。\]
  - All remaining data fields consist of a data type followed by a `:` colon delimiter and data values. `type:value(s)`
  -  \[所有剩余的数据字段都由一个后跟`：`冒号分隔符和数据值的数据类型组成`类型：值`\]
  - Data values are given either as as one or more pre-defined character codes to indicate certain states/conditions or as numeric values, which are separated by a `,` comma delimiter when more than one is present. Numeric values are also in a pre-defined order and units of measure.
  -  \[数据值以一个或多个预定义字符代码的形式给出，以指示某些状态/条件，或以数值的形式给出，当存在多个状态/条件时，这些数值由`、`逗号分隔符分隔。数值也以预定义的顺序和度量单位显示。\]
  - The first (Machine State) and second (Current Position) data fields are always included in every report.
  -  \第一个（机器状态）和第二个（当前位置）数据字段始终包含在每个报告中。[\]
  - Assume any following data field may or may not exist and can be in any order. The `$10` status report mask setting can alter what data is present and certain data fields can be reported intermittently (see descriptions for details.)
  -  \[假设以下任何数据字段可能存在，也可能不存在，并且可以是任意顺序。`$10`状态报告掩码设置可以改变存在的数据，某些数据字段可以间歇报告（有关详细信息，请参阅说明）\]
  - The `$13` report inches settings alters the units of some data values. `$13=0` false indicates mm-mode, while `$13=1` true indicates inch-mode reporting. Keep note of this setting and which report values can be altered.
  -  \[`$13`报告英寸设置更改某些数据值的单位`$13=0`false表示毫米模式，`$13=1`true表示英寸模式报告。请注意此设置以及哪些报告值可以更改。\]
- **Data Field Descriptions:**   \[**数据字段说明：**\]

    - **Machine State:**
    - \[**机器状态：**\]

      - Valid states types:  `Idle, Run, Hold, Jog, Alarm, Door, Check, Home, Sleep`
      - \[有效状态类型：`Idle、Run、Hold、Jog、Alarm、Door、Check、Home、Sleep`\]
      - Sub-states may be included via `:` a colon delimiter and numeric code.
      - \[子状态可以通过`：`冒号分隔符和数字代码包含。\]
      - Current sub-states are:
      - \[当前的子状态是：\]
        - `Hold:0` Hold complete. Ready to resume.
        - \[`保持：0`保持完成。准备恢复。\]
	     - `Hold:1` Hold in-progress. Reset will throw an alarm.
        - \[`暂停：1 `暂停进行中。重置将发出警报。\]
        - `Door:0` Door closed. Ready to resume.
        - \[`门：0`门已关闭。准备恢复。\]
        - `Door:1` Machine stopped. Door still ajar. Can't resume until closed.
        - \[`门：1`机器停止。门还半开着。在关闭之前无法恢复。\]
        - `Door:2` Door opened. Hold (or parking retract) in-progress. Reset will throw an alarm.
        - \[`门：2`门开了。保持（或驻车缩回）正在进行。重置将发出警报。\]
	    - `Door:3` Door closed and resuming. Restoring from park, if applicable. Reset will throw an alarm.  
        - \[`门：3`门关闭并恢复。从停车场恢复（如适用）。重置将发出警报。\]

      - This data field is always present as the first field.
      - \[此数据字段始终作为第一个字段出现。\]

    - **Current Position:** \[**当前职位：**\]

        - Depending on `$10` status report mask settings, position may be sent as either:
        - \[根据`$10`状态报告掩码设置，位置可以通过以下方式发送：\]

          - `MPos:0.000,-10.000,5.000` machine position or
          - \[`MPos:0.000，-10.000,5.000`机器位置或\]
          - `WPos:-2.500,0.000,11.000` work position
          - \[`MPos:0.000，-10.000,5.000`机器位置或\]

        - **NOTE: Grbl v1.1 sends only one position vector because a GUI can easily compute the other position vector with the work coordinate offset `WCO:` data. See WCO description for details.**
        - **\[**注意：GRBLV1.1只发送一个位置向量，因为GUI可以使用工作坐标偏移`WCO:`数据轻松计算另一个位置向量。有关详细信息，请参见世界海关组织的说明**\]**

        - Three position values are given in the order of X, Y, and Z. A fourth position value may exist in later versions for the A-axis.
        - \[三个位置值按X、Y和Z的顺序给出。第四个位置值可能存在于A轴的更高版本中。\]
        - `$13` report inches user setting effects these values and is given as either mm or inches.
        - \[`$13`报告英寸用户设置影响这些值，并以毫米或英寸表示。\]
        - This data field is always present as the second field.
        - \[此数据字段始终作为第二个字段出现。\]

    - **Work Coordinate Offset \[工作坐标偏移\]:**

        - `WCO:0.000,1.551,5.664` is the current work coordinate offset of the g-code parser, which is the sum of the current work coordinate system, G92 offsets, and G43.1 tool length offset.
        - \[`WCO:0.000,1.551,5.664`是g代码解析器的当前工作坐标偏移量，它是当前工作坐标系、G92偏移量和G43.1刀具长度偏移量的总和。\]

        - Machine position and work position are related by this simple equation per axis: `WPos = MPos - WCO`
        - \[机器位置和工作位置通过每个轴的一个简单等式关联：`WPos=MPos-WCO`\]
        
        	- **GUI Developers:** Simply track and retain the last `WCO:` vector and use the above equation to compute the other position vector for your position readouts. If Grbl's status reports show either `WPos` or `MPos`, just follow the equations below. It's as easy as that!
          - \[**GUI开发人员：** 只需跟踪并保留最后一个`WCO:`向量，并使用上述等式计算位置读数的另一个位置向量。如果Grbl的状态报告显示`WPos`或`MPos`，只需遵循以下等式即可。就这么简单！\]

            - If `WPos:` is given, use `MPos = WPos + WCO`.
             - \[如果给定了`WPos:`，请使用`MPos=WPos+WCO`。\]
        	  - If `MPos:` is given, use `WPos = MPos - WCO`.
            - \[如果给定了`MPos:`，请使用`WPos=MPos-WCO`。\]

        - Values are given in the order of the X,Y, and Z axes offsets. A fourth offset value may exist in later versions for the A-axis.
        - \[值按X、Y和Z轴偏移的顺序给出。第四个偏移值可能存在于A轴的更高版本中。\]
        - `$13` report inches user setting effects these values and is given as either mm or inches.
        - \[`$13`报告英寸用户设置影响这些值，并以毫米或英寸表示。\]
        - `WCO:` values don't change often during a job once set and only requires intermittent refreshing.
        - \[`WCO:`值在作业中设置后不会经常更改，只需要间歇性刷新。\]
        - This data field appears:
        - \[此数据字段将显示：\]

          - In every 10 or 30 (configurable 1-255) status reports, depending on if Grbl is in a motion state or not.
          - \[根据Grbl是否处于运动状态，在每10或30（可配置1-255）个状态报告中。\]
          - Immediately in the next report, if an offset value has changed.
          - \[如果偏移值已更改，则在下一个报告中立即显示。\]
          - In the first report after a reset/power-cycle.
          - \[在重置/电源循环后的第一份报告中。\]

        - This data field will not appear if:
        - \[如果出现以下情况，则不会显示此数据字段：\]

          - It is disabled in the config.h file. No `$` mask setting available.
          - \[它在config.h文件中被禁用。没有可用的`$`掩码设置\]
          - The refresh counter is in-between intermittent reports.    
          - \[刷新计数器处于间歇报告之间。\]   

    - **Buffer State\[缓冲区状态\]:** 

        - `Bf:15,128`. The first value is the number of available blocks in the planner buffer and the second is number of available bytes in the serial RX buffer.
        - \[`Bf:15128`。第一个值是planner缓冲区中的可用块数，第二个值是串行RX缓冲区中的可用字节数。\]
        
        - The usage of this data is generally for debugging an interface, but is known to be used to control some GUI-specific tasks. While this is disabled by default, GUIs should expect this data field to appear, but they may ignore it, if desired.
        - \[这些数据通常用于调试接口，但已知用于控制某些特定于GUI的任务。虽然这在默认情况下是禁用的，但GUI应该期望出现此数据字段，但如果需要，他们可能会忽略它。\]
        
        - NOTE: The buffer state values changed from showing `in-use` blocks or bytes to `available`. This change does not require the GUI knowing how many block/bytes Grbl has been compiled with.
        - \[注意：缓冲区状态值从显示`正在使用`的块或字节更改为`可用`。此更改不需要GUI知道Grbl编译时使用了多少块/字节。\]

        - This data field appears:
        - \[此数据字段将显示：\]
        
          - In every status report when enabled. It is disabled in the settings mask by default.
          - \[启用时，在每个状态报告中。默认情况下，它在设置掩码中被禁用。\]
        
        - This data field will not appear if:
        - \[如果出现以下情况，则不会显示此数据字段：\]

          - It is disabled by the `$` status report mask setting or disabled in the config.h file.
          - \[它被`$`状态报告掩码设置禁用，或在config.h文件中被禁用。\]

    - **Line Number:**
    - **\[行号：\]**

        - `Ln:99999` indicates line 99999 is currently being executed. This differs from the `$G` line `N` value since the parser is usually queued few blocks behind execution.
        - \[`Ln:99999`表示当前正在执行第99999行。这与`$G`line`N`值不同，因为解析器通常在执行后的几个块中排队。\]

        - Compile-time option only because of memory requirements. However, if a GUI passes indicator line numbers onto Grbl, it's very useful to determine when Grbl is executing them.
        - \[编译时选项仅因为内存需求。然而，如果GUI将指示符行号传递给Grbl，那么确定Grbl何时执行它们是非常有用的。\]

        - This data field will not appear if:
        - \[如果出现以下情况，则不会显示此数据字段：\]

          - It is disabled in the config.h file. No `$` mask setting available.
          - \[它在config.h文件中被禁用。没有可用的`$`掩码设置。\]
          - The line number reporting not enabled in config.h. Different option to reporting data field.
          - \[未在config.h中启用行号报告。报告数据字段的不同选项。\]
          - No line number or `N0` is passed with the g-code block.
          - \[g代码块不传递行号或'N0'。\]
          - Grbl is homing, jogging, parking, or performing a system task/motion.
          - \[Grbl是归位、慢跑、停车或执行系统任务/运动。\]
          - There is no motion in the g-code block like a `G4P1` dwell. (May be fixed in later versions.)
          - \[g代码块中没有像`G4P1`驻留那样的运动。（可能在更高版本中修复。）\]

    - **Current Feed and Speed:**
    - **\[当前进给和速度：\]**

        - There are two versions of this data field that Grbl may respond with. 
        - \[-Grbl可能会响应此数据字段的两个版本\]
        
          - `F:500` contains real-time feed rate data as the value. This appears only when VARIABLE_SPINDLE is disabled in config.h, because spindle speed is not tracked in this mode.
          - \[`F:500`包含实时进给速率数据作为值。仅当在config.h中禁用变量_主轴时，才会出现此信息，因为在此模式下不会跟踪主轴速度。\]
          - `FS:500,8000` contains real-time feed rate, followed by spindle speed, data as the values. Note the `FS:`, rather than  `F:`, data type name indicates spindle speed data is included.
          - \[`FS:500，8000`包含实时进给速度，然后是主轴速度，数据作为值。请注意，数据类型名称中的`FS:`，而不是`F:`，表示包含主轴转速数据。\]
                
        - The current feed rate value is in mm/min or inches/min, depending on the `$` report inches user setting. 
        - \[当前进给速度值以毫米/分钟或英寸/分钟为单位，具体取决于`$`报告英寸用户设置。\]
        - The second value is the current spindle speed in RPM
        - \[第二个值是当前主轴转速，单位为RPM\]
        
        - These values will often not be the programmed feed rate or spindle speed, because several situations can alter or limit them. For example, overrides directly scale the programmed values to a different running value, while machine settings, acceleration profiles, and even the direction traveled can also limit rates to maximum values allowable.
        - \[这些值通常不是编程的进给速度或主轴转速，因为有几种情况会改变或限制它们。例如，覆盖直接将编程值缩放为不同的运行值，而机器设置、加速度曲线甚至行驶方向也可以将速率限制为允许的最大值。\]

        - As a operational note, reported rate is typically 30-50 msec behind actual position reported.
        - \[作为操作说明，报告的速率通常比报告的实际位置晚30-50毫秒。\]

        - This data field will always appear, unless it was explicitly disabled in the config.h file.
        - \[除非在config.h文件中显式禁用此数据字段，否则此数据字段将始终显示。\]

    - **Input Pin State:**
    - **\[输入引脚状态：\]**

        - `Pn:XYZPDHRS` indicates which input pins Grbl has detected as 'triggered'.
        - \[`Pn:XYZPDHRS`表示哪些输入引脚Grbl已检测为'已触发'。\]

        - Pin state is evaluated every time a status report is generated. All input pin inversions are appropriately applied to determine 'triggered' states.
        - \[每次生成状态报告时都会评估Pin状态。所有输入引脚反转均适用于确定`触发`状态。\]

        - Each letter of `XYZPDHRS` denotes a particular 'triggered' input pin.
        - \[`XYZPDHRS`的每个字母表示一个特定的`触发`输入引脚。\]

          - `X Y Z` XYZ limit pins, respectively
          - \[`X Y Z`XYZ限位销，分别为\]
          - `P` the probe pin.
          - \[`P`探针。\]
          - `D H R S` the door, hold, soft-reset, and cycle-start pins, respectively.
          - \[`D H R S`分别关闭门、保持、软复位和循环启动引脚。\]
          - Example: `Pn:PZ` indicates the probe and z-limit pins are 'triggered'.
          - \[示例：`Pn:PZ`表示探针和z限位销已`触发`。\]
          - Note: `A` may be added in later versions for an A-axis limit pin.
          - \[注：A轴限位销的更高版本中可能会添加`A`。\]

        - Assume input pin letters are presented in no particular order.
        - \[假设输入端号字母没有特定顺序。\]

        - One or more 'triggered' pin letter(s) will always be present with a `Pn:` data field.
        - \[一个或多个`已触发`pin字母将始终与`Pn:`数据字段一起出现。\]

        - This data field will not appear if:
        - \[如果出现以下情况，则不会显示此数据字段：\]

          - It is disabled in the config.h file. No `$` mask setting available.
          - \[它在config.h文件中被禁用。没有可用的`$`掩码设置。\]
          - No input pins are detected as triggered.
          - \[未检测到触发的输入引脚。\]

    - **Override Values:**
    - **\[覆盖值：\]**

        - `Ov:100,100,100` indicates current override values in percent of programmed values for feed, rapids, and spindle speed, respectively.
        - \[`Ov:100100`分别以进给、急流和主轴速度编程值的百分比表示当前超越值。\]

        - Override maximum, minimum, and increment sizes are all configurable within config.h. Assume that a user or OEM will alter these based on customized use-cases. Recommend not hard-coding these values into a GUI, but rather just show the actual override values and generic increment buttons.
        - \[覆盖最大、最小和增量大小都可以在config.h中配置。假设用户或OEM将根据定制的用例更改这些。建议不要将这些值硬编码到GUI中，而只显示实际的覆盖值和通用增量按钮\]

        - Override values don't change often during a job once set and only requires intermittent refreshing. This data field appears:
        - \[一旦设置了覆盖值，在作业期间不会经常更改，只需要间歇性刷新。此数据字段将显示：\]

          - After 10 or 20 (configurable 1-255) status reports, depending on is in a motion state or not.
          - \[10或20（可配置1-255）后的状态报告，取决于是否处于运动状态。\]
          - If an override value has changed, this data field will appear immediately in the next report. However, if `WCO:` is present, this data field will be delayed one report.
          - \[如果覆盖值已更改，此数据字段将立即显示在下一个报告中。但是，如果存在`世界海关组织：`，该数据字段将延迟一次报告。\]
          - In the second report after a reset/power-cycle.
          - \[在重置/电源循环后的第二份报告中。\]

        - This data field will not appear if:
        - \[如果出现以下情况，则不会显示此数据字段：\]

          - It is disabled in the config.h file. No `$` mask setting available.
          - \[它在config.h文件中被禁用。没有可用的`$`掩码设置。\]
          - The override refresh counter is in-between intermittent reports.
          - \[覆盖刷新计数器位于间歇报告之间。\]
          - `WCO:` exists in current report during refresh. Automatically set to try again on next report.
          - \[`WCO:`刷新期间存在于当前报告中。自动设置为下次报告时重试。\]

	- **Accessory State:**
  - **\[附加声明：\]**

    - `A:SFM` indicates the current state of accessory machine components, such as the spindle and coolant.
    - \[`A:SFM`表示辅助机器部件的当前状态，如主轴和冷却液。\]

    - Due to the new toggle overrides, these machine components may not be running according to the g-code program. This data is provided to ensure the user knows exactly what Grbl is doing at any given time.
    - \[由于新的切换覆盖，这些机器部件可能未按照g代码程序运行。提供这些数据是为了确保用户确切知道Grbl在任何给定时间正在做什么。\]
		
  	- Each letter after `A:` denotes a particular state. When it appears, the state is enabled. When it does not appear, the state is disabled.
    - \[每个字母后面都有`A:`表示特定的状态。当它出现时，状态为启用。如果不显示，则状态为禁用。\]
			
      - `S` indicates spindle is enabled in the CW direction. This does not appear with `C`.
      - \[`S`表示主轴在顺时针方向启用。这不会与`C`一起出现。\]

  	  - `C` indicates spindle is enabled in the CCW direction. This does not appear with `S`.
      - \[`C`表示主轴在CCW方向启用。这不会与`S`一起出现。\]

      - `F` indicates flood coolant is enabled.
      - \[`F`表示启用了溢流冷却液。\]

      - `M` indicates mist coolant is enabled.
      - \[`M`表示雾冷却液已启用。\]
		 
    - Assume accessory state letters are presented in no particular order.
    - \[假设附件状态字母没有特定顺序。\]
      
      - This data field appears:
      - \[此数据字段将显示：\]
        - When any accessory state is enabled.
        - \[启用任何附件状态时。\]
        - Only with the override values field in the same message. Any accessory state change will trigger the accessory state and override values fields to be shown on the next report.
        - \[仅在同一消息中使用`覆盖值`字段。任何附件状态更改都将触发附件状态和覆盖值字段，以便在下一个报告中显示。\]

      - This data field will not appear if:
      - \[如果出现以下情况，则不会显示此数据字段：\]

      	- No accessory state is active.
        - \[没有附件状态处于活动状态。\]
        - It is disabled in the config.h file. No `$` mask setting available.
        - \[它在config.h文件中被禁用。没有可用的`$`掩码设置。\]
       	- If override refresh counter is in-between intermittent reports.
        - \[如果覆盖刷新计数器处于间歇报告之间。\]
        - `WCO:` exists in current report during refresh. Automatically set to try again on next report.
        - \[`WCO:`刷新期间存在于当前报告中。自动设置为下次报告时重试。\]