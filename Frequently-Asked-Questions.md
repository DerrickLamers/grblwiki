_Please feel free to contribute more up-to-date answers or questions!_

#### How do you pronounce Grbl!?

This is probably the number one question, and the answer is: "It depends on who you ask." 

When Simen Svale Skogsrud first sat down and wrote Grbl in 2009, he named it after a bigger version of a computer mouse. It's small, useful, and doesn't do much other than what its designed to do. So, if you ask him, it's pronounced like "_gerbil_". If you ask Sonny Jeon, the developer since 2011, he'd say it's pronounced '_grr-ble_', because he was teased too many times by his wife when working on the project. If you ask Edward Ford of Shapeoko, he'd probably say '_garble_', even though he knows better. In any case, pronounce it however you'd like. We love hearing the various interpretations of our name!

#### I think there's a bug! How can I help?

Grbl has been well-tested and vetted over the years, but we're human and still make dumb-ass mistakes. If you think you've found a problem, first do a quick search to see if we know about it or if there is already a fix. If not, then post a new issue here with a description of the problem and any instructions on how to reproduce the problem. Please post your `$I` build info string and `$$` settings too! It makes debugging much faster. Finally, if you are not using the most recent version of the Arduino IDE to upload Grbl or are flashing our pre-compiled hex file, let us know what IDE version or what you do to compile Grbl. Thanks!!

#### What happened to v1.0? There was v0.9, then v1.1. What gives?
Grbl v1.0 was a development version that was not officially released. Some OEMs and users started using this development v1.0 version anyway. Because of that, GUI developers had v1.0 users asking to support it. To make it easier on GUI developers, Grbl v1.0 was just skipped, so they don't have to deal with trying to figure out which v1.0 was which.

#### My GUI doesn’t connect to Grbl but connecting directly via serial terminal works fine! What gives?!
The communication protocol was updated in Grbl v1.1 to accommodate the new and more complete real-time data, like the new overrides and pin states, and keep the streaming bandwidth efficient. This means older GUIs that used to work with Grbl, like older versions of UGS, may not if they have not been updated to support v1.1. First download the most recent version of your GUI to see if that fixes your problem. If not, try another Grbl GUI like UGS Platform or bCNC. 

## Compiling Grbl

