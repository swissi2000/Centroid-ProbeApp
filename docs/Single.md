# Single Axis XYZ Probing Cycle

![](/images/pa011.PNG)

## Description
Find the position of a X, Y or Z-Surface. This probing cycle is not available for CNC12 Free.

## Implementation Details
The probe needs to be placed next to the X or Y-Surface or above the Z-Surface to be probed.
For X and Y-Surface measurements, the Z-Height has to be set at the level the measurements needs to be taken at the surface.
For Z-Surface measurements, the probe can be placed at any distance above the location where the Z measurment needs to be taken.

For X and Y-Surface measurements, the probe will be away from the probed surface by
```
(Probing Recovery Distance [System Parameter #13]) + (Probe DIameter /2)
```
For Z-Surface measurements, the probe will be above the probed surface by
```
(Probing Recovery Distance [System Parameter #13]) 
```

The position of the probed surface location will be displayed:

![](/images/pa032.PNG)

Note that this message can be surpressed if "*Set WCS after probing*" is checked and the configuration settings have been made to skip the measurement display.
See [Configuration Options](configuration.md) for details.

Also be aware that the DRO of the axis that's been reset will not show 0 as it does with most probing cycles as the probe will be at a retracted position by the time the WCS is being reset.

If "*Set WCS after probing*" was checked, the WCS reset message will be displayed:

![](/images/pa033.PNG)

Note that this message can also be surpressed if a direct WCS reset is desired withou a prior WCS Reset Info message display.
See [Configuration Options](configuration.md) for details.

## Probing Parameters
The only parameter for this probing cycle is the "*Set WCS after probing*" checkbox

![](/images/pa034.PNG)

With the Single Axis probing cycle, only the axis that has been probed can be reset. 
By default, WCS 0 for the axis will be set at the measured surface position of the currently *Active* WCS.

It is possible to set WCS 0 at an *Offset* from the measured surface. For an offset in the negative axis direction, a negative value needs to be entered.

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

