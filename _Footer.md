Limit switches: I deliberately use NC switches for a reason. Because whenever switches fail, the failure mode is ALWAYS "open" or "fails to make contact". Simple fact of nature, it's just the way things are.

Assuming a NO switch is used.....
While homing in on a defective switch, you will not know that the switch is malfunctioning, not until after you have crashed your machine. If the switch fails to make contact then the machine crashes.

Assuming a NC switch is used.....
If the switch is bad (in this case the contacts will be open) then no homing occurs, GRBL will return an error and homing does not proceed, and your machine doesn't crash. 

So it does make a BIG difference which switch contact configuration you use for limit switches, NC or NO. Crash or no crash.

Now even if you do use NC contacts, you still need those 104 capacitors (0.1uF), as close to the Arduino as you can place them. You can argue all day that those caps won't make a difference since the caps are shorted out by the switches. The explanation for this phenomenon is quite long but the first powerline glitch will convince you otherwise. (Plug in your blender next to the CNC's AC plug and turn it on. Your CNC should still behave normally despite the blender.)

Side note: With NC switches, the connection is broken cleanly when you hit home position, therefore no contact bounce occurs. (Contact bounce occurs only during switch closure, NOT during switch opening.) 

"This is the way how all professional CNC machines end switches were wired." <--- the explanation above is the reason why.
