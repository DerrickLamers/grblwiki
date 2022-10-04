_This wiki page is intended to provide various instructions on how to use Grbl. Please feel free to contribute and help keep this page up-to-date!_

## Getting Started (For New Users.)
After flashing Grbl to your Arduino, connecting to Grbl is pretty simple. You can use the Arduino IDE itself to connect to Grbl. Experiment or play with it, just to see if you like it. Other serial port programs, such as CoolTerm or PuTTY, work great too. The instructions are pretty much the same. 

* Open up the Arduino IDE and make sure your Arduino with Grbl is connected to your USB port.
* Select the Arduino's Serial Port in the Tools menu, as you would normally with an Arduino.
* Open up the 'Serial Monitor' window from the Tools menu.
* If you are using Grbl v0.9 or later, make sure to change the baud rate from 9600 to 115200.
* Once open, you should see a Grbl welcome message like ```Grbl v0.Xx ['$' for help]```. This means all is good! You're connected!
 * Make sure you change the "No line ending" drop-down menu to "Carriage return". If you are using any other serial port program, you must do the same.
 * If you haven't received the welcome message or some garbled characters, make sure that the baud rate is set at 9600 (or 115200 for v0.9+).

From here, you can simply start sending Grbl some G-code commands, and it'll perform them for you. Or, you can type ```$``` to get some help on what some of Grbl's special commands are or how to write some of your machine settings into Grbl's EEPROM memory.

When have started to feel comfortable with G-code/CNC and you're ready to run a whole G-code program, we recommend that you use one of the many great GUIs that users have written to stream your G-code programs to Grbl and to fully harness all of Grbl's capabilities.

**NOTE:** Check out ShapeOko's [Wiki](http://www.shapeoko.com/wiki/index.php/Communication_/_Control). It has the most up-to-date and comprehensive list of Grbl GUIs.

## How to Stream G-Code Programs to Grbl

