# Overview

![](/images/pa001.PNG)

The ProbeApp is a collection of probing cycles that integrate with the Centroid CNC12 control software.
If you have tried to use the probing cycle files that come with CNC12 outside of CNC12, you might have experienced that many don't work as they don't use fully qualified file names.
Also the Centroid probing cycles have hardcoded axis assignments and expect Axis 1=X, 2=Y and 3=Z.  

The ProbeApp is using it's own enhanced probing cycles that are partially based on the original Centroid probing cycles with some additional, new cycles.
The enhancements of the ProbeApp probing cycles include fully qialified file names for all cycles, Axis Number indipendence and some Probe Retry features 
(e.g if the given clearance height of a boss measurement is to low, the probing cycle will walk up the Z-Axis until the probe can traverse to the other side or a limit is tripped).


**Please report issues as such in GitHub. No Warranties are given. Use at your own risk!**

# Implementation Details
The ProbeApp is a Windows Application that runs outside of CNC12 and supports Touch Screen Input. 
By default the ProbeApp uses M58 to integrate with CNC12, that means a customized mfunc58.mac will be copied to the c:\cncm folder and the M58 Button on the VCP 2.0 will be replaced with a Probing Icon.
When you touch the probing icon on the VCP, the modified mfunc58.mac file will start up the ProbeApp and let you make any selections.
When you press Start in the ProbeApp, it will generate all the probing moves based on your selection and will give the control back to CNC12 to execute them.


![](/images/pa002.PNG)


You can also use one of the Macro buttons on the MPG to start up the ProbeApp. If M58 is already taken on your system, you can move it to any other AUX button you have available.


* NEW in v2 [Safe Retracts](safeRetracts.md)
* NEW in v2 [Smoothing Profile](smoothingProfile.md)
* [Add Command to Begin/End of Job](addCommand.md)
* [Add Debug Information](addDebug.md)
* [Check Tool Offset](checkToolOffset.md)
* [Enable Clamp (*M10/M11*)](enableClamp.md)
* [Comment Line Formatting](commentFormatting.md)
* [Dwell after Spindle Start](enableDwell.md)
* [Enforce Numeric Program Name](forceNumeric.md)
* [XY-Position at End of Job](xyPosition.md)
* [Z-Position at End of Job](zPosition.md)
* [Rotary Table Axis](rotaryAxis.md)
* [Write CNC12 Info Variables](CNC12.md)

![](/images/pp001.PNG)

# Additional Post Processor Logic
Additional logic has been added to the Post Processor to support

* NEW in v2 [Using Inverse Time Feedrate for Rotary Axis](inverseTime.md)
* [Check for conflicting Tool Numbers (same Number but different Tools)](checkDuplicateTools.md)
* [Support for Manual NC Commands](manualNC.md)

