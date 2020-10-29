# Tool Offset Manager: Fix Z-Height Method

## Description
This Method is for machines that have fixed Tool Holders and a constant distance between the Z-Home position and the machine table.
The Reference Height will be set at the top of the Z-Axis at the Z-Home position and all Tool Height Offsets will be measured from the Z-Home Position to the point where the tool touches the Tool Touch Off Reference Surface. 

The Reference Surface can be anywhere but it needs to be a position that never changes its Z-Height position and it needs to be the same position each time a new tool is being measured for tool height offset. The big benefit of tthis method is that it doesn't need a Reference Tool as the Reference Height is fixed at the Z-Home position.

It is important for this method to work accurately that the Z-Home position has an acceptable, repeatable position accuracy.
If you are unsure how accurate the Z-Home positioning of the machine is, the Guided Setup does provide a test cycle that resets the Z-Home position multiple times and measures the distance between the Z-Home position and the Reference Surface and will display the maximum deviation between all measuring cycles.

![](/images/pa107.png)

The number of cycles that can be selected is between 3 and 10.
If the accuracy of the Z-Home position is not good enough, the **Fix Z-Height Method**  can't be used and needs to be switched to the **Reference Tool Method**

## Configuration Settings

The configuration page for the **Fix Z-Height Method** does offer the following settings:

![](/images/pa108.png)

### CNC12 Configuration Settings

![](/images/pa109.png)

The right side of the Configuration page shows all the current CNC12 configuration settings related to Probing just for reference.
The coordinates of the Return Positions G28, G30, G30 P3 and G30 P4 must be configured in CNC12 directly and can't be configured in the ProbeApp (Setup[F1] -> Part[F1] -> WCS Table[F9] -> Return[F1])

### CNC12 Parameter 3

![](/images/pa110.png)

For  the **Fix Z-Height Method**, Parameter 3 will be set to 2 by default. There's no need to change this unless you want to enable one of the other options provided by Parameter 3:

![](/images/pa106.PNG)

Note that the ProbeApp can't change this parameter while CNC12 is running. You will get a message that you need to restart CNC12 for this Parameter to change. 
It's also important to know that the setting of Parameter 3 does not have any impact as long as you only use the ProbeApp-Tool Offset Manager to measure the Tool Height Offsets.
Only if you want to use the CNC12 built in functions to add Tool Height Offsets to the Tool Offset Library it is important that this Parameter is set correctly.

### Tool Touch Off Location

![](/images/pa111.png)

Check this box if you want the machine to move to a fixed point to touch off a tool.

Possible Options that can be configured as the fixed touch off location are:

* G28
* G30
* G30 P3
* G30 P4
* G53 X_ Y_

Note that the currently in CNC12 configured return positions for G28, G30, G30 P3 and G30 P4 are displayed on the right side of the configuration screen.
The default option is G30.
Changes to these return positions must be done directly in CNC12  (Setup[F1] -> Part[F1] -> WCS Table[F9] -> Return[F1]).

It is also possible to configure the X and Y machine coordinates (MCS) with the G53 option directly in the ProbeApp.

### Z Retract Position after Tool Touch Off

![](/images/pa112.png)

Configures the preferred retract position of the Z-Axis after the Tool Touch Off is completed.

Possible options are:

* G28
* G30
* G30 P3
* G30 P4
* G53 Z_
* Start Pos

See comments above about these options. An additional option here is to retact the Z-Axis to the Start Position which is the position at which the Tool Touch Off Cycle was started.
This can save time if you don't want to retract Z all the way up after a Tool Touch Off but be careful when you combine this option with the option to move the X and Y axes to a certain position after the Tool Touch Off as you need to make sure you always have enough Z Clearance.

The default option is G28.

### X and Y Return Positions after Tool Touch Off

![](/images/pa113.png)

Configures the preferred return positions of the X and Y-Axis after the Tool Touch Off is completed.

Possible options are:

* G28
* G30
* G30 P3
* G30 P4
* G53 X_ Y_
* No Move
* G0 X_ Y_

In addition to the G28, G30 and G53 return positions is the option to return to the current WCS Zero point with the G0 command.

The default option is **No Move**.

### Probing Method Selection

![](/images/pa114.png)

This selection depends on the type of Tool Touch Off device being used. If the Tool Touch Off device has a spring loaded touch off plate, it is recommended to do a more agressive fast probing move first followed by a slow accurate probing move. 

If the Tool Touch Off device is a fixed metal plate it is recommended to skip the first fast probing move and run a slow probing move only to protect the tool.

### Probing Speed Overwrite

![](/images/pa115.png)

These settings allow to overwrite the default CNC12 slow and fast probing speeds just for the Tool Touch Off cycles.
If these values are left at 0, the default CNC12 probing speeds are being used.
The currently configured CNC12 probing speeds are listed on the right side of the configuration screen.

### Return Option

![](/images/pa116.png)

The Tool Offset Manager main screen has a Return check box that will tell the ProbeApp to re-open after data has been saved or tools have been measured.
This check box here in the configuration screen configures if the Return check box on the Tool Offset Manager main screen should be checked by default or not.

### Button Functions

![](/images/pa117.png)

* Cancel: Will abort the setup process 
* Guided Setup: Use to re-run the Guided Setup if machine information has changed
* Save Configuration: Saves the configuration settings and opens the Tool Offset Manager

## Instructions Page

After the configuration settings are saved, an Instruction screen like this will be displayed:

![](/images/pa118.png)

Read this information carefully as the instructions are dynamically created based on the configuration settings.
Pressing the **OK** button willl close the Instructions page but the page can be re-openen anytime again with the **Instructions** button on the Tool Offset Manager main screen.

## Tool Offset Library Manager

A typical Tool Offset Library Manager main screen for the **Fix Z-Height Method** looks like this:

![](/images/pa119.png)



[Back](index.md)

