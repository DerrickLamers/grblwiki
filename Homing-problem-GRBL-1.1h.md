I am setting up a new "Home Brew" laser cutter/engraver, using GRBL, Arduino UNO and CNC shield v3 (clone). The shield has a double Y axis using a cloned A driver in order to drive two Y axis motors. I am using LaserGRBL to control GRBL 1.1h in laser mode (I actually have laser mode turned off during testing so I can do some things that laser mode will not do while not in motion)

I had all the setting done correctly I thought, I could move the machine in all the correct directions and then I came to doing the homing.
I pressed the homing button and all moved in the right directions, Z was OK, Y gave a problem and X didn't complete due to Y having the problem. I checked out the wiring and found a bad connection on the Y switch and that has now been fixed.

The strange problem is that after trying to home, the X and Y axis will now only move in a positive direction, no matter which direction I try to move in. The Z will work OK in both directions. If I try homing again, it will home in the negative direction (as it should), so the motors and drivers seem to be OK.
I reset the program (LaserGRBL) and I even closed the program and restated it again, shut the computer and the machine down and restated them again, but still the problem persists.
If I turn off homing cycle, I can manually (moving by hand) home to the front left hand corner and set X Y zero and all movements are then correct. Once I initiate homing, the problem returns and I can only move in a positive direction, no matter if I try positive or negative moves.

$0=10 (Step pulse time)
$1=25 (Step idle delay)
$2=0 (Step pulse invert)
$3=5 (Step direction invert)
$4=0 (Invert step enable pin)
$5=0 (Invert limit pins)
$6=0 (Invert probe pin)
$10=19 (Status report options)
$11=60.000 (Junction deviation)
$12=0.002 (Arc tolerance)
$13=0 (Report in inches)
$20=0 (Soft limits enable)  Can only enable if $21 =1
$21=0 (Hard limits enable)
$22=0 (Homing cycle enable) I turned this off to do further tests.
$23=3 (Homing direction invert)
$24=1200.000 (Homing locate feed rate)
$25=1000.000 (Homing search seek rate)
$26=144 (Homing switch debounce delay)
$27=3.000 (Homing switch pull-off distance)
$30=1000 (Maximum spindle speed)
$31=0 (Minimum spindle speed)
$32=0 (Laser-mode enable) I turned this off so I could test the laser without motion
$100=40.000 (X-axis travel resolution)
$101=40.000 (Y-axis travel resolution)
$102=80.000 (Z-axis travel resolution)
$110=10000.000 (X-axis maximum rate)
$111=10000.000 (Y-axis maximum rate)
$112=1000.000 (Z-axis maximum rate)
$120=1000.000 (X-axis acceleration)
$121=1000.000 (Y-axis acceleration)
$122=100.000 (Z-axis acceleration)
$130=840.000 (X-axis maximum travel)
$131=420.000 (Y-axis maximum travel)
$132=75.000 (Z-axis maximum travel)
