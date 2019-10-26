Hello everyone we have a project that is CNC Plasma Cutter, and We are using the software "Universal G-Code Sender" which is connected with the Arduino UNO. We are also using the Fusion 360 application from Autodesk for designing the output to be traced and generating the G-code.

How can I automatically control the firing of the plasma cutter when the operation starts (the torch starts to move, tracing the drawing) and then turn off when the operation ends?

Need help guys.



I found a relay very helpful.  The beefcake relay from sparkfunk.  The fire button on the plasma would be controled by the chosen controller (arduiono uno) via a relay.  The power to the plasma torch would be seperate and independent to the plasma controls.  You have power outlet to powerbox 120 to 24volts and the 240volt to the plasma torch but the two wires to the fire button would be controled by the arduino controler by way of relay (normal on, comm).
Also, a well constructed toolpath is needed from software to the UG sender.

Cheers