### SourceRabbit GCode Sender 4-Axis -  [https://www.sourcerabbit.com](https://www.sourcerabbit.com/Shop/pr-i-88-t-grbl-gcode-sender-4axis.htm) [Win, Mac, Linux]
![screenshot_heightmap_original](https://cdn.sourcerabbit.com/Data/GRBLGNEAGitHubImages/GCodeSender4Axis-MainScreen.png)

CNC control software for **3 & 4-Axis** CNC Mills, Routers, Lasers and Plasma Cutters running with the GRBL Firmware. It features a highly optimized and asynchronous (event-driven) UI and USB-to-Serial communication and can be also used on computers with small amounts of RAM and CPU.

-------
### SourceRabbit GCode Sender - [https://www.sourcerabbit.com](https://www.sourcerabbit.com/Shop/pr-i-80-t-grbl-gcode-sender.htm) [Win, Mac, Linux]
![screenshot_heightmap_original](https://github.com/SourceRabbit/gcode-sender/blob/master/Images/Screenshot.png)

SourceRabbit GCode Sender turns a personal computer into a 3-Axis CNC machine controller. It is cross platform, it features a highly optimized and asynchronous (event-driven) UI and USB-to-Serial communication and can run on computers with small amount of RAM.

**Features:**
- Compatible with GRBL v0.9 and above
- Semi auto tool changes with automatic tool height measurement
- Event-Driven UI and USB-to-Serial communication

-------

### [bCNC] (https://github.com/vlachoudis/bCNC)
![bCNC Screenshot](https://raw.githubusercontent.com/vlachoudis/bCNC/doc/Screenshots/bCNC.png)

An advanced fully featured g-code sender for GRBL. bCNC is a cross platform program (Windows, Linux, Mac) written in python with minimal external dependencies. The sender is robust and fast able to work nicely with old or slow hardware like [Rasperry PI](http://www.openbuilds.com/threads/bcnc-and-the-raspberry-pi.3038/) (As it was validated by the GRBL mainter on heavy testing).

**Features:**
- simple and intuitive interface for small screens
- importing g-code and **dxf** files
- fast g-code sender (works nicely on RPi and old hardware)
- workspace configuration (dialog for G54..G59 commands)
- user configurable buttons
- g-code **function evaluation** with run time expansion
- Easy probing:
  - simple probing
  - center finder with a probing ring
  - **auto leveling**, Z-probing and auto leveling by altering the g-code during
    sending.
  - height color map display
  - **manual tool change** expansion and automatic tool length probing
- Various Tools:
  - user configurable database of materials, endmills, stock
  - properties database of materials, stock, end mills etc..
  - basic **CAM** features (profiling, pocketing, drilling)
  - User g-code plugins:
    - bowl generator
    - finger joint box generator
    - simple spur gear generator
    - spirograph generator
    - surface flatten
    - ...
- G-Code editor and display
    - graphical display of the g-code, and workspace
    - graphically moving and editing g-code
    - reordering code and **rapid motion optimization**
    - moving, rotating, mirroring the g-code
- web pendant to be used via smart phones

### [Universal G-code Sender (UGS)](https://github.com/winder/Universal-G-Code-Sender) [Java Cross-Platform]
![UGS main window](https://github.com/winder/Universal-G-Code-Sender/raw/master/pictures/2.0_platform_ugs_platform.png)

A full-featured GUI, developed by @wwinder, that streams, visualizes G-code, and has complete control and feedback functionality for Grbl's higher level features. It's written in Java, which means it can be run on any Java-capable machine including the RaspberryPi! The Grbl group works closely with this project and highly recommend using this GUI. If you find any issues or would like to request more GUI features, @wwinder has done a wonderful job in completing many of the requests.

Version 2.0 of UGS is required for full GRBL v1.1 compatibility.

--------
### [Easel](http://app.easel.com) [Browser-based CAD + CAM + Grbl controller]

![Easel Screenshot](http://cl.ly/image/0m3Q0w0u0c1F/Screen%20Shot%202014-10-07%20at%205.22.00%20PM.png)

Easel is a web based project developed by Inventables specifically for use with X-Carve, Carvey + Grbl. It is an all-in-one package for design (including SVG imports), toolpath generation, and machine control. It also has an app store where 3rd party developers can make apps that can import into Easel.  Documentation for how to make an app is here (https://discuss.inventables.com/c/easel/app-development).  In addition to 2D design tools, Easel lets you preview your toolpaths in 3D before sending them to your machine. You can also import G-Code into Easel and use it as a sender.  Easel is in constant development by the Inventables team. You can request features or report issues through the feedback button in the app or on the Inventables forum www.inventables.com/forum.

----------

### [GRBLweb](http://andrewhodel.github.io/grblweb/) [Web Browser]

![GRBLweb ui](http://andrewhodel.github.io/grblweb/grblweb_ui_small.png)

GRBLweb is a web based GCODE sender and controller for GRBL. Multiple serial devices can be connected to control multiple machines.

There is also a pre-built Raspberry Pi image based on Raspbian running GRBLweb available [here](http://andrewhodel.github.io/grblweb/).

----------

### GrblPanel [Windows]
![GrblPanel](https://cloud.githubusercontent.com/assets/8202594/4531716/a1f748c2-4d8c-11e4-8cde-bf5f4d1d06a3.JPG)

Maintained by retired computer professional @gerritv, **GrblPanel** is a GUI that implements more advanced features and functionality commonly found in production machines like Haas, Fanuc, etc. All of the required tools for setting up and running a milling job are neatly arranged and designed to be easily accessible based on decades-old accepted workflows in machine shops. **GrblPanel** currently only works in Windows via .Net v4.5, but will eventually be updated for cross-platform use through [Mono](http://www.mono-project.com).


-----------

### [Candle](https://github.com/Denvi/Candle) [Windows/Linux]
![screenshot_heightmap_original](https://cloud.githubusercontent.com/assets/5119037/9604203/7e979a52-50cf-11e5-8b10-23e54977a782.png)

GUI application for GRBL-based CNC-machines with G-Code visualizer.

Supported functions:
* Controlling GRBL-based cnc-machine via console commands, buttons on form, numpad.
* Monitoring cnc-machine state.
* Loading, editing, saving and sending of G-code files to cnc-machine.
* Visualizing G-code files.
* Autoleveling Z-axis for PCB milling.

-----------

### Python Streaming Scripts _(Officially Supported by Grbl)_ [Cross-Platform]

__NOTE: If you are having difficulties with streaming to Grbl, we will ask you to use this Python streaming script to eliminate the GUI you are using as the source of the issue. Before posting to the issues thread, please use this script to run your G-code program.__

Included with the source code and officially supported by Grbl, two Python streaming scripts are supplied to illustrate simple and more complex streaming methods that work well cross-platform. These scripts don't fully support all of the Grbl's features, but are intended more as a way to compare or troubleshoot other garden variety or newly-written GUIs out there. These are located in the 'script' folder on the main repository. **Note:** The streaming scripts require the [pySerial](http://pyserial.sourceforge.net) module installed.

* Install the [pySerial](http://pyserial.sourceforge.net) module.
* Download [simple_stream.py](https://github.com/grbl/grbl/blob/master/doc/script/simple_stream.py) Python script.
* Open the script in a plain text editor and change the following line to reflect your system:  

`s = serial.Serial('/dev/tty.usbmodem1811',9600)`  

* In place of **/dev/tty.usbmodem1811**(Mac), you should put the serial port device name of your Arduino. This will be different for each machine and OS. For example, on a Linux system this would look like **/dev/ttyACM0**. Or on a Windows machine, this may look like **COM3**. 
* The script looks for and reads gcode from a file named **grbl.gcode**, you should create this file and put the gcode you want to execute in it. Or simply change this name in the script to your needs.
* Open a terminal/command window and change directories to the location of the Python script and execute the Python script with the following command:  

`./simple_stream.py`  (Mac/Linux)
`python simple_stream.py` (Windows)

* You should now see the gcode being streamed to grbl along with 'ok' messages and your machine should begin moving.

The other, more advanced streaming script **stream.py** has command line arguments and does not require modifying the script itself, unlike **simple_stream.py**. The main difference is that **stream.py** uses a character counting scheme to ensure that Grbl's serial read buffer is full, which effectively creates another buffer layer on top of Grbl's internal motion queue. This allows for Grbl to access and parse the next G-code block immediately from the serial read buffer, rather than wait for the 'ok' send and response in the **simple_stream.py** script. This is very useful for motions, like curves, that have very rapid, short line segments in succession that may cause buffer starvation, which can lead to strange motion hiccups. In other words, it ensures a smoother motion. Use this script, if you are not afraid of command line or are experiencing weird motions.

-------

### Other GUIs

#### grblUI
A simple graphical user interface: https://github.com/jgeisler0303/grblUI. Programmed in Java, using rxtx for serial communication. Should theoretically run on Linux, Mac and Windows alike. Apparently some problems on Mac. Any feedback, tips and tricks appreciated (Issues or Wiki in grblUI). Check out the ready to use jar in the Downloads.

#### grblgui
A graphical G-Code Streamer: https://github.com/cody82/grblgui. Programmed in Java, using rxtx for serial communication and OpenGL 2.0 for rendering.

Notable features:
* It displays the job duration and remaining time to complete in minutes.
* It displays current speed.
* You can toggle feed hold and enter G-Code commands.
* It displays the buffer status graphically on the toolpath!

In development:
* Simulate the milling process and display the resulting model.


#### CNCinfusion [Windows]
Currently under development in C#   https://github.com/nm156/CNCInfusion

#### Gcode Sender [Windows] 
https://github.com/downloads/OttoHermansson/GcodeSender/gcodesender.exe
http://www.contraptor.org/forum/t-287260/gcode-sender-program

#### LaserGRBL [Windows]
Laser optimized GUI for Grbl http://lasergrbl.com
- Simple & Minimal Interface designed for Grbl v1.1
- jpg,bmp,png Image import (Image Vectorization, Grayscale Lines, Dithering 1bit)
- 2D Graphic preview for engraving/cutting (with grayscale mapping)
- Easy-To-Use Overrides control
- User defined buttons, power to you!
- Grbl Configuration Import/Export
- Configuration, Alarm and Error codes decoding for Grbl v1.1 (with description tooltip)
- Homing button, Feed Hold button, Resume button and Grbl Reset button
- Job time preview and realtime projection
- Jogging (for any Grbl version)
- Feed overrides (for Grbl > v1.1) with easy-to-use interface
- Multilangual: english, italian, german, french, spanish, danish and brazilian

![LaserGRBL_preview](https://cloud.githubusercontent.com/assets/8782035/21350108/ed218200-c6b5-11e6-9712-21de7bf7a1ea.jpg)

[<img src="https://cloud.githubusercontent.com/assets/8782035/23578353/fba95768-00d4-11e7-9357-99c00a30631d.jpg">](https://www.youtube.com/watch?v=Uk2fGoNL3Yk&list=PLABqghG_gwj1-62OBAymMF9-KBXPyrW0P&index=1)

#### Image2GCode [Windows]
**image2gcode** is a free open source program for Windows designed to generate G-code from raster images for laser and nichrome burners.

<https://www.image2gcode.ru>

![image2gcode v3.x](https://www.image2gcode.ru/images/image2gcode.png)

#### GRBL-Plotter [Windows]
https://github.com/svenhb/GRBL-Plotter  
* Check videos on YouTube ['GRBL-Plotter'](https://www.youtube.com/results?search_query=grbl-plotter)  
* Supporting GRBL 1.1 (and 0.9 also)  
* Import/creation and conversion into GCode 
  - from SVG and DXF Graphics
  - from [Images](https://github.com/svenhb/GRBL-Plotter/wiki/Image-import)  
  - from Eagle Drill file
  - from Text (into Hershey Font)
  - conversion of the Z-Dimension into Z-axis (router) or Spindle on/off (laser) or Spindle-Speed (RC-Servo PWM) 
* Export / import machine specific settings (Joystick, Buttons)
* Controlling a 2nd GRBL-Hardware
* Tool exchange [Video](https://youtu.be/GGtdwYdZWi8)  
* User defined Buttons
* Joystick like control
* GamePad support
    
Screenshot of Main GUI  
![GRBL-Plotter GUI](https://raw.githubusercontent.com/svenhb/GRBL-Plotter/master/doc/GRBLPlotter_GUI.png) 
   
#### [Grbl Overseer](https://gitlab.com/Pilatomic/grbl-overseer) [Multiplatform Desktop + Android]
Touch-friendly user interface with multiple jobs scheduling 
* Simple, easy to use, touch-friendly user interface
* 3D view of jobs and current tool position
* Schedule multiple jobs, each has its specified origin
* Automatically executes a simulation run before production, and compiles all errors
* Adaptative jog controls (the longer you press, the faster it goes)
* Smart serial console, GRBL message / responses are grouped with the corresponding command
* Smart top bar, always showing the current GRBL status. Background color changes with status to allow easy state reading even far from the device
* Built-in editor for GRBL configuration
* Multiplatform (tested on Windows, Linux and Android)
* Support USB / serial interface on > Android 3.1 devices with USB API
* Support Grbl >= v1.1

Screenshot : 

![Grbl Overseer screenshot](https://gitlab.com/Pilatomic/grbl-overseer/raw/master/doc/screenshot_jobs_panel.png)

Developement is still ongoing, please report any issue you encounter

#### [Grbl Controller](https://play.google.com/store/apps/details?id=in.co.gorest.grblcontroller) [Android Mobile Application]

Compact user interface even for small screen mobile

* Support Grbl >= v1.1
* Supports real time overrides, feed rate, spindle speed, and toggle coolant.
* Real time machine position, feed, buffer state reporting. (you need to enable buffer data in status report via $10=2)
* Supports Sending G-Code files from mobile. (supported extensions are .gcode .nc and .tap)
* Supports short text commands.
* Auto adjusts Z-Axis on work surface using G38.3 probing.
* Highly Configurable 4 Custom Buttons.

![Axis Control](https://raw.githubusercontent.com/zeevy/grblcontroller/master/doc/screenshots/resized/Screenshot_20171001-090425.png) ![File Streaming](https://raw.githubusercontent.com/zeevy/grblcontroller/master/doc/screenshots/resized/Screenshot_20171001-090518.png)

#### [github](https://github.com/zeevy/grblcontroller)
-------

## Serial Emulators:

Other than CoolTerm or PuTTY, Linux and Mac systems have a great lightweight serial emulator called `screen` that's either built in or easily installable (apt-get install screen) through the terminal interface.

If your device is connected on /dev/ttyACM0 (for Mac, /dev/tty.usbxxxx), type `screen /dev/ttyACM0 115200` to connect to the device at 115200 baud. There you'll be connected to Grbl. To exit

To get out of the screen interface, simply press `Ctrl-a` followed by a `k`. 

***

# Diba controller [windows]
![screenshot](http://s8.picofile.com/file/8299838934/Diba_screen.png)

GUI application for GRBL 0.9J with 2D G-Code visualizer.
Contact us : diba.team2017@gmail.com
download : [Diba controller 2.5.01](https://www.mediafire.com/file/ok7nc1t8em983b3/Diba+controller+2.5.01.rar)

# Zen Toolworks Controller (Zen CNC) (Under heavy development) 

Contact Us: info@zencnc.com

![](https://github.com/zentoolworks/zencnc/blob/master/doc/images/zencnc_01.jpg)

This application is currently only available on Windows Platform, (Windows 7 and 10 Tested)

The GUI provide very basic functions, such as Digital Read Out, Position Set To and Move To. X, Y and Z Jogging, Zero and Homing. Easy Grbl Parameter Editing.

All other functions are based on Plugin Architecture. You can basically extend the functionality by defining your own panels and buttons. 

![](https://github.com/zentoolworks/zencnc/blob/master/doc/images/ZTWTEST_01.JPG)

Zen Toolworks also developed a Grbl Test Board, which can be used combined with our Zen CNC Application to test all features provided by Grbl. 

[ZenCNC Wiki Page](https://github.com/zentoolworks/zencnc/wiki)

***

# Grbl4P: gui implemented in Processing 3

![Grbl4P screen shot](https://github.com/TPMoyer/Grbl4P/blob/master/Grbl4P_0.5.png) 

Front end gui panel interface to grbl, within the Processing 3 framework.  
Supports g-code file streaming and mouse/keyboard interaction.  

Written to enable easy extensions for Java and/or Scala development of tightly coupled motion Control apps. 

All code provided is java, using only the Processing 3 included IDE. All GUI controls are from the Processing G4P Library, and were created with the G4P GUI Builder tool.

#### [github](https://github.com/TPMoyer/Grbl4P)

 ***

# GRBLMASTER: Gui created in Processing 4

Early alpha release

![GRBLMASTER](https://raw.githubusercontent.com/carcanhol/cnc-interface/main/GRBLMASTER.png?token=GHSAT0AAAAAABZRXQ6AMDHVY2AYYU6ELOCMYZ4YQSA)

[Blog Download Links](https://grblmaster.blogspot.com)