#### How do I compile Grbl and upload it?
See our [Compiling Grbl](https://github.com/gnea/grbl/wiki/Compiling-Grbl) for a list of recommended ways to do this!

#### I get this "Low-memory available, stability problems may occur." warning after I compile Grbl! Is there a problem?!
Nope! Grbl packs the Arduino 328p processor to its absolute max. It uses quite literally **all** of the available memory and flash to make it as good as possible. Grbl has been designed to not need that much stack memory.

#### Where can I download a pre-compiled Grbl HEX file to flash onto my Arduino?
Links to pre-compiled Grbl HEX files are located on the "Releases" tab on the front page. Or [click here](https://github.com/gnea/grbl/releases).

#### The HEX file is larger than 32K that the Arduino will hold. Will it still upload?
Yes, it should. The compiled HEX file is in hexidecimal format, not binary. So it can be somewhat larger than the 32K flash limit on Arduinos. No more than about double the 32K, though.

## Flashing Grbl

#### How do I flash Grbl to my Arduino?
Please check the Wiki help pages for details. If they are not up-to-date, please notify us or update them if you found they were in error or have another method. Thanks!

#### Does Grbl overwrite the Arduino bootloader?  
Nope! Grbl fits on the ATmega328P without having to overwrite the bootloader; you will still be able to upload Arduino sketches after flashing without having to re-burn the bootloader. NOTE: Grbl v1.0 may be too large for Arduino Duemilanove-compatible boards, as their boot loader takes up 1.5KB, rather than 0.5KB on the Uno. This means that there is 1KB less available flash.

#### Grbl won't fit on my Arduino Nano (or FTDI-based Arduino)! Am I totally hosed?
No! The default Grbl build will fit on an Arduino Nano. Just barely. If you enable compile-time options like CoreXY support, you can easily exceed the 30.5KB limit on the Nano. **BUT**, you have a few options to free up some more flash. First, you can write the much smaller Arduino Uno bootloader (0.5KB vs 1.5KB) to your Nano with a [spare Arduino](https://www.arduino.cc/en/Tutorial/ArduinoISP)! Just wire things up, select the Arduino Uno as your board, and write the Uno bootloader to your Nano. Congrats! Now you have one KB of extra flash. Your Arduino Nano will now behave like an Uno! Just make sure that Arduino IDE knows it by selecting the Uno as the board. (NOTE: On some Nanos, you might have to hard reset your Nano right at the start of flashing.) The second option is to just write Grbl directly to the 328p processor with your spare Arduino through the ISCP header. This will get rid of the bootloader completely and free all of the remaining flash. Don't worry, you can still write the bootloader back on, if you want to.

## Grbl Porting

#### Will you make a port for X MCU?

The intent for the soon-to-be released HAL for Grbl is to be able to spread out the workload to MCU 'experts' to tailor their port to a particular MCU and what that MCU can do. They are all different and have unique needs/capabilities.

The Grbl based project is including multiple pin assignments with a hardware abstraction. But this will not include the 328p Uno. Only Mega, ARM, or any micro with enough ram and flash. Ports will not be supported by myself, but users interested in maintaining a particular port for the community.

In the meanwhile here is a list of latest community ports, if you know of another please contribute here

**Grbl 1.1**

- [Mega2560](https://github.com/gnea/grbl-Mega)
- [STM32F103](https://github.com/usbcnc/grbl)
- [PSOC5](https://github.com/bdring/PSoC_Grbl)
- [ARM LPC1769](https://github.com/gnea/grbl-LPC); [ARM Discussion GIT](https://github.com/gnea/grbl/issues/67)

**Grbl 0.9**

- [RAMPS and Sanguinololu](https://github.com/CarlosGS/grblForCyclone)

## Extending Grbl

#### I want to add some new function to Grbl, how much space is left on the 328p?
Literally none left, v1.1 exhausted the last bits, use Grbl for [Mega2560](https://github.com/gnea/grbl-Mega) instead!

#### Ok I'm using a Mega, how do I add a LCD or another add-on?
The best practice for doing this is to let Grbl do it's job and use another arduino to do the display and jog.

While it's possible to edit directly the source code for this it's easier and safer to write a sender to talk to Grbl, you don't risk breaking it, and you will not have trouble with future updates. 

Here's some known contribuitions from the community on this matter;
- [LCD on Grbl DRO](https://www.shapeoko.com/wiki/index.php/LCD_on_GRBL#Full_version_GRBL_1.1)
- [Grbl + LCDKeypad Shield](https://forum.arduino.cc/index.php?topic=343638.0)

#### Why can't I use Arduino Libraries instead?
Grbl is written in pure C and does not use any Arduino code and will break if you try to. It only uses the IDE as a vehicle to compile and flash it. 

If you want to add functions You will have to edit and write in Grbl source code only.
Avoid any Arduino function calls.

#### What if I hardcode a Library?
Arduino libraries have a ton of dependencies. Wading through that will require you to fully understand the Arduino source code in its entirety. It is doable, but good luck with that.

## Connecting Grbl

#### My CNC moves erratically when I boot up my Arduino! Why does it do this?
The Arduino bootloader takes a second or two to boot up before Grbl initializes. During this time, the stepper enable pin is LOW, which is enabled, before Grbl finishes its initialization and sets the pin to HIGH to disable the steppers. This brief moment makes your stepper drivers susceptible to electronic noise, so if your driver step pins have enough noise to falsely indicate a step, your steppers may start moving erratically. 

This may be an unavoidable problem with straight-up Arduinos. There are some potential solutions however. You can try to locate the source of the electronic noise and remove it (a fan too close the other electronics). You can place a pull-up resistor on the stepper enable line to disable the steppers during the boot-up process. You can also remove the Arduino bootloader altogether and install Grbl through the ICSP header, which requires specific hardware to do it.

When the Arduino board is USB powered and the stepper drivers have their own logic voltage supply, don't forget to connect the ground of both circuits.

#### My spindle turns on when I boot up my Arduino for a second or two! Why does it do this? 
This is only applicable to users that have certain custom compile-time options, namely using the spindle direction pin as an enable pin (custom option) when `VARIABLE_SPINDLE` is enabled (default), which uses pin D13 as a spindle enable. The problem comes from the Arduino boot loader itself. Upon power-up, it toggles pin D13 for a second or two to flash the D13 LED to let the user know that the Arduino is active. If your spindle enable pin is connected to this D13 pin, then it will tell your spindle to toggle on and off with each flash until Grbl is booted and the pin is finally initialized properly. 

What can you do? There are a few options, described in increasing difficulty. First, you can try flashing Grbl without the Arduino boot loader. You'll need an ISCP programmer or spare Arduino (see Arduino as ISP) to directly flash Grbl. You just need to properly connect the programmer the target Grbl board and select the appropriate programmer in the Arduino IDE `Tools -> Programmer` menu. Second, a hardware solution would be to gate the D13 pin with something else that is inactive during boot time. Third, you can alter the Grbl firmware to try selecting another available pin to be used as a spindle enable pin. You should be able to alter the cpu_map.h file to use a different pin on the same port, but D13 can't be used as an input pin due to the LED present there. So you may have to combine limit pins to share axes, like Z and X, to make one available on the port. Otherwise, you'll need to alter the spindle_control source file to move the enable pin to a different port, such as pins A0-A5. Fourth, you can customize the Arduino boot loader to not flash pin D13 upon initialization. However, modifying, compiling, and flashing the boot loader can be a royal pain and is only for advanced users.

#### My Z-limit and Z-homing doesn't work! I updated from Grbl v0.8 (or less) and it used to work! What gives?
In Grbl v0.9 and after, Grbl switched the z-limit (D11) and spindle enable pin (D12) to access the hardware PWM on pin D11 to enable variable spindle output (aka spindle speed) and enable laser support. This is well-documented in the "Connecting Grbl" wiki page and various other sites and issues threads. This is most commonly caused by the popular use of a cheap, open-source CNC shield that is based around the old v0.8 pin configuration. The manufacturers of these clone boards haven't and seemingly refuse to update the design, but there are versions of this board that have updated to the current pin configuration by the original designer. The simplest solution is to revert to the old pin configuration. However, this sacrifices your laser support and variable spindle output. To do so, just edit the config.h file in the Arduino library path for Grbl, comment out the `#define VARIABLE_SPINDLE` line, and recompile and flash. If you want laser and variable spindle support on these cheap clone boards, there are several solutions on how to alter the boards, like soldering jumper wires, but this isn't the place for questions about this. Simply search the web. There are plenty of solutions and prior issues threads that have already answered this question in great depth and detail.


## Configuring Grbl

#### What happened to the `$$` setting descriptions! They're missing! I'm so confused.
We miss them too! In versions prior to Grbl v1.1, the `$$` machine settings printout used to have nice text descriptions after each one to let you know what you are looking at. In Grbl v1.1, these are gone. Basically, it came down to using nearly a 1KB for these nice settings descriptions or removing a feature like the new laser mode or real-time overrides. As you can tell, the features won! However, to help with this problem of just looking at a whole bunch of confusing numbers, we've asked GUI developers to help out and add these descriptions for users inside their programs. We've added some easy to parse CSV files in the `/doc/csv/` folder that will always be kept updated for them. If your GUI doesn't do that, let them know about the CSV files!

#### Writing individual settings is tedious. Is there a way to speed this up?
Yes, there is a very simple way to write all of your settings at once. Just copy and paste the current settings to a text file, meaning the whole print-out of the '$$' command with labels and all. Grbl will ignore those labels because they are inside '()' comments. Change the values after the '=' characters to whatever you need. Save the file and stream it to Grbl using the 'simple_stream.py' streaming script in the '/script' folder of our repo. (Or, you can use the other streaming script `stream.py` with the `-s` settings-mode flag.) Once it's streamed, all your settings are updated!

#### My Grbl settings and parameters are all funky after flashing Grbl! How do I clear my EEPROM to start from a clean slate?!
After flashing, Grbl tries to check the settings in EEPROM. If it finds an incompatibility, it will automatically clear the EEPROM and restore its default. We try to cover all scenarios within the limited flash space we can devote to checking it, but sometimes we miss something and the data in the EEPROM may be incompatible with the version you have. Or, something went wrong with flashing process and corrupted the EEPROM, which does happen every once in a while. This results in the data having weird numbers and values. In Grbl v0.9j and later, the easiest way to clear out the EEPROM is to use the `$RST=*` command. This will restore the EEPROM space that Grbl uses to their defaults. 

_Before trying this method, check with your CNC manufacturer! They sometimes store some product ID and calibration data in EEPROM!_ For earlier versions of Grbl or to do a complete EEPROM wipe, an Arduino IDE example can do this for you, found in `File->Examples->EEPROM->eeprom_clear`. Change the 512 bytes to 1024 bytes in the for loop and upload it. This should wipe your EEPROM. When you re-flash Grbl, you'll start out from a clean slate. 

#### Homing cycle isn't working right! The movements are all going in wrong directions! Or its not stopping when hitting the limit switch!
It's a very common mistake when users test their hard limit switches and see that Grbl triggers a hard limit alarm. They assume that this means their limit switches are wired correctly. **NOT TRUE**. The Arduino AVR processor does not tell you what state the limit pin is in and which pin triggered (more advanced processors do tell you this). So, Grbl will trigger a hard limit upon any **change** in pin state. So, if you have your limit switches inverted somehow, you will have issues with the homing cycle. 

To check and fix this, Grbl's homing cycle assumes you have set up axes directions correctly. So on a standard mill, this means the positive directions for each axis is: Z-axis spindle moves up, Y-axis table moves toward you (or carriage moves away), and X-axis table moves to the left. Once you have this setup, the homing cycle default `$23=0` direction setting is to start motion in the **positive** direction for all axes, starting with the z-axis and followed by the x-axis and y-axis together. If you have inverted an axis search direction with the `$23` setting, then it will start the search in the **negative** direction.

If it's not moving in the expected direction when you start the homing cycle and then it stops after a short distance (about 2.5 times $27 pull-off distance), then you likely have electrical interference problems. Grbl has detected your limit switch as triggered due the noise and skipped to the pull-off motion phase of the homing cycle. You will need to add noise filtering and shielding to your limit switches. See the [limit switch wiki](https://github.com/gnea/grbl/wiki/Wiring-Limit-Switches) page for more information.

#### Why is Grbl in all negative coordinates after homing? Or it so annoying and not what I'm used to!
When homing is enabled, Grbl sets all of the machine space in negative space. This may seem unusual to people who own 3D printers or newbies, but this is how it has been historically in CNC mills (and due to a misunderstanding how to use work coordinate systems and tool offsets. See below.) 3D printers have a single coordinate system, which is in all positive space. They can do this because it is additive manufacturing, rather than subtractive manufacturing. A 3D printer extruder has a fixed length and nozzle position, and the homing cycle can always reference the tip of the nozzle consistently. Thus, the print job can always start at the print bed from zero to positive-Z on the way up, and it's trivial to set X and Y to positive as well such that all axes are in positive space. 

However, a CNC mill is the opposite. It starts a job from the top of a piece of stock and mills out material on downward, or Z-negative. If you try to setup a mill in all positive space, like a 3D printer, it doesn't work, because you can no longer reference from the tool tip. The main problem is a mill accepts different tools with different lengths and can be chucked inconsistently along their lengths. Every time a tool is changed and touched off to the bed, the machine coordinate frame will change and not stay consistent to the actual machine travel. This is not ok, especially if you need to re-chuck a tool or change a tool for the next operation, because you have then lost your reference system. The only natural and consistent place to set a Z-axis reference point on a CNC mill is to place it at the top of Z-travel, such that Z is all negative and the tool tip is always relative to travel (using G43.1 tool offsets). 

If Z is always negative on a CNC mill due to referencing the Z-travel, rather than the tool tip, it's thought that the XY-axes should also be set in negative space. Either as a way to quickly show the operator which coordinate frame they are looking at, to have CNC machines with XY-tables toward the operator for quick setup, or to have gantry-style CNCs move the tool move away from the operator. The truth is no one really knows why machine space is all negative, rather than a mix of Z-negative and XY-positive. Even my two machinists friends, who have a combined 70 years of experience and have been around since the days of running the very first CNC machines with punch cards or tape reels, don't know why. Just keep in mind that Z-travel will almost always be Z-negative unless you happen to own a tool changer with pre-set tools, each with known offsets from machine travel (most don't because these are very expensive, precision add-ons).

Regardless, how the machine coordinate system is setup (all positive, all negative, or a mix) is meaningless, because CNC mills (and other traditional CNCs) operate primarily in work coordinates systems, where work coordinate systems are simply programmable offsets of the reference machine coordinate system. Unless explicitly told to do so, all motion is performed in the active work coordinate system (Grbl has six G54-G59 and defaults to G54). They are incredibly useful, because their origins can be placed anywhere in the machine and used in many different ways (using the G10L2 and G10L20 commands). For example, you can restart a job at any time and between power cycles to within the accuracy of your limit switches, because your machine coordinate system is repeatable and your work coordinate offsets are saved in EEPROM. Or, since CAM programs generate g-code in terms of a part zero, typically at the top of a piece of stock to be milled, work coordinate systems can easily replicate a job in different areas of the machine with no additional CAM by simple altering the work coordinate offsets between each job. Because of all this, the machine coordinate system is there only to serve as a consistent and accurate machine position to reference from and also to ensure you operate within the valid machine space (soft-limits).

For use cases outside of CNC mills or people annoyed about the machine being in negative space, there is a compile-time option that will force Grbl to set the origin to wherever the homing cycle ends. So, if you'd like a machine in all positive space, simply uncomment the ```HOMING_FORCE_SET_ORIGIN``` option in the config.h file, set your limit switches at the negative ends of travel, and Grbl will be set the origin [0,0,0] upon homing completion. However, keep in mind that if you have a mill, it's highly recommended that you reference the top of Z-travel, not the tool tip, such that travel will be Z-negative for the reasons above. And remember that you can just as easily set your work coordinate system to operate in all positive space with the G10L2 and G10L20 commands!

#### When setting homing pull-off to something greater than zero, why is it crashing INTO my limit switch?! 
The homing pull-off issue is almost always caused by a user wiring in normally-closed limit switches, rather than normally-open switches. Or, the user has inverted their limit switch setting incorrectly for their type of switch. The reason Grbl defaults to requiring normally-open limit switches is purely for the fact of maintaining historical continuity. Either normally-open or normally-closed switch work with Grbl, if they are connected to ground and the limit pins. Arduinos have a nice little internal pull-up resistor already there, so typically you won't need to add another one. Although it's recommended that you add a small capacitor in parallel between the pin and ground to create a simple low-pass filter circuit to help eliminate transient electrical noise.

#### How do I configure my homing cycle to just do 2-axes? Or I have a pen plotter and don't have a Z-axis.
Configuring Grbl for a 2-axis CNC machine is easy. The ```config.h``` file contains a lot of compile-time options that you can enable or disable based on particular needs, including altering the homing cycle to only perform it on the X and Y axes. There are instructions and descriptions for each option available, but some users may not be familiar with C programming, comments, and macros. So, here's how to do it. You'll see two lines in the config.h file like so:
```
#define HOMING_CYCLE_0 (1<<Z_AXIS)                // REQUIRED: First move Z to clear workspace.
#define HOMING_CYCLE_1 ((1<<X_AXIS)|(1<<Y_AXIS))  // OPTIONAL: Then move X,Y at the same time.
```
Change these two lines to look like:
```
#define HOMING_CYCLE_0 (1<<X_AXIS)
#define HOMING_CYCLE_1 (1<<Y_AXIS)
```
That's it! Re-compile and re-flash Grbl via the uploading procedure outlined in the Grbl Wiki. 

#### How do I configure Grbl for a RAMPS board? Or an Arduino stepper shield? 

In short, Grbl is not compatible with RAMPS or Arduino shields (unless they are specifically made for and advertised to work with Grbl). The main reason is the pin layout for Grbl is designed to ensure the maximum performance produced by the Arduino's micro controller. While it's still possible to alter Grbl to work with a RAMPS board, it'll require intimate knowledge of how Grbl works and non-beginner coding skills to do so. At this time, there is no plans to make the 328p version of Grbl RAMPS compatible. However, the Mega2560 version of Grbl may do so, but not in the near future.

## Using Grbl

#### How do I connect and start using Grbl?
Grbl communicates through the serial port, just as in the Arduino IDE. You may connect to it via any standard serial terminal program (i.e Coolterm) at 115200 baud (9600 for v0.8 and prior). Once you are connected, you should be presented with a short message indicating the Grbl version and settings how-to. Just type valid G-code commands followed by an enter and Grbl should respond with an **ok** or an **error:** message. Note: You won't see any character echoes as you type commands to Grbl.

#### How do I stream a complete G-code program to Grbl?
Streaming g-code programs to Grbl may be done by a simple send-and-respond method through the serial port. Every command followed by a return is responded to when Grbl is ready to receive another command. See the **Using Grbl** wiki page for more details, as there are multiple streaming scripts and GUIs available to do this for you.

#### Why can't I just upload a file to Grbl? Or I swear XON/XOFF flow control used to work!
You would think that just uploading a file to Grbl would work, but it won't. This functionality requires some kind of serial flow control to indicate to the computer when the receiver's (Grbl) serial read buffer is full and when it's ready to get more data. The Arduino's hardware flow control lines are hardwired to reset and re-flash the Arduino, not for flow control. XON/XOFF software flow control is not officially supported by Arduinos, but they **used** to work on older Arduinos. This was due to a recent switch in the Arduino's USB-to-serial emulator chips, from FTDI to Atmega. The idea was to allow for people to flash their own firmware onto these emulator chips for their own nefarious needs, where FTDI chips were a closed-platform. This switch inadvertantly removed the FTDI's XON/XOFF software flow control support. As far as we know, there is no push to bring XON/XOFF flow control back. Although, since this firmware is now open-source, it may show up if someone does it or badgers the right people.

#### My stepper drivers require a time delay between the direction pin and step pin settings! (Or I'm noticing that my steppers drift after many many direction changes.) How do I configure this/fix this problem?
This problem comes from Grbl's main stepper algorithm. It sets the direction pin immediately before the stepper pins, which may not occur with enough time in between for certain stepper drivers to acknowledge a change in direction. This can cause a slight stepper drift after many direction changes.
The Grbl master branch has an experimental compile-time option that creates a user-specified time delay after the direction pin is set and before the step pulse is initiated. This is done by enabling another interrupt (Timer2 compare). However, since it does add another interrupt, there is a chance that this can adversely effect Grbl's high-end performance (i.e. high step frequencies or complex curves), but this has not been thoroughly tested to verify this. If everything proves to be solid, we will consider adding this feature in later releases. So, please report any successes or problems with this feature!
There is also a hack/work-around without needing to re-compile. The Grbl invert mask setting not can not only invert your direction pins, but also your stepper pins as well. So instead of being normal low, they can be normal high. Since most stepper drivers acknowledge a step by sensing only a rising(or falling) edge and the other is ignored, you can create a virtual direction pin time delay. Note that now your Grbl pulse microseconds settings will now define this time delay and you will no longer have any control over your step pulse time length (but this shouldn't matter since your stepper drivers shouldn't care.) Although there have been reports that certain stepper drivers don't like to be held normal high for prolonged periods, but it doesn't hurt to try. :)

#### Grbl is acting very weird. Dropping connections, freezing, dropping lines. What's up?
There can be a lot of reasons for this, but the most common is **electromagnetic interference**. If you have AC power lines or your stepper motor wiring near the logic wires or USB cord, the electro-magnetics from these 'high' power lines can cause the logic lines to spike occasionally. This leads to weird connection problems, freezing, etc., etc. Make sure you route these clear of each other and if you can't, try routing them at a 90 degree angle to each other (works with Ethernet cables, should work here as well). Another solution can be placing a small capacitor in parallel with your logic lines creating a high frequency low-pass filter. This will even out some of those spikes. Also, check for grounding. If your setup is poorly grounded, ground loops can cause the logic input voltages to be unstable. Make sure you star-ground wherever you can, especially your motor drivers.

In the past, users reporting this problem have typically noted it has been caused by too many devices on a shared AC circuit, poor grounding, a vacuum motor too close, or a logic line too close to a cooling fan.

Also, if you are dropping lines when streaming, this could be caused by a problem with USB-to-serial drivers on particular computers or environments (Java). It's been reported that the Java-based Universal GcodeSender can drop lines (although reported to have been fixed recently), where the Grbl-supported Python streaming script does not. Please try using the supported streaming script as comparison if you are experiencing this problem.

## G-codes

#### Where are G-codes defined? What does each of them do? 
Grbl tries to follow g-code standard set by the great LinuxCNC project. Click the [link](http://linuxcnc.org/docs/html/gcode.html) to see these always-up-to-date descriptions. There are a few minor differences and are listed on the main Wiki page. Grbl used to use the NIST NGC/RS274 v3 standard for numerical control, aka G-code, but this standard is a bit-outdated and rigid. LinuxCNC is a direct continuation of the NIST standard, if you were curious.

#### Why are some of Grbl's G-codes a little different as on some other CNC machines?
While Grbl follows the LinuxCNC standard, G-code itself is not standardized between all existing CNC machines and manufacturers. Numerical control is pretty old, arcane, and predates MS-DOS. In other words, it's a mess. There have been pushes to standardized it, like by NIST, and it has only been successful for the most of common G-code commands. Yet, there are a few G-codes that are not standardized, such as the G-codes G28/G30, G92.X, etc. It's a goal of Grbl to follow a published standard so that people may build off of it, so as not to muddy the G-code waters even more.

#### Then why do you use LinuxCNC's G-codes rather than only the NIST standard? Aren't you muddying the G-code waters? 
Yes. It's unavoidable, but at least it's muddy water, rather than a gloppy mud paste. The NIST standard only covers the basic functions and some of those are out-dated. There are some really useful G-code functions that other CNC machines and manufacturers use that aren't covered by NIST or even LinuxCNC. We picked one, LinuxCNC, which is user supported, open-source and familiar to hobbyists already. So we think that just as long as we stick to one, everyone's happy.

#### But Grbl has a few different G-codes commands than LinuxCNC? Are you a hypocrite?
Yes. Completely. Also a little selfish. Truthfully, G-codes became mangled because it doesn't cover all cases and scenarios. Over time, users would start to prefer operating a machine in a certain way or machine features would change, where some machines would have it and other wouldn't. Machine manufacturers would alter their G-codes, or make up new ones, to make it easier for their user base or to support their new undefined-by-gcode features. For example, the 3D printer craze created a huge spaghetti hair-ball mess of G-code commands, but most of it is out of necessity due to the new things they had to control such as the extruder and heat controller. Similarly, Grbl tweaked the `M3` and `M4` commands to be specifically suited for laser cutters/engravers while in laser mode. Since lasers are extremely dangerous and prone to starting fires, `M3` and `M4` only turn on while in a `G1 G2 G3` motion mode, strictly for safety. Also, to add Grbl's new dynamic power laser mode, `M4` was used to differentiate the dynamic laser power mode from existing support from laser CAM tools that use `M3` as a constant power mode. It should be noted that CAM developers were contacted prior to this G-code alteration to see if it is kosher. It was.

#### Why would you separate a circle into multiple arcs? Not just draw a full circle?
This comes from a problem of how the arc are defined in G-codes. In radius mode **R**, solving the path for a complete circle or semi-circle will cause severe numerical round-off problems that are unavoidable. This can lead to an error in the tool path. In fact, NIST guidelines state only use **R** mode for arc angles from 0- 165 and 195-345 degrees. Some CNC manufacturers actually don't allow users to draw a complete circle to avoid this problem altogether, limiting users to either a maximum 90 or 180 degree arc motions only. It is good practice to separate all of your arc motions into 90 or 180 degree motions. However, incremental arc mode **I,J** does not have this problem, but it's still good practice to separate your arcs.

#### Grbl seems to be acting weird when streaming when there are G10, G28.1, or G30.1 commands in the program. What's going on?
This has to do with EEPROM writing and how it automatically shuts down all interrupt processes while it's writing, including the serial interrupts. These pauses can happen up to 20 milliseconds, which means that a serial character can be lost within that time period. The G10, G28.1, and G30.1 commands all write to the EEPROM the coordinate offset parameters so that they are persistent between sessions. In practice, you almost never need to update these **in** a G-code program, since they are performed during machine setup and typically hand-coded. If you do need to stream these commands in a program for some reason, there unfortunately isn't really a way to get around this problem. If you come upon a good solution, please let us know.

#### With this EEPROM write issue mentioned above, what data is kept in EEPROM? 
Grbl stores only a few things in EEPROM. The '$$' main settings array, G54-G59 work coordinate offsets, G28/G30 pre-defined positions, $N startup-line strings, and the $I build info user string.