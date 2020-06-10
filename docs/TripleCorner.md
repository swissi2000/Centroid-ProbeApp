# 3 Axis Corner and Top Probing Cycle

![](/images/pa014.PNG)

## Description
Find the corner and top of a Rectangular Stock to set WCS 0 for X, Y and Z-Axis with one probing cycle. This probing cycle is available for all versions of CNC12.

## Implementation Details
Any of the four corners of the rectangular stock can be selected.
The probe needs to be placed above the Z-Surface of the corner at the approximate X and Y distance to the edge of the stock specified in the *Approx. X/Y Distance from Edge*.
The *Z Drop Down from Surface* parameter specifies the distance the probe should drop down from the actual stock Z-Surface to make the edge probings.
Note that the Z-Surface probing will be done first at the starting position of the probing cycle so the cycle will know the exact position of the Z-Surface when it will drop down on Z for the X and Y surface measurements.
The *Z Drop Down from Surface* value needs to be larger than half of the probe diameter.

The probe will make the following moves and measurements:

* Makes the Z-Surface masurement first by dropping straight down from the Start position to the Z-Surface
* Move Z back up to the start position
* Move along the X-Axis first past the edge of the stock (distance = [Approx. X Distance from Edge] + [Probe Diameter/2] + [Probing Recovery Distance]) 
* Drop down Z-Axis to *Z Drop Down from Surface* distance past the measured Z-Surface position
* If the *Approx. X Distance from Edge* given was too short and the probe trips while trying to get down to the Z-Height, the probe will advance in increments until it can drop down to the Z-Height or a limit is triggered 
* Make the second measurement to get the X-Surface position
* Move Z-Axis back up to start position
* Traverse accross the corner to the position for the Y-Surface measurement
* Drop down Z-Axis to *Z Drop Down from Surface* distance past the measured Z-Surface position
* If the *Approx. Y Distance from Edge* given was too short and the probe trips while trying to get down to the Z-Height, the probe will advance in increments until it can drop down to the Z-Height or a limit is triggered 
* Make the third measurement to get the Y-Surface position
* Move Z-Axis back up to start position
* Traverse to the measured corner point and move the Z-Axis to *Probing Recovery Distance (System Parameter #130* above the Z-Surface

At the end of the probing cycle, the probe will be at the corner position of the Rectangular Stock at the *Probing Recovery Distance (System Parameter #13)* above the Z-Surface and the *Set WCS* message will be displayed:

![](/images/pa039.PNG)

Note that this message can be surpressed if a direct WCS reset is desired withou a prior *Set WCS* message display.
See [Configuration Options](configuration.md) for details.

Be aware that the DRO for the Z-Axis will not show 0 after the WCS reset as the probe is above the Z-Surface by the *Probing Recovery Distance (System Parameter #13)* when the WCS is being reset.

## Probing Parameters
### Approx. X/Y Distance from Edge
The *Approx. X/Y Distance from Edge* specifies the distance from the center of the probe at the start positionm to the X or Y edge of the stock.
Under normal conditions these parameters will be set to equal values for a distance you have a good eye for and only change them when a special situation has some features that don't allow a Z-Surface probing at the standard location.
 
Be generous with this distance and rather error on the plus side than being too short. 

If this distance is too short, the probe will be tripped while trying to move down on Z to get to the X and Y-Surface probing height. 
When this happens, the retry cycle will be activated that will advance the probe in increments until it's clear to drop down on Z.
Should the retry cycle hit any limits (e.g Probing Search Distance or travel limit) the probing cycle will be aborted with an error message.

### Z Drop Down from Surface
The *Z Drop Down from Surface* specifies the distance the probe needs to drop down past the measured Z-Surface to take the X and Y-Surface measurements. 

Note that the *Z Drop Down from Surface* distance *MUST* be larger than the radius of the probe tip.

### Set WCS after probing

![](/images/pa040.PNG)

If the WCS box is checked, at least one of the Axis needs to be checked too. 
By default, WCS 0 for the axis will be set at the corner position of the Rectangular Stock for the currently *Active* WCS.

All 3 axis allow to set WCS 0 at an *Offset* if needed. For an offset in the negative axis direction, a negative value needs to be entered.

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

