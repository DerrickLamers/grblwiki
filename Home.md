***
_Welcome to the Grbl wiki! Please feel free to modify these pages to help keep Grbl up-to-date!_

### About Grbl
Grbl is a free, open source, high performance software for controlling the motion of machines that move, that make things, or that make things move, and will run on a straight Arduino. If the maker movement was an industry, Grbl would be the industry standard.

Most open source 3D printers have Grbl in their hearts. It has been adapted for use in hundreds of projects including laser cutters, automatic hand writers, hole drillers, graffiti painters and oddball drawing machines. Due to its performance, simplicity and frugal hardware requirements Grbl has grown into a little open source phenomenon.

In 2009, **Simen Svale Skogsrud** (**http://bengler.no/grbl**) graced the open-source community by writing and releasing the early versions of Grbl to everyone ([inspired by the Arduino GCode Interpreter by Mike Ellery](http://www.contraptor.org/grbl-gcode-interpreter)). Since 2011, Grbl is pushing ahead as a community-driven open-source project under the pragmatic leadership of **Sungeun "Sonny" Jeon Ph.D.** (**@chamnit**).

-----
### Official Supporters of the Grbl CNC Project

<p align="center">![Official Supporters](https://github.com/gnea/gnea-Media/blob/master/Contributors.png?raw=true)
</p>_Grbl is an open-source project and fueled by the free-time of our intrepid administrators and altruistic users. If you'd like to donate, all proceeds will be used to help keep this project sustainable and to fund supporting equipment. Thank you!_

[![Donate](https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=EBQWAWQAAT878) 

-----

### Who should use Grbl

Makers who do milling or laser cutting and need a nice, simple controller for their system that will run on   the ubiquitous Arduino Uno. People who loathe to clutter their space with legacy PC-towers just for the parallel-port. Tinkerers who need a controller written in tidy, modular C as a basis for their project.

###Nice features

Grbl is great for light duty production. We use it for all our milling, running it from our laptops or Raspberry Pis using superb GUIs written for Grbl to stream G-code jobs. Grbl is written in highly optimized C utilizing all the clever features of the Arduino's Atmega328p chips to achieve precise timing and asynchronous operation. It is able to maintain more than **30kHz** step rate and delivers a clean, jitter free stream of control pulses.

Grbl is for three axis machines. No rotation axes (yet) – just X, Y, and Z.

The G-code interpreter implements a subset of the LinuxCNC standard and is supported by most CAM-tools with no issues. For descriptions of these G-codes, see LinuxCNC's superb documentation for their G-code descriptions, [(G-code Quick Reference)](http://linuxcnc.org/docs/html/gcode.html), and the [Shapeoko wiki](http://www.shapeoko.com/wiki/index.php/G-Code) which attempts to list all codes supported by Grbl with appropriate commentary. Note that there are only two deviations from the written G-code standard listed below. If you notice any other discrepancies, please let use know!
- Multiple full circle arcs with G2 and G3 arcs with a P word is not supported.
- Laser mode alters the operation of M3, M4, and spindle speed S word changes. See the [Laser Mode](https://github.com/gnea/grbl/wiki/Grbl-v1.1-Laser-Mode) page for details.

* Supported G-Codes in **v1.1**
 * **G0, G1:** _Linear Motions_
 * **G2, G3:** _Arc and Helical Motions_ 
 * **G4:** _Dwell_ 
 * **G10 L2, G10 L20:** _Set Work Coordinate Offsets_
 * **G17, G18, G19:** _Plane Selection_ 
 * **G20, G21:** _Units_ 
 * **G28, G30:** _Go to Pre-Defined Position_ 
 * **G28.1, G30.1:** _Set Pre-Defined Position_
 * **G38.2:** _Probing_ 
 * **G38.3, G38.4, G38.5:** _Probing_ 
 * **G40:** _Cutter Radius Compensation Modes_
 * **G43.1, G49:** _Dynamic Tool Length Offsets_
 * **G53:** _Move in Absolute Coordinates_
 * **G54, G55, G56, G57, G58, G59:** _Work Coordinate Systems_
 * **G61:** _Path Control Modes_
 * **G80:** _Motion Mode Cancel_ 
 * **G90, G91:** _Distance Modes_ 
 * **G91.1:** _Arc IJK Distance Modes_
 * **G92:** _Coordinate Offset_ 
 * **G92.1:** _Clear Coordinate System Offsets_ 
 * **G93, G94:** _Feedrate Modes_ 
 * **M0, M2, M30:** _Program Pause and End_ 
 * **M3, M4, M5:** _Spindle Control_ 
 * **M8, M9:** _Coolant Control_ 

###Acceleration management

In the early days, Arduino-based CNC controllers did not have acceleration planning and couldn't run at full speed without some kind of easing. Grbl’s constant acceleration-management with look ahead planner solved this issue and has been replicated everywhere in the micro controller CNC world, from Marlin to TinyG. Grbl intentionally uses a simpler constant acceleration model, which is more than adequate for home CNC use. Because of this, we were able to invest our time optimizing our planning algorithms and making sure motions are solid and reliable. When the installation of all the feature sets we think are critical are complete and no longer requires us to modify our planner to accommodate them, we intend to research and implement more-advanced motion control algorithms, which are usually reserved for machines only with very high feed rates (i.e. pick-and-place) or in production environments. Lastly, here's a [link](http://onehossshay.wordpress.com/2011/09/24/improving_grbl_cornering_algorithm/) describing the basis of our high speed cornering algorithm so motions ease into the fastest feed rates and brake before sharp corners for fast, yet jerk free operation. 

###Limitations by design

We have limited G-code-support by design. This keeps the Grbl source code simple, lightweight, and flexible, as we continue to develop, improve, and maintain stability with each new feature. Grbl supports all the common operations encountered in output from CAM-tools, but leave some human G-coders frustrated. No variables, no tool databases, no functions, no canned cycles, no arithmetic and no control structures. Just the basic machine operations and capabilities. Anything more complex, we think interfaces can handle those quite easily and translate them for Grbl.

### New features in v1.1!
Another **HUGE** update! This version includes the last remaining priority features that have been long been on the to-do list. Here's a summary of the new changes:
* **Real-time Overrides** : Alters the machine running state immediately with feed, rapid, spindle speed, spindle stop, and coolant toggle controls. This awesome new feature is common only on industrial machines, often used to optimize speeds and feeds while a job is running. Most hobby CNCs try to mimic this behavior, but usually have large amounts of lag. Grbl executes overrides in realtime and within tens of milliseconds.
* **Jogging Mode** : The new jogging commands are independent of the G-code parser, so that the parser state doesn't get altered and cause a potential crash if not restored properly. Documentation is included on how this works and how it can be used to control your machine via a joystick or rotary dial with a low-latency, satisfying response.
* **Laser Mode** : The new "laser" mode will cause Grbl to move continuously through consecutive G1, G2, and G3 commands with spindle speed changes. When "laser" mode is disabled, Grbl will instead come to a stop to ensure a spindle comes up to speed properly. Spindle speed overrides also work with laser mode so you can tweak the laser power, if you need to during the job. Switch between "laser" mode and "normal" mode via a $ setting.
* **Dynamic Laser Power Scaling with Speed** : If your machine has low accelerations, this option will automagically scale the laser power based on how fast Grbl is traveling, so you won't have burnt corners when your CNC has to make a turn! Enabled by the `M4` spindle CCW command when laser mode is enabled.
* **Sleep Mode** : Grbl may now be put to "sleep" via a $SLP command. This will disable everything, including the stepper drivers. Nice to have when you are leaving your machine unattended and want to power down everything automatically. Only a reset exits the sleep state.
* **Significant Interface Improvements**: Tweaked to increase overall performance, include lots more real-time data, and to simplify maintaining and writing GUIs. Based on direct feedback from multiple GUI developers and bench performance testing. NOTE: GUIs need to specifically update their code to be compatible with v1.1 and later.
* **New Status Reports**: To account for the additional override data, status reports have been tweaked to cram more data into it, while still being smaller than before. Documentation is included, outlining how it has been changed.
Improved Error/Alarm Feedback : All Grbl error and alarm messages have been changed to providing a code. Each code is associated with a specific problem, so users will know exactly what is wrong without having to guess. Documentation and an easy to parse CSV is included in the repo.
* **Extended-ASCII realtime commands** : All overrides and future real-time commands are defined in the extended-ASCII character space. Unfortunately not easily type-able on a keyboard, but helps prevent accidental commands from a G-code file having these characters and gives lots of space for future expansion.
* **Message Prefixes** : Every message type from Grbl has a unique prefix to help GUIs immediately determine what the message is and parse it accordingly without having to know context. The prior interface had several instances of GUIs having to figure out the meaning of a message, which made everything more complicated than it needed to be.
New OEM specific features, such as safety door parking, single configuration file build option, EEPROM restrictions and restoring controls, and storing product data information.
* **New safety door parking motion as a compile-option**. Grbl will retract, disable the spindle/coolant, and park near Z max. When resumed, it will perform these tasks in reverse order and continue the program. Highly configurable, even to add more than one parking motion. See config.h for details.
* **New '$' Grbl settings for max and min spindle rpm**. Allows for tweaking the PWM output to more closely match true spindle rpm. When max rpm is set to zero or less than min rpm, the PWM pin D11 will act like a simple enable on/off output.
* **Updated G28 and G30 behavior** from NIST to LinuxCNC G-code description. In short, if an intermediate motion is specified, only the axes specified will move to the stored coordinates, not all axes as before.
* Lots of minor bug fixes and refactoring to make the code more efficient and flexible.
* NOTE: Arduino Mega2560 support has been moved to an active, official Grbl-Mega project. All new developments here and there will be synced when it makes sense to.

### New features in v0.9!
* **IMPORTANT:**
 * **Default serial baudrate is now 115200! (Up from 9600)**
 * **Z-Axis limit input on D11 has swapped with spindle enable D12 to support variable spindle PWM output.**
 * **No QUEUE state: Queue was removed due to it being redundant. Holds now suspend Grbl and only allow realtime commands. Cycle start resumes, and reset exits.**
* **_NEW_ Super Smooth Stepper Algorithm:**  Complete overhaul of the handling of the stepper driver to simplify and reduce task time per ISR tick. Much smoother operation with the new Adaptive Multi-Axis Step Smoothing (AMASS) algorithm which does what its name implies (see stepper.c source for details). Users should immediately see significant improvements in how their machines move and overall performance!
* **Stability and Robustness Updates:** Grbl's overall stability has been focused on for this version. The planner and step-execution interface has been completely re-written for robustness and incorruptibility by the introduction of an intermediate step segment buffer that "checks-out" steps from the planner buffer in real-time. This means we can now fearlessly drive Grbl to its highest limits. Combined with the new stepper algorithm and planner optimizations, this translated to **5x to 10x** overall performance increases in our testing! Also, stability and robustness tests have been reported to easily take 1.4 million (yes, **million**) line G-code programs like a champ!
* **(x4)+ Faster Planner:** Planning computations improved four-fold or more by optimizing end-to-end operations, which included streamlining the computations and introducing a planner pointer to locate un-improvable portions of the buffer and not waste cycles recomputing them.
* **Variable Spindle Speed Output:** Enables a hardware PWM output for 'S' G-code commands. **NOTE:** This feature requires a pin swap with the Z-limit D11 pin and spindle enable D12 pin to access the hardware PWM on pin D12. The Z-limit pin, now on D12, should work just as it did before.
* **Compile-able via Arduino IDE!:** Grbl's source code may now be downloaded and altered, and then be compiled and flashed directly through the Arduino IDE, which should work on all platforms. See the Wiki for details on how to do it.
* **G-Code Parser Overhaul:** Completely re-written from the ground-up for 100%-compliance* to the G-code standard. (* Parts of the NIST standard are a bit out-dated and arbitrary, so we altered some minor things to make more sense. Differences are outlined in the source code.) We also took steps to allow us to break up the G-code parser into distinct separate tasks, which is key for some future development ideas and improvements.
* **Independent Acceleration and Velocity Settings:** Each axis may be defined with unique acceleration and velocity parameters and Grbl will automagically calculate the maximum acceleration and velocity through a path depending on the direction traveled.
* **Soft Limits:** Checks if any motion command exceeds workspace limits before executing it, and alarms out, if detected. 
* **Probing:** The G38.2, G38.3, G38.4, & G38.5 straight probe G-code commands are now supported and connected through the A5 pin.
* **Tool Length Offsets:** Probing doesn't make sense without tool length offsets (TLO), so we added it! The G43.1 dynamic TLO (described by linuxcnc.org) and G49 TLO cancel commands are now supported. G43.1 dynamic TLO works like the normal G43 TLO (NOT SUPPORTED) but requires an additional axis word with the offset value attached. We did this so Grbl does not have to track and maintain a tool offset database in its memory. Perhaps in the future, we will support a tool database, but not for this version.
* **Improved Arc Performance:** The larger the arc radius, the faster Grbl will trace it! We are now defining arcs in terms of arc chordal tolerance, rather than a fixed segment length. This automatically scales the arc segment length such that maximum radial error of the segment from the true arc is never more than the chordal tolerance value of a super-accurate default of 0.002 mm.
* **New Grbl SIMULATOR! (by @jgeisler and @ashelly):** A completely independent wrapper of the Grbl main source code that may be compiled as an executable on a computer. No Arduino required. Simply simulates the responses of Grbl as if it was on an Arduino.
* **CPU Pin Mapping:** In an effort for Grbl to be compatible with other AVR architectures, such as the 1280 or 2560, a new cpu_map.h pin configuration file has been created to allow Grbl to be compiled for them. This is currently user supported, so your mileage may vary. If you run across a bug, please let us know or better, send us a fix! Thanks in advance!
* **Configurable Real-time Status Reporting:** Users can now customize the type of real-time data Grbl reports back when they issue a '?' status report. This includes data such as: machine position, work position, planner buffer usage, serial RX buffer usage.
* **Updated Homing Routine:** Sets workspace volume in all negative space regardless of limit switch position. Common on pro CNCs. But, the behavior may be changed by a compile-time option though. Now tied directly into the main planner and stepper modules to reduce flash space and allow maximum speeds during seeking.
* **CoreXY Support:** Grbl now supports CoreXY kinematics on an introductory-level. Most functions have been verified to work, but there may be bugs here or there. Please report any problems you find!
* **Safety Door Support:** Safety door switches are now supported. Grbl will force a feed hold, shutdown the spindle and coolant, and wait until the door switch has closed and the user has issued a resume. Upon resuming, the spindle and coolant will re-energize after a configurable delay and continue. Useful for OEMs that require this feature.
* **Full Limit and Control Pin Configurability:** Limits and control pins operation can now be interpreted by Grbl however you'd like, with the internal pull-up resistors enabled or disabled, or reading a high or low as a trigger. This should cover all wiring and NO or NC switch scenarios.
* **Optional Limit Pin Sharing:** Limit switches can be combined to share the same pins to free up precious I/O pins for other purposes. When combined, users must adjust the homing cycle mask in config.h to not home the axes on a shared pin at the same time. Don't worry; hard limits and the homing cycle still work just like they did before.
* **Additional Compile-Time Feature Options:** Limit/control pin state reporting, line number tracking, real-time feed rate reporting, and more.


### New features in v0.8!
A lot has happened since the v0.7. We're pushing real hard to create a simple, yet powerful CNC controller for the venerable Arduino. Here's a list of the new things that have come to v0.8.

* **Multi-Tasking Run-time Commands:** Feed hold with controlled deceleration for no loss of location, Resume after feed hold, Reset, and Status Reporting.
* **Advanced Homing Cycle:** Lots of configuration options for different types of machines from which axes to move when to their search directions. Limit switches may also be used as hard limits as well.
* **Persistent Coordinate System Data:** Work Coordinate Systems (G54-G59) and Pre-Defined Positions (G28,G30) are held in EEPROM, so they'll always be set as you last set them.
* **Check G-Code Mode:** Sets up Grbl to run through your program without moving anything, so you can check whether or not you have any errors that Grbl may not like.
* **Improved Feedback:** Reports real-time position, what Grbl is doing, the G-code parser state, and stored coordinate offset values.
* **Startup blocks:** Auto-magically runs user G-code blocks at startup or reset. Can be used to set your defaults.
* **Pin-outs:** Cycle start, feed hold, and abort are now pinned-out to the A0, A1, and A2 pins. Just connect a normally-open switch to the pin and ground. That's it!
* More Robust G-Code Parser with Error Checking Feedback.
* And much more!

***
_Grbl v0.8 (and prior) is distributed under the MIT-License and was developed by Simen Svale Skogsrud, Sungeun K. Jeon Ph.D., and Jens Geisler._

_Grbl is distributed under the GPLv3 license and is developed by Sungeun K. Jeon Ph.D.. See [Licensing](https://github.com/gnea/grbl/wiki/Licensing) for more details._
***

## Documentation for Previous Versions

* **[Old Wiki Homepage](https://github.com/grbl/grbl/wiki)**

* **[Configuring Grbl v0.9](https://github.com/grbl/grbl/wiki/Configuring-Grbl-v0.9)**

* **[Configuring Grbl v0.8](https://github.com/grbl/grbl/wiki/Configuring-Grbl-v0.8)**

* **[Configuring Grbl v0.7](https://github.com/grbl/grbl/wiki/Configuring-Grbl-v0.7)**