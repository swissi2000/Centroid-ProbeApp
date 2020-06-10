# Web Probing Cycle

![](/images/pa003.PNG)

## Description
Find the center of a Web. This probing cycle is not available for CNC12 Free.

## Implementation Details
Four possible starting positions are available for the Web Probing Cycle: Left, Right, Front or Back.
The probe needs to be placed at the selected position.
The Z-Height has to be set at the level the measurements needs to be taken outside the Web.

The probe will make the following moves and measurements:

* Takes the first measurement at the selected position where the probe has been placed
* Move Z up the distance given by *Z-Clearance Amount*
* Move over to the opposite side of the starting point
* If the *Z-Clearance Amount* was too short and an unexpected probe trip occurs while trying to traverse, the Z-Axis will move up in increments until the probe can traverse or a llimit is been triggered.
* At the opposite side, the Z-Axis will drop down to the level of the starting point from the first measurement
* If the *Web Width* given was too short and the probe trips while trying to get down to the Z-Height, the probe will advance in increments until it can drop down to the Z-Height or a limit is triggered 
* The second measurement will be taken at the opposite side of the first measurement
* Move up to the Z-Clearance Height
* Traverse to the center position of the two measurements

At the end of the probing cycle, the probe will be at the center of the Web and the Web-Width will be displayed:

![](/images/pa029.PNG)

Note that this message can be surpressed if "*Set WCS after probing*" is checked and the configuration settings have been made to skip the measurement display.
See [Configuration Options](configuration.md) for details.

If "*Set WCS after probing*" was checked, the WCS reset message will be displayed:

![](/images/pa021.PNG)

Note that this message can also be surpressed if a direct WCS reset is desired withou a prior WCS Reset Info message display.
See [Configuration Options](configuration.md) for details.

## Probing Parameters
### Web Width
The *Web Width* specifies the approximate width of the Web and is being used by the probing cycle to calculate the distance the probe needs to move to measure the opposite side of the boss.
Be generous with this amount and rather error on the plus side rather than being too small. 

If this amount is too small, the probe will be tripped while trying to move down on Z to get to the probing height. 
When this happens, the retry cycle will be activated that will advance the probe in increments until it's clear to drop down on Z.
Should the retry cycle hit any limits (e.g Probing Search Distance or travel limit) the probing cycle will be aborted with an error message.

### Z-Clearance Amount
The *Z-Clearance Amount* specifies the distance the probe needs to move up on Z from the start point to traverse the boss to get to the opposite side. 
Be generous with this amount and rather error on the plus side rather than being too low. 

If this amount is too small, the probe will be tripped while trying to traverse the boss. 
When this happens, the retry cycle will be activated that will advance the probe in increments until it has enough clearing height to traverse.
Should the retry cycle hit any limits (e.g Probing Search Distance or travel limit) the probing cycle will be aborted with an error message.

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

