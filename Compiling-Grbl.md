_This wiki is intended to provide various instructions on how to compile grbl. Once compiled, you should have a brand new .hex file to flash to your Arduino. Please feel free to contribute more up-to-date or alternative methods._

## For Ubuntu:
_Last updated: 2020-06-15 by Alexandre Melo._

The following has been tested on Ubuntu 18.04 and an Arduino Uno.  It will compile grbl from source code and flash it to your Arduino.

On a brand new ubuntu box, the install process goes like this:

1) install the avr build tools by running:
```
sudo apt-get install arduino-core make unzip
```

2) Compile the GRBL source code and create the firmware file:
```
cd ~/workspace ## or a location you cloned the git repository.
cd grbl
sudo make
```

3) To flash the firmware to your Arduino Uno, plug the Arduino in using the USB cable (Confirm that the device is located at /dev/ttyACM0 and run the following command:
```
sudo PROGRAMMER="-c arduino -P /dev/ttyACM0" make flash
```
That's it, the firmware should now be installed on your Arduino.