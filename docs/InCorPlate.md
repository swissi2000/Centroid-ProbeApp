# Tripple Corner Plate with Inside Corners

![](/images/pa019.PNG)

## Description
Find the exact corner position of a stock and measure Tool Height Offsets. This probing cycle is available for all versions of CNC12.
This probing cycle is extremly versatile as the plate can be placed on any corner of the stock and any inside corner or edge of the plate can be used for measurements.

Probing cycles can be made with a tool or with a Touch Probe. 

If probing on the outside edges of the plate is preferred, use the [Standard Tripple Corner Plate](CornerPlate.md) Probing Cycle. 

## Implementation Details
This probing cycle supports the plate being placed on any corner of the stock. Make sure the *Select Stock Corner* selection matches the actual plate position.
Any inside corner, a single side or touch-off surface can be used for measurements.

Having the option to use the front outside corner of the square hole to find the stock corner gives the *safty* probing option.
In the case the probe is malfunctioning for some reasons, the probing move would just push the plate away from the stock without breaking the tool or cause other damage.

Place the Tool/Probe in the approximate center inside the square hole.
The Z-Height has to be set at the level the measurements needs to be taken on the surface.

If preferred, the Tool/Probe can be placed closer to the inside corner or edge to shorten the probing cycle but be aware that the Z-Axis measurement is always last and at the position the Tool?probe was placed to start the probing cycle.
 
If tool flute alignment is necessary for accurate contact, make sure that the check box *Delay between XY Measurement* is checked to allow time to reposition the flutes for the second measurement.

If only a Z-Axis measurement is selected, the measurement will take place at the current position of the Tool/Probe. 

At the end of the probing cycle, the *Set WCS* message will be displayed:

![](/images/pa042.PNG)

Note that this message can be surpressed if a direct WCS reset is desired withou a prior *Set WCS* message display.
See [Configuration Options](configuration.md) for details.

See all the configuration parameters below on details how to customize the probing cycle. 


## Configuration Parameters
Before this probing cycle can be used, the plate parameters need to be entered.
After the plate has been setup, the check box *Show Corner Plate Configuration* can be unchecked to unclutter the screen.

![](/images/pa051.PNG)


### Inside Square Width
Enter the exact dimension of the inside square hole. 
This dimension is needed to calculate the position of the stock corner or edge based on the selected plate inside corner/edge being measured.
 
### Touch Plate Height (Z Offset)
This is the distance (offset) from the stock surface on which the plate is being placed to the Z-Axis touch-off surface of the plate. 

This value is needed to calculate the actual Z position of the stock surface.

### Z-Clearance Amount (incr. from Touch Plate)
This is the distance the Tool/Probe will move up on the Z-Axis at the end of the probing cycle and before traversing over to the calculated stock corner position if requested.

This amount MUST be larger than the depth of the Square Hole to make sure the Tool/Probe has fully cleared the plate height before traversing to avoid an unexpected probe trip that would terminate the probing cycle.

### Delay between XY Measurements
If measurements are taken with a cutter, it might be necessary to adjust the position of the tool flute to get an accurate measurement in each direction.
This option allows to add a pause or delay between the X and Y measurements to give time to make this adjustment.

The default value for this option is 0 which indicates that the probing cycle will stop with a message which needs to be confirmed with a *Cycle Start* before the probing cycles continues.
A value larger than 0 specifies the number of seconds the *Tool Adjustment Message* is being displayed before the probing cycle continues.


### Move to Corner at the End of Cycle
This option makes no difference when a single Z-Axis measurement is taken as the probing cycle will always end at the Z-Clearance Height where the Z measurement is taken.

If this option is unchecked, the probing cycle will end at the Z-Clearance Height where the last measurment was taken.

If this option is checked, the center of the Tool/Probe will move to the Z-Clearance Height above the calculated corner point of the stock at the end of the cycle.
If only a single side was selected, the center of the Tool/Probe will move to the Z-Clearance Height above the measured stock edge position.


### Fast Probing Speed First
If this option is selected, all probing moves will occure at fast probing feedrate first followed by a final probing move at slow probing feedrate.

If this option is unchecked, only one probing move in slow probing feedrate will be done. 
While using a tool for the probing cycle, it is recommended to use slow probing feedrate only.


### Show Clamp Warning Message
If this option is selected, a warning message will be diplayed before the probing cycle starts. 
The message needs to be confirmed with a *Cycle Start* before the probing cycle continues.

![](/images/pa043.PNG)

The *Clamp Warning Message* can be customized. See [Configuration Options](configuration.md) for details.


## Probing with Tool or Touch Probe

![](/images/pa061.PNG)


Probing can be done with a Tool or with a Touch Probe. There are 3 options that can be selected:

### Tool Diameter 
If you don't use the CNC12 Tool Libary and your G-Code is written/generated without Tool Diameter compensation, you can select this option. 

If only a Z-Measurment is selected without a side or corner, no diameter value needs to be entered. For corner and side measurements, the Tool-Diameter is needed to calculate the corner or side position.

### Probing with Tool #
Here it is IMPORTANT that the tool diameter of the selected tool is properly configured in the CNC12 Offset Library for X/Y measurements. 

As an example, if Tool # 6 has been choosen that has a diameter of 4mm, the CNC12 Offset Library MUST be configured like this:

![](/images/pa049.PNG)

For Z-Axis measurements, the Tool Diameter plays no role but the Tool Height Offset is important. 
If every tool is being touched off before it's been used, make sure the Height Offsets in the CNC12 Offset Library are all set to 0 like the example above.

By default the *Tool #* is set to 0 which means that the currently active tool in CNC12 is being used.
Make sure that CNC12 does have the correct tool # active when you make this selection and that the CNC12 Offset Libray has the correct Offsets for that tool:

![](/images/pa045.PNG)

In the case where the CNC12 Tool Offset Library does have different Tool Height Offsets, it is important that the *Tool #* does match the tool # used in the job file.

### Probing with Touch Probe
If *Probing with Touch Probe* is selected, the Touch Probe needs to be setup and configured properly in the CNC12 Wizard as the probing cycle will depend on the CNC12 probing parameters.


## Start the Probing Cycle
After making all the selections necessary on the screen, pressing the *START* button will start the parameter validation.
A message will be displayed if any of the parameters are missing or invalid

![](/images/pa050.PNG)

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

