_This wiki is intended to provide information on known bugs or issues. Please feel free to contribute more known bugs and their work arounds as they are discovered._

# Character-counting Streaming Protocol Issues
 
* While the character-counting streaming protocol is indeed very efficient and effective, there are a couple of newly uncovered issues interface writers should aware of.

 * Since the GUI is preloading Grbl's serial RX buffer with commands, Grbl will continually execute all of the queued g-code in the RX serial buffer. The first problem is if there is an error at the beginning of the RX buffer, Grbl will continue to execute the remaining buffered g-code and the GUI won't be able to control what happens. The interim solution is to check all of the g-code via the $C check mode, so all errors are vetted prior to streaming.

 * When Grbl stores data to EEPROM, the AVR requires all interrupts to be disabled during this write process, including the serial RX ISR. This means that if a g-code or Grbl $ command writes to EEPROM, the data sent during the write may be lost. This is usually rare and typically occurs when streaming a G10 command inappropriately inside a program. For robustness, GUIs should track and detect these EEPROM write commands and handle them appropriately by waiting for the queue to finish executing before sending more data. Note that the simple send-response protocol doesn't not suffer from this issue.

# USB to Serial transmission errors

* It has been found that both genuine Arduino boards, and clone Arduino boards experience occasional errors in transmission of data from the host computer to Grbl. The related errors could result in unwanted motion of the machine. The problem was originally discovered on a an Arduino clone that used a CH340G USB to serial chip, but the problem is also present to a lesser degree on genuine Arduino's and clones that use the Atmel 16U2 chip.  There is no fix for the CH340G USB to serial chip. An updated firmware is required to eliminate the problem on the boards using the 16U2 chip. 

* Clone Arduino boards using the CH340G USB to Serial chip experience occasional transmission errors and their use is at your own risk. There is no current fix for this problem on CH340G equipped boards.

* Genuine Arduino boards and clone Arduino boards using the Atmel 16U2 USB to Serial chip also experience occasional transmission errors and it is recommended that users re-flash the 16U2 chip with updated firmware. You can use the instructions here: https://www.arduino.cc/en/Hacking/DFUProgramming8U2 to falsh the new firmware, and the new firmware can be found here:https://github.com/AlexHolden/Arduino/tree/master/hardware/arduino/avr/firmwares/atmegaxxu2/arduino-usbserial  Many thanks to user AlexHolden for taking the time to edit the firmware to solve this problem.

* You can read more about the problem in the issue here: https://github.com/grbl/grbl/issues/845