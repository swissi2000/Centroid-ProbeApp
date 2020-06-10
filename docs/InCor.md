# Inside Corner Probing Cycle

![](/images/pa009.PNG)

## Description
Find the inside corner location of two walls that do not need to be perpendicular.
This probing cycle is not available for CNC12 Free.

## Implementation Details
Four probing orientations are possible: X-/Y-, X+/Y-, X-/Y+ and X+/Y+ Probing Directions
The probe needs to be placed at the inside of the selected corner at about equal distances to both sides of the corner walls.
The Z-Height has to be set at the level the measurements needs to be taken inside the corner walls.

The probe will make the following moves and measurements:

* First probing move along the X-Axis to get the first X-Axis measurement
* Retract along the X-Axis to the starting point
* Second probing move along the Y-Axis to get the first Y-Axis measurement
* Retract along the Y-Axis to 2/3 the lenght of the start position
* Third probing move along the X-Axis to get the second X-Axis measurement
* Retract along the X-Axis to 2/3 the lenght of the start position
* Fourth probing move along the Y-Axis to get the second Y-Axis measurement
* Based on the two measurements of each corner wall, the angle of both walls can be calculated
* Based on the angles of the two walls, the intersection point of the two vectors can be calculated which is the corner position
* Move up to the Z-Clearance Height
* Traverse above the calculated corner position

At the end of the probing cycle, the probe will be at Z-Clearance Height above the Inside Corner position and the coordinates of the corner will be played:

![](/images/pa030.PNG)

Note that this message can be surpressed if "*Set WCS after probing*" is checked and the configuration settings have been made to skip the measurement display.
See [Configuration Options](configuration.md) for details.

If "*Set WCS after probing*" was checked, the WCS reset message will be displayed:

![](/images/pa021.PNG)

Note that this message can also be surpressed if a direct WCS reset is desired withou a prior WCS Reset Info message display.
See [Configuration Options](configuration.md) for details.

## Probing Parameters
### Z-Clearance Amount
The *Z-Clearance Amount* specifies the distance the probe needs to move up on Z from the start point to traverse above the corner position at the end of the probing cycle. 
Be generous with this amount and rather error on the plus side rather than being too low. 

If the Z-Clearance Height is too low and an unexpected probe trip occurs, the probing cycle will be terminated with an error message.

### Set WCS after probing

![](/images/pa022.PNG)

If the WCS box is checked, at least one of the Axis needs to be checked too. 
By default, WCS 0 for the axis will be set at the center of the bore hole for the currently *Active* WCS.

Both axis allow to set WCS 0 at an *Offset* from the center if needed. For an offset in the negative axis direction, a negative value needs to be entered.

Note that the available WCS numbers depend on the version of CNC12:

* CNC12 Free: Only WCS #1 (G54) is available
* CNC12 Pro: WCS #1 - #6 (G54 - G59) are available
* CNC12 Digi: WCS #1 - #18 (E1 - E18 where E1 - E6 is the same as G54 - G59)

Any of the available WCS # can be selected to be reset by the probing cycle.

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

