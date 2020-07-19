# ProbeApp Configuration 

![](/images/pa052.PNG)

## Default Window Position
All windows of the ProbeApp have a fixed Upper Left corner.
If a different position of the ProbeApp screens is preferred, move the top left corner of the *Configuration* Window to the preferred position and press the **Get current Position from this Window** button.

## Confirm Button Text
The default *Button Text* on each ProbeApp screen to kick off the probing cycle is START.

![](/images/pa053.PNG)

Change this text if desired.

## Probing Cycle File Name and Location
The ProbeApp dynamically creates all the probing moves required based on the users selection in tha app.
When the START button is pressed, the probing moves are saved into a file for CNC12 to execute.

By default the name and location of this file is
```
c:\cncm\ncfiles\probing_cycles.cnc
```
Change the file name and location if preferred.

**NOTE** that you need to adjust the mfunc58.mac (or any other macros you used to call the probing_cycle.cnc file) manually.

## Enable Return Feature
If this feature is enabled, a Check-Box named **Return** will appear on each Probing Cycle Screen next to the **START** Button:

![](/images/pa098.PNG)

If **Return** is checked when the **START** Button is clicked, the ProbeApp will add commands to the probing cycle file that will re-open the ProbeApp after the probing cycle has finished.

This feature is useful when the ProbeApp is being started from a running job, e.g. a M6 tool change command (*mfunc6.mac*).
This will allow to execute several different probing cycles while the job is still in the M6 tool change hold to set WCS and Tool Height Offsets and then let the job continue after all probing has been completed.

## Show Probed Measurements
Note that this setting only impacts a probing cycle that will set WCS.
The default behavior is to display the probed measurements first and on a second screen present the WCS coordinates that will be set.

Unchecking this box will skip the first display of the probed measurements.

The text box *Displayed Confirmation Text* allows to customize the text that's been presented on the measurement display.

![](/images/pa020.PNG)


## Show Set WCS Confirmation Message
The default behavior is to display the *Set WCS* display first before WCS is being set.
This allows to cancel the probing cycle before WCS is set if needed.

Unchecking this box will skip the *Set WCS* display and will directly set WCS without needed user intervention.

If it is preferred that a probing cycle automatically sets WCS without needed user interventions, uncheck this box together with the *Show Probed Measurements* check box. 

The text box *Displayed Confirmation Text* allows to customize the text that's been presented on the Set WCS display.

![](/images/pa021.PNG)

### Clamp Warning Message
This text box allows to cuctomize the *Clamp Warning Message*.

![](/images/pa043.PNG)




[Back](index.md)

