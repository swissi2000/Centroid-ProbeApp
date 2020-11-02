# Method 1: Double Probe Method

## Description
This Probing Cycle is designed for machines that do not have a spindle with fixed tool holdings and every tool needs to me measured for Tool Height Offset after each M6 Tool Change.
For this Probing Cycle to work properly, it is required that the Tool Height Offsets for all Tools are set to ) in the CNC12 Tool Offset library.
It is recommended that you run the **ProbeApp-Tool Offsetter** bevore using this cycle as it will make sure all Tool Height Offsets are reset to 0 in the CNC12 Tool Offset Library.

This method works with the following combinations of Probing Device:

* A movable and a fixed Tool Touch Off device (Preferred Option)
* A single Tool Touch Off device that can be used as a movable and a fixed TT
* A Touch Probe and a fixed Tool Touch Off device

### Movable and a fixed Tool Touch Off device (Preferred Option)
This option uses two Tool Touch Off devices (TT). The movable TT is being placed on top of the workpiece to find the exact position of the top surface.
The second TT is installed in a fixed location and is used first to measure the Height Offset between the top of the stock and the top of the fixed TT to set the Reference Height Offset. After the Reference Height Offset is known, all subsequent tools can be touched off automatically on the fixed TT only to calculate the tools height offset compared to the top of the stock.

### Single Tool Touch Off device that can be used as a movable and a fixed TT
This cycle works the same way as the option with a separate movable and fixed TT with the exception that only one TT is present that plays a double role as a movable and a fixed TT.
The TT is first used as a movable TT on top of the workpiece to find the exact positionn of the top surface.
The movable TT is then placed at a fixed location afterwards and all sugsequent probing is done with the TT at the fixed location.

### Touch Probe and a fixed Tool Touch Off device
With this option, the Touch Probe will be used to find the top surface of the workpiece and then move to the fixed TT to find the top of the fixed TT and set the Reference Height Offset. After the Reference Height Offset has been set with the Touch Probe, the probing cycle will ask to remove the Touch Probe and insert the requested tool.
The **ProbeApp-Tool Setter** will open again to have the active tool touched off with the fixed TT before the job continues.

This option will be activated by selecting the checkbox **Use TP to set Ref. Height**:

![](/images/pa130.png)

The default message texts set in the configuration settings are targeted for the preferred option of using a movable and a fixed TT.
If the Touch Probe option is being used, make sure you customize the message text accordingly.

## On the Fly configuration Options on the Tool Setter Main Screen
Four options can be selected on the bottom right to customize the probing cycle:

* *Test TT//TP (Movable TT only):* This will add a TT or TP Trigger Check before the probing cycle is started to assure that the TT/TP is functioning properly.
* *Movable TT Fast Probing first:* This check box will always default to the selected configuration setting and allows to temporarely enable or disable the fast probing move for the movable TT or TP.
* *Fixed TT Fast Probing first:* This check box will always default to the selected configuration setting and allows to temporarely enable or disable the fast probing move for the fixed TT.
* *Use WCS#:* This allows to select the WCS# that's being reset by this probing cycle. Default is always the currently active WCS#. The machine will remain in the currently active WCS and only switches quickly to the selected WCS for the Reset. 

## Probing Cycle Types
This method has three different Probing Cycle Types. Read the description for each type carefully and make sure you select the appropriate cycle for each Tool Change at the right time.

![](/images/pa065.PNG)

### Cycle 1: Set Top of Stock and set Reference Height Offset

![](/images/pa070.PNG)

This cycle is being used on the first Tool Change after a new workpiece has been mounted that has an unknown top surface position. *Cycle 1* involves two different Probing Moves.

The first Probing Move is using the movable TT (or the TP if selected) on top of the workpiece and will measure the exact position of the top surface.

The second Probing Move is using the Fixed TT and is needed to calculate the *Reference Height Offset* which is the distance between the top of the Fixed TT and the top of the workpiece.
The Reference Height Offset is used to correctly set the Tool Height Offset for subsequent tool changes on the same workpiece by using the Fixed TT only.

The Cycle 1 is being used with a movable TT, the cycle will also set the correct Tool Height Offset of the first tool on top of the stock surface.
This is a little different if a Touch Probe is being used as the Touch Probe will just measure and set the Reference Height Offset in Cycle 1.
If Cycle 1 finishes when using a Touch Probe, a message will ask to remove the Touch Probe and insert the requested tool.
After this has been completed and the Cycle Start button is pressed, the **ProbeApp-Tool Setter** will open again and Cycle 2, as described below, needs to be executed to set the correct Tool Height Offset for the first tool.

### Cycle 2: Set Z0 with Fixed TT - Ref Height Offset 

![](/images/pa071.PNG)

After a *Reference Height Offset* has been established with *Cycle 1*, all subsequent Tool Changes on the same workpiece can be done with the Fixed TT only.

Use this option for all subsequent Tool Changes on the same workpiece after the *Reference Tool Height* has been set with *Cycle 1*.
*Note* that if Cycle 1 was done with a Touch Probe, Cycle 2 MUST be executed also with the first tool in order to have the Tool Height Offset set properly.

The currently active *Reference Height Offset* is displayed in the box and can be manually adjusted if necessary.

