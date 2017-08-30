_This wiki is intended to provide various instructions on how to compile grbl. Once compiled, you should have a brand new .hex file to flash to your Arduino. Please feel free to contribute more up-to-date or alternative methods._

# Via the Arduino IDE (All Platforms): Recommended for all users.

Thanks to the great people working on the Arduino IDE, it has everything you need to compile grbl included in their [software](http://arduino.cc/en/Main/Software) package. This method compiles the Grbl source code and automatically uploads it to an Arduino. You can't directly flash a pre-compiled .hex file through the IDE interface. See our [Flashing Grbl to an Arduino](https://github.com/grbl/grbl/wiki/Flashing-Grbl-to-an-Arduino) wiki page for how to do this if you only have a .hex file.

_**NOTE: Before starting, delete prior Grbl library installations from the Arduino IDE. Otherwise, you'll have compiling issues! On a Mac, Arduino libraries are located in ```~/Documents/Arduino/libraries/```. On Windows, it's in ```My Documents\Arduino\libraries```.**_

1. Download the Grbl source code.
 * Click on the ```<>Code``` Tab
 * Click the ```Clone or Download``` green button on the Grbl home page.
 * Click the ```Download ZIP```
 * Unzip the download and you'll have a folder called ```grbl-XXX```, where `XXX` is the release version. 
2. Launch the Arduino IDE
 * Make sure you are using the most recent version of the Arduino IDE!
3. Load Grbl into the Arduino IDE as a Library.
 * Click the ```Sketch``` drop-down menu, navigate to ```Include Library``` and select ```Add .ZIP Library```.
 * **IMPORTANT:** Select the ```Grbl``` folder **_inside_** the ```grbl-XXX``` folder, which **only** contains the source files and an example directory.
 * If you accidentally select the `.zip` file or the wrong folder, you will need to navigate to your Arduino library, delete the mistake, and re-do Step 3.
4. Open the `GrblUpload` Arduino example.
 * Click the ```File``` down-down menu, navigate to ```Examples->Grbl```, and select ```GrblUpload```.
5. Compile and upload Grbl to your Arduino.
 * Connect your Arduino Uno to your computer.
 * Make sure your board is set to the Arduino Uno in the ```Tool->Board``` menu and the serial port is selected correctly in ```Tool->Serial Port```. 
 * Click the ```Upload```, and Grbl should compile and flash to your Arduino! (Flashing with a programmer also works by using the ```Upload Using Programmer``` menu command.)

**_Compiling GRBL or Advanced Users:_** Most users are just fine with Grbl's default build, but you can customize Grbl by editing the `config.h` file in the Arduino library (**not where you downloaded it, it has been copied into Arduino Library**) folder. This file enables or disables all of Grbl's additional compile-time options. There are descriptions in the file that explains what they all do. Once edited and saved, just follow the steps above to flash your custom Grbl build!

No fuss! No muss!

_NOTE: If you are having upload issues, try re-burning the Arduino bootloader. If you have a spare Arduino, it's [easy](https://www.arduino.cc/en/Tutorial/ArduinoISP)!_

_Last updated: 2016-12-11_

------
------
# The following methods are for reference only. 

## For Mac OS X: 
_Last updated: 2012-01-29 by chamnit. (Tested on OS X 10.7, 10.6, 10.4 and the Arduino IDE r22,v1.0)_

This method of compiling Grbl uses the Mac OSX terminal and command line to access the Arduino IDE's compilers  without having to use the Arduino IDE. This produces the same firmware as the Arduino IDE method above.

First, you'll need to make sure you have the most up-to-date Arduino IDE version installed on your Mac. The trickiest part is setting up the environment path for the compilers included in the Arduino software. To do this, you'll need to first locate where they are. Depending on where you place your Arduino.app software, this will usually be located in _/Applications/Arduino.app_ for most people. The complete path is then: `/Applications/Arduino.app/Contents/Java/hardware/tools/avr/bin/`

**To add the compiler path:** Open the Terminal.app in /Applications/Utilities. 

Then type: `nano ~/.bashrc` to edit your shell config file. 

Now add this line at the end of the file: `export PATH=$PATH:/Applications/Arduino.app/Contents/Java/hardware/tools/avr/bin/` or whatever your path happens to be.

Press _Crtl-X_ to exit and select _Yes_ to save the file. Now you have added the compiler path. You will need to close the current working window and re-open a new one for the path to be loaded correctly.

NOTE: If you are having problems, you may need to add this same PATH to your .bash_profile file. The process is exactly the same, just switch out the names.

**To compile:** Once your paths are setup, all you will need to do is go to your grbl directory and type `make`. (To clear all of the old compilation files from a previous build, type `make clean` first.) This should call _avr-gcc_, begin compiling grbl, and create a brand new firmware file called _grbl.hex_ that may then be flashed to your Arduino.


 
## For Windows:
_Last updated: 2012-01-28 by txjammer. (Tested on Windows XP and the Arduino IDE r23)_

You can use the Arduino platform as well since it comes with "win-avr" avrgcc. 


**You must add the paths the the executable's** like make.exe and avrdude.exe to windows environment variables.  Right click my computer on the start menu and click Properties. Go to the Advanced tab and on the bottom there will be a button that says environment variables. Under system variables there will be a Variable with the name "Path". Click edit and add the paths to the executable's eg, _C:\arduino-00xx\hardware\tools\avr\bin;C:\arduino-00xx\hardware\tools\avr\avr\bin;C:\arduino-00xx\hardware\tools\avr\utils\bin_ Do not erase your previous paths just add the new ones. Once this is done you can compile the source.  

### For windows 7 and arduino 1.5.7
*Add the following paths to your PATH variable* - be sure to include ; after each one, except the last in your PATH variable entry.

`C:\Program Files (x86)\Arduino\hardware\tools\avr\avr\bin\`

`C:\Program Files (x86)\Arduino\hardware\tools\avr\bin\`

`C:\Program Files (x86)\Arduino\hardware\arduino\sam\system\CMSIS\Examples\cmsis_example\gcc_atmel`

`C:\Program Files (x86)\Arduino`

You will very likely need to restart your computer in order for Windows to recognize the newly added paths.


***

Once your path has been updated, you can open a command prompt. To do so:

Click start, in the run box, type `cmd`  or find the command prompt in your start menu, usually in Start -> Programs -> Accessories.

Change your working directory to the directory that contains the grbl source code:

`cd  C:\grblpath`

type 

`make clean`

This will output something similar to this:

`rm -f grbl.hex main.elf main.o motion_control.o gcode.o spindle_control
t_control.o serial.o protocol.o stepper.o eeprom.o settings.o planner.o
ts.o limits.o print.o probe.o report.o system.o main.d motion_control.d
spindle_control.d coolant_control.d serial.d protocol.d stepper.d eepro
ngs.d planner.d nuts_bolts.d limits.d print.d probe.d report.d system.d`

Type `make grbl.hex` or simply `make`

If all goes well grbl.hex should be created and you can upload to your atmega328p using avrdude. For instructions on how to flash your newly compiled grbl.hex file to your Arduino, see [this wiki entry](https://github.com/grbl/grbl/wiki/Flashing-Grbl-to-an-Arduino#for-windows)

**Ruby is optional**, __but if you don't edit the Makefile you will need to download ruby__ and in the installation settings add the path to environment variables again. Then you can compile the full source with flash calculation. If you don't want to install ruby, edit the Makefile (removing?) everything after ruby (on line 84 only).

**An alternative is to use Atmel Studio**, a customized version of Visual Studio.

_Last update: 2014-07-18 by gerritv (tested on Windows 8.1, 64bit)_

* Install Atmel Studio
* Install the Create From Makefile Extension (Tools/Extension Manager)
* run Tools/Create Project From Makefile
* select the Makefile from your grbl code directory
* Select Device, use ATmega328p for the Arduino Uno
* In Projects/Properties, uncheck Use External Makefile
* Add -DF_CPU=16000000 -mmcu=atmega328p to Project/Properties/Toolchain/AVR Gnu Compiler/Miscellaneous Other Flags

The last 2 steps need to be done for both Debug and Release configurations

Enjoy the benefits of Visual Studio for Atmel/AVR

## For Linux:
_Last updated: 2012-03-02 by speters. (Tested on ???)_

Make sure you have the prerequisite libraries installed: _avr-gcc_ and _arduino_ (_sudo aptitude install arduino_)

At a terminal prompt, change directories to where the _grbl_ source code located. Then type the following to compile and build the firmware:
```
make clean
make grbl.hex
```

## For Arch Linux
_Last updated: 2017-03-26 by brownjohnf. (Tested on 2017-03-06)_

You may encounter an error about a missing `libtinfo.so.5`. I got grbl to compile and run using
the following. **WARNING:** The following is a hack, and there are probably better solutions, but this was
quick and has worked for me.

1. Install `libtinfo` from AUR with `yaourt -S libtinfo`
2. Check to see what you've got installed. In my case, it looked like the following, where you can 
  see that `libtinfo.so.6` is installed, but not `5`.

  ```
  $ ls -l /usr/lib | grep libtinfo
  lrwxrwxrwx  1 root root       22 Mar 26 10:13 libtinfo.so -> /usr/lib/libtinfo.so.6
  lrwxrwxrwx  1 root root       27 Mar 26 10:13 libtinfo.so.6 -> /usr/lib/libncursesw.so.6.0
  ```

3. You can get things working by symlinking `libncursesw.so.6.0` as `libtinfo.so.5`:

  ```
  $ sudo ln -s /usr/lib/libncursesw.so.6.0  /usr/lib/libtinfo.so.5
  ```

## For Ubuntu:
_Last updated: 2014-01-20 by EliteEng._

The following has been tested on Ubuntu 11.10 and an Arduino Uno.  It will compile grbl from source code and flash it to your Arduino.  It should in theory work with other flavours of debian too.

On a brand new ubuntu box, the install process goes like this:

1) install the avr build tools by running:
```
sudo apt-get install arduino-core make unzip
```

2) Compile the GRBL source code and create the firmware file:
```
cd /home ## or a location you want to download the source code to.
wget https://github.com/grbl/grbl/archive/master.zip
unzip master.zip
cd grbl-master
sudo make grbl.hex
```

3) To flash the firmware to your Arduino Uno, plug the Arduino in using the USB cable (Confirm that the device is located at /dev/ttyACM0 and run the following command:
```
sudo PROGRAMMER="-c arduino -P /dev/ttyACM0" make flash
```
That's it, the firmware should now be installed on your Arduino.