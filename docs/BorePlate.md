# Tripple Corner Plate with Bore

![](/images/pa015.PNG)

## Description
Find the exact corner position of a stock and measure Tool Height Offsets. This probing cycle is available for all versions of CNC12.

Probing cycles can be made with a tool or with a Touch Probe. 
This probing cycle is designed to use the bore of the plate to find the corner position. 
If probing on the outside edges of the plate is preferred, use the [Standrad Tripple Corner Plate](CornerPlate.md) Probing Cycle. 


## Implementation Details
This probing cycle supports the plate being placed on any corner of the stock. Make sure the *Select Stock Corner* selector matches the actual plate position.
Combined or single Bore and Tool Height Offset measurements can be selected.

For Bore only or combined Bore and Z-Axis measurments place the tool/probe at the approximate center inside the bore at Z probing height. 
If tool flute alignment is necessary for accurate contact, be aware that the first probing move will be in the X negative direction.
Also make sure that the check box *Delay between XY Measurement* is checked to allow time to reposition the flutes for the Y measurement.

For Z-Axis Tool Height Offset only measurements, place the Tool/Probe above the Z-Axis measurement position.

At the end of the probing cycle, the *Set WCS* message will be displayed:

![](/images/pa042.PNG)

Note that this message can be surpressed if a direct WCS reset is desired withou a prior *Set WCS* message display.
See [Configuration Options](configuration.md) for details.

See all the configuration parameters below on details how to customize the probing cycle. 


## Congiguration Parameters
Before this probing cycle can be used, the plate parameter need to be entered.
After the plate has been setup, the check box *Show Corner Plate Configuration* can be unchecked to unclutter the screen.

![](/images/pa041.PNG)

Press the button *Parameter Help* for a graphic that explains the parameters a), b), c) and x), y), z).

![](/images/pa016.PNG)

Additional options that are available are explained below.


### Delay between XY Measurement
If measurements are taken with a cutter, it might be necessary to adjust the position of the tool flute to get an accurate measurement in each direction.
This option allows to add a pause or delay between the X and Y measurements in the bore to give time to allow for this adjustment.

The default value for this option is 0 which indicates that the probing cycle will stop with a message which needs to be confirmed with a *Cycle Start* before the probing cycles continues.
A value larger than 0 specifies the number of seconds the *Tool Adjustment Message* is being displayed before the probing cycle continues.

Note that the first probing move is always along the X-Axis probing the X- side first so the Tool Flute needs to be positione accordingly before the probing cycle is started.


### Move to Corner at the End of Cycle
This option only makes a difference when a combined probing of Bore and Z-Height is being done or if the center of the Bore has an offset to the stock corner (unusual).
In a combined cycle, the bore is always probed first followed by the Z touch off.

If this option is checked, the tool/probe will move to the Z-Clearance Height above the calculated corner point of the stock at the end of the cycle.

If this option is unchecked, the tool/probe will be at Z-Clearance Height above the Bore center or if a Z-Axis measurement was selected, above the Z-Axis measurment point, at the end of the probing cycle.


### Fast Probing Speed First
If this option is selected, all probing moves will occure at fast probing feedrate first followed by a final probing move at slow probing feedrate.

If this option is unchecked, only one probing move in slow probing feedrate will be done. 
While using a tool for the probing cycle, it is recommended to use slow probing feedrate only.


### Show Clamp Warning Message*
If this option is selected, a warning message will be diplayed before the probing cycle starts. 
The message needs to be confirmed with a *Cycle Start* before the probing cycle continues.

![](/images/pa043.PNG)

The *Clamp Warning Message* can be customized. See [Configuration Options](configuration.md) for details.


## Probing with Tool or Touch Probe

![](/images/pa044.PNG)

Probing cycles can be made with a tool or a Touch Probe.
Measuring the center of a bore has the benefit that the diameter of the tool is not needed to calculate the center position. 
If you are just measuring the center of the bore, a *Tool #* is not really needed and it doesn't matter which tool # you enter.
Also if all of the Tool Height Offsets in the CNC12 Tool Offset Library are set to 0, it doesn't matter which *Tool #* is selected for a Z-Axis measurement.

In the case where the CNC12 Tool Offset Library does have different Tool Height Offsets, it is important that the *Tool #* does match the tool # used in the job file.
By default the *Tool #* is set to 0 which means that the currently active tool in CNC12 is being used.
Make sure that CNC12 does have the correct tool # active when you make this selection:

![](/images/pa045.PNG)


## Start the Probing Cycle
After making all the selections necessary on the screen, pressing the *START* button will start the parameter validation.
A message will be displayed if any of the parameters are missing or invalid

![](/images/pa023.PNG)

If no validation issues have been found, the ProbeApp will write all the necessary probing commands to
```
c:\cncm\ncfiles\probing_cycle.cnc
```
The ProbeApp will close giving control back to CNC12

![](/images/pa024.PNG)

At this point the Probing Cycle can be started by pressing *Cycle Start* or aborted with *Cycle Cancle*.

## Closing ProbeApp without a Probing Cycle selections
The Probing Cycle screen can be closed by either pressing the *X* on the top right corner of the window or pressing the *Green Back Arrow* on the top left.
This will open the ProbeApp main window where the *Exit* button can be pressed.

Control will go back to CNC12 where this message will be displayed:

![](/images/pa024.PNG)

Pressing *Cycle Cancle* will terminate the cycle immediately. Pressing *Cycle Start* will bring up this message:

![](/images/pa025.PNG)

Pressing *Cycle Start* at this point will also terminate the cycle.



[Back](index.md)

