# Slot Probing Cycle

![](/images/pa007.PNG)

## Description
Find the center of a Slot. This probing cycle is not available for CNC12 Free.

## Implementation Details
The probe needs to be placed in the approximate center inside the Slot.
The Z-Height has to be set at the level the measurements needs to be taken inside the Slot.

This probing cycle has 3 possible orientations:
* Probing Move along the X-Axis
* Probing Move along the Y-Axis
* Combined Probing Move along the X and Y-Axis to measure a Rectangular Hole

The probe will make the following moves and measurements:

* Move to the negative side of the selected axis for the first measurement
* Move to the positive side of the selected axis for the second measurement
* Move to the calculated center based on the two measurements
* For the Rectangular Hole probing cycle, the above moves will be repeated for the second axis

At the end of the probing cycle, the probe will be at the center of the Slot and the width of the Slot will be displayed:

![](/images/pa027.PNG)

For a Rectangular Hole Probing Cycle, the probe will be at the center of the hole and the X and Y-width will be displayed:

![](/images/pa028.PNG)

Note that this message can be surpressed if "*Set WCS after probing*" is checked and the configuration settings have been made to skip the measurement display.
See [Configuration Options](configuration.md) for details.

If "*Set WCS after probing*" was checked, the WCS reset message will be displayed:

![](/images/pa021.PNG)

Note that this message can also be surpressed if a direct WCS reset is desired withou a prior WCS Reset Info message display.
See [Configuration Options](configuration.md) for details.

## Probing Parameters
The only parameter for this probing cycle is the "*Set WCS after probing*" checkbox

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

