# Method 1: Movable and Fixed Tool Touch Off

## Description

![](/images/pa065.PNG)

This method uses two Tool Touch Off Plates (TT). The movable TT is being placed on top of the workpiece to find the exact position of the top surface,
the second TT is installed in a fixed location and is being used to automatically touch off all subsequent tools on the same workpiece.

Three options can be selected on the bottom right to customize the probing cycle:

* *Test TT (Movable TT only):* This will add a TT Trigger Check before the probing of the Movable TT is executed to assure that the TT is functioning properly.
* *Fast Probing Speed First:* This check box will always default to the selected configuration setting and allows to temporarely enable or disable the fast probing move.
* *Use WCS#:* This allows to select the WCS# that's being reset by this probing cycle. Default is always the currently active WCS#. The machine will remain in the currently active WCS and only switches quickly to the selected WCS for the Reset. 

This method has three cycle options. Read  the description for each option carefully and make sure you select the appropriate cycle for each Tool Change at the right time.


### Cycle 1: Probe Z0 and Reference Height Offset 

![](/images/pa070.PNG)

This cycle is being used on the first Tool Change after a new workpiece has been mounted that has an unknown top surface position. *Cycle 1* involves two probing cycles.

The first probing cycle is using the movable TT on top of the workpiece and will measure the exact position of the top surface and will also set Z0 on top of the surface for the first tool.

The second probing cycle is using the Fixed TT and is needed to calculate the *Reference Height Offset* which is the distance between the top of the Fixed TT and the top of the workpiece.

The Reference Height Offset is used to correctly set the Tool Height Offset for subsequent tool changes on the same workpiece by using the Fixed TT only.



### Cycle 2: Set Z0 with Fixed TT - Ref Height Offset 

![](/images/pa071.PNG)

After a *Reference Height Offset* has been established with *Cycle 1*, all subsequent Tool Changes on the same workpiece can be done with the Fixed TT only.

Use this option for all subsequent Tool Changes on the same workpiece after the *Reference Tool Height* has been set with *Cycle 1*.

The currently active *Reference Height Offset* is displayed in the box and can be manually adjusted if necessary.

Three options can be selected on the bottom right to customize the probing cycle (see above for details).

### Cycle 3: Continue without Tool Probing 

![](/images/pa072.PNG)

If the Tool Setter screen is being launched by a M6 Tool Change command but no Tool Touch Off is needed, the *Cycle 3* button allows to continue the job without a Tool Touch Off.

### Configuration for Movable and Fixed TT Method

![](/images/pa066.PNG)

The Configuration screen allows for a very flexible customization of the probing cycle. 
These options can be changed at any time and the ProbeApp will dynamically generate the necessary probing commands for CNC12 based on current configuration settings. 

Check out the details below for each of these options.

![](/images/pa074.PNG)

If this option is checked, CNC12 will display a Message before the probing move with the Movable TT is executed. 
The message can be customized in the *Message* box (adding \n does create a new line in the message box). 

Be aware that not checking this box does directly engage the probing cycle when the *Cycle Start* button is pressed. 
A *Remainder* to attach a Clip could be added here. The message needs to be confirmed with a *Cycle Start*.

![](/images/pa075.PNG)

This will display a message after the probing cycle with the Movable TT has finished and the tool has been retracted. A *Remainder* can be added here to remove the Movable TT and Clip. Be aware that not checking this box will directly move the Tool to the Fixed TT position without warning. The message needs to be confirmed with a *Cycle Start*.

![](/images/pa076.PNG)

This will display a WCS Reset message after the Movable TT has been probed. 
This allows to verify if Z0 has been set correctly. 
A time in Seconds can be set for the lenght of the message display before the cycle continues. 
A time value of 0 will require to confirm the message with a *Cycle Start*.

![](/images/pa077.PNG)

For Cycle 1, this will display the measured *Reference Height Offset*. 

For Cycle 2, this will display a WCS Reset message after the Fixed TT has been probed. 