Three options can be selected on the bottom right to customize the probing cycle (see above for details).

### Cycle 3: Continue without Tool Probing 

![](/images/pa072.PNG)

If the Tool Setter screen is being launched by a M6 Tool Change command but no Tool Touch Off is needed, the *Cycle 3* button allows to continue the job without a Tool Touch Off.

## Configuration for Dual Probe Method

![](/images/pa066.PNG)

The Configuration screen allows for a very flexible customization of the probing cycle. 
The right side of the Configuration screen shows the currently configured CNC12 parameters related to Probing and Return Positions.
This allows to verify the correctness of the CNC12 settings while customizing the ProbeApp Tool Setter configuration.
These options must be changed directly in CNC12 and the ProbeApp will always use the currently active CNC12 configuration settings. 

Check out the details below for each of the available Configuration Settings.

![](/images/pa074.PNG)

If this option is checked, CNC12 will display a Message before the probing move with the Movable TT or the TP is executed. 
The message can be customized in the *Message* box (adding \n does create a new line in the message box). 

Be aware that not checking this box does directly engage the probing cycle when the *Cycle Start* button is pressed. 
A *Remainder* to attach a Clip could be added here. Also change this to a more appropriate message text if a Touch Probe is being used.
The message needs to be confirmed with a *Cycle Start*.

![](/images/pa075.PNG)

This will display a message after the probing cycle with the Movable TT or the TP has finished and the tool or TP has been retracted. A *Remainder* can be added here to remove the Movable TT and Clip. Be aware that not checking this box will directly move the Tool to the Fixed TT position without warning. The message needs to be confirmed with a *Cycle Start*.

![](/images/pa099.PNG)

This option was added in v1.2 and will display a message after the probing cycle with the fixed TT has finished and the tool has been retracted. 
A *Remainder* can be added here to attach the Dust Boot before the job continues. The message needs to be confirmed with a *Cycle Start*.
Note that this message will be skipped if a Touch Probe is being used as it is necessary to re-open the **ProbeApp-Tool Setter** to measure the current tool and this message would not make any sense.

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
Customize this message to your requirements. A \n within the message will force a line break in the CNC12 message box.

![](/images/pa079.PNG)

This is the message that will be diplayed when the trigger signal of the Movable TT was succesfully recognized.
The lenght of the display time in seconds can be selected for this message. A time value of 0 will require the message being confirmed with a *Cycle Start*.

![](/images/pa080.PNG)

This setting defines the Z position the machine should retract to after probing the Movable TT (or after the TP has touched off the stock surface) and before moving over to the Fixed TT. 
This setting is only used in Cycle 1 that includes a probing cycle with the Movable TT or the TP.

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

Note that *No Move* is also an option here. This option will just retract Z above the Fixed TT to the selected Z return position and can be used if a Dust Boot needs to be installed before continuing.
  
![](/images/pa083.PNG)

This specifies the X and Y position the machine needs to move to to probe with the Fixed TT.

![](/images/pa084.PNG)
This probing cycle supports a Tool Touch Off device being installed as a Touch Probe. 

If both, the Movable and the Fixed TT are of the same typ, they can be connected to the same Acorn Input and configured as a Tool Touch Off.
TTs of type NO need to be connected in parallel while NC type TTs need to be connected in series.

In the case where one TT is of type NO and the other of Type NC, you can connect one TT to an Acorn Input configured as TT and the other to an Acorn Iput configured as TP

If a Touch Probe is being used instead of a movable TT, make sure to select the checkbox **Use TP to set Ref. Height**.

![](/images/pa085.PNG)

If you have configured the height of the Tool Touch Off in the CNC12 Wizard it will be stored in CNC12 System Parameter P71.

![](/images/pa087.PNG)

There is some unexpected behavior in the way the Wizard configures P71 as the value you enter in the Wizard is positive but when you look up the parameter in CNC12 it will be stored as a negative number.
If you change P71 in CNC12 to a positive number it will show up in the Wizard as a negative number. 
To work around this issue, the ProbeApp will always use the absolute value of P71 and will do the correct calculation with it.

You can always use P71 even when your Movable TT is configured as Touch Probe. If the TT Height is not set in CNC12 use the alternative option and enter the Height Offset here.

Note if a Touch Probe is being used instead of a movable TT, the Height Offset will be forced to be 0.

![](/images/pa086.PNG)

The probing moves can be executed in a fast probing move first followed by a slow probing move or a slow probing move only.

With this selection you define what your preferred, default probing behavior is. The defaults for the movable and fixed TT can be set seperately.
This setting impacts the default settings of the check boxes *Movable/Fixed TT Fast Probing first* on the Tool Setter Cycle Main screen.

If you select *First Fast then Slow* here, the check boxes on the Tool Setter Cycle Main screen will always be selected by default and vice versa.

![](/images/pa100.PNG)

This allows to configure a dedicated slow and fast probing feedrate seperately for the movable and fixed TT.
These speed settings are only for the Tool Setter cycle and do not impact the probing speed settings in CNC12.

If these probing feedrate values are left at 0, the default CNC12 probing feedrate values configured in the Probe Section of the CNC12 Wizard are being used.

Note that the units (mm/min or inch/min) are displayed depending on the default units of the machine.



[Back](ToolSetter.md)