This allows to verify if Z0/Offset has been set correctly. 
A time in Seconds can be set for the lenght of the message display before the cycle continues. 
A time value of 0 will require to confirm the message with a *Cycle Start*.

![](/images/pa078.PNG)

This is the text of the message that's being displayed before the test cycle of the Movable TT is being engaged if the *Test TT* option was selected om the main screen.
Customize this message do your requirements. A \n within the message will force a line break in the CNC12 message box.

![](/images/pa079.PNG)

This is the message that will be diplayed when the trigger signal of the Movable TT was succesfully recognized.
The lenght of the display time in seconds can be selected for this message. A time value of 0 will require the message being confirmed with a *Cycle Start*.

![](/images/pa080.PNG)

This setting defines the Z position the machine should retract to after probing the Movable TT and before moving over to the Fixed TT. 
This setting is only used in Cycle 1 that includes a probing cycle with the Movable TT.

The available options are:
* G28 (same as G30 P1)
* G30 (same as G30 P2)
* G30 P3
* G30 P4
* G53 with selectable Z Position
* Start Position 

The configuration screen does display all the Return Values as they are currently configured in CNC12 (Setup[F1] -> Part[F1] -> WCS Table[F9] -> Return[F1])

The G53 option does allow to select any Z machine coordinate position but be aware that MCS Z0 is usually on the top going down in negative direction so you would need to enter a negative position value.

The option *Start Position* will mark the current position at which the measurement of the Movable TT was kicked off.
If there's nothing in the way going from the Movable TT probing position to the Fixed TT location, this is a very convinient option to reduce Z travel moving down to the Fixed TT. 

If a Z return value has been selected that is below the probing point, an error message will be displayed and the probing cycle will be aborted.

![](/images/pa081.PNG)

This setting defines the Z position the machine should retract to at the end of the probing cycle after probing the Fixed TT.
The vailable options are the same as above.

![](/images/pa082.PNG)

Specifies the X and Y position the tool should move to at the end of the probing cycle.

The available options are:
* G28 (same as G30 P1)
* G30 (same as G30 P2)
* G30 P3
* G30 P4
* G53 with selectable X and Y Position
* No Move

Note that *No Move* is also an option here. This option will just retract Z abobe the Fixed TT to the selected Z return position and can be used if a Dust Shoe needs to be installed before continuing.
  
![](/images/pa083.PNG)

This specifies the X and Y position the machine needs to move to to probe with the Fixed TT.

![](/images/pa084.PNG)
This probing cycle supports a Tool Touch Off device being installed as a Touch Probe. 

If both, the Movable and the Fixed TT are of the same typ, they can be connected to the same Acorn Input and configured as a Tool Touch Off.
TTs of type NO need to be connected in parallel while NC type TTs need to be connected in series.

In the case where one TT is of type NO and the other of Type NC, you can connect one TT to an Acorn Input configured as TT and the other to an Acorn Iput configured as TP

![](/images/pa085.PNG)

If you have configured the height of the Tool Touch Off in the CNC12 Wizard it will be stored in CNC12 System Parameter P71.

![](/images/pa087.PNG)

There is some unexpected behavior in the way the Wizard configures P71 as the value you enter in the Wizard is positive but when you look up the parameter in CNC12 it will be stored as a negative number.
If you change P71 in CNC12 to a positive number it will show up in the Wizard as a negative number. 
To work around this issue, the ProbeApp will always use the absolute value of P71 and will do the correct thing with it.

You can always use P71 even when your Movable TT is configured as Touch Probe. If the TT Height is not set in CNC12 use the alternative option and enter the Height Offset here.

![](/images/pa086.PNG)

The probing moves can be executen in a fast probing move first followed by a slow probing move or a slow probing move only.

With this selection you define what your preferred, default probing behavior is.
This setting impacts the default setting of the check box *Fast Probing First* on the Cycle Main screen.

If you select *First Fast then Slow* here, the check box on the Cycle Main screen will always be selected by default and vice versa.



[Back](ToolSetter.md)

