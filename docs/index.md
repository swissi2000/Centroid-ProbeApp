# Overview

![](/images/pa001.PNG)

The ProbeApp is a collection of probing cycles that integrate with the Centroid CNC12 control software.
If you have tried to use the probing cycle files that come with CNC12 outside of CNC12, you might have experienced that many don't work as they don't use fully qualified file names.
Also the Centroid probing cycles have hardcoded axis assignments and expect Axis 1=X, 2=Y and 3=Z.  

The ProbeApp is using it's own enhanced probing cycles that are partially based on the original Centroid probing cycles with some additional, new cycles.
The enhancements of the ProbeApp probing cycles include fully qialified file names for all cycles, Axis Number independence, improved error handling and some Probe Retry features 
(e.g if the given clearance height of a boss measurement is to low, the probing cycle will walk up the Z-Axis until the probe can traverse to the other side or a limit is tripped).


**Please report issues as such in GitHub. No Warranties are given. Use at your own risk!**

# Implementation Details
The ProbeApp is a Windows Application that runs outside of CNC12 and supports Touch Screen Input. 
By default the ProbeApp uses M58 to integrate with CNC12, that means a customized mfunc58.mac will be copied to the c:\cncm folder and the M58 Button on the VCP 2.0 will be replaced with a Probing Icon.

```javascript
;--------------------------------------------------------------------------------
; Filename: mfunc58.mac 
; M58 macro
; Description: Provides Probing Routines when used with program Probing_Setup.exe 
; Author: swissi
;---------------------------------------------------------------------------------
#33999 = #4006                            ; store active units
M130 "C:\cncm\probing\ProbeApp.exe"       ; Call the external probing cycle setup program
#30000 = 0				      			  ; No probing Cycle saved when this stays 0
N100
G4 P1
;+=============================================
;|                     Probing Cycle in Progress!"
;|
;|                 Press "Cycle Cancle" to terminate
;+=============================================
M200 "Press Cycle Start to begin Probing Cycle\nPress Cycle Cancle to Exit"

IF #50001 ;sync
G65 "c:\cncm\ncfiles\probing_cycle.cnc"

; restore active unit of meassure after probing
IF #33999 == 20 THEN G20 ELSE G21

; display message if no Probing Cycle has been found
IF #30000 == -1 THEN M200 "No Probing Cycle found!\n\nPress Cycle Start"

; display Error Message if Probing Cycle was aborted
IF #30000 > 1 THEN G65 "#301\probe_error.cnc" A[#30000]

N1000	;End of Macro
```

When the probing icon on the VCP is touched, the modified mfunc58.mac file will start up the ProbeApp and let you make any selections.
You can also use one of the Macro buttons on the MPG to start up the ProbeApp. If M58 is already taken on your system, you can move it to any other AUX button you have available.

When you press Start in the ProbeApp, it will generate all the probing moves based on your selection and will give the control back to CNC12 to execute them.


![](/images/pa002.PNG)


The probing moves are saved in the file
```
c:\cncm\ncfiles\probing_cycle.cnc
```
This file name and location can be changed in the configuration settings.

A output for a typical probing cycle looks like this:
```
;Boss Probing Macro Start

#30000 = 1    ; Global Error Variable reset to 1
#34570 = 1    ; Start Axis X=1 Y=2
#34571 = 1    ; Probing Cycle Orientation Plus Direction = 1, Negative Direction = -1
#34572 = 39   ; Boss Diameter
#34573 = 10   ; Probing Cycle Z-Clearance

G65 "C:\cncm\probing\probe_initialize.cnc"
G65 "#301\probe_boss.cnc" X[#34572] Z[#34573] E[#34570] D[#34571] F[#34018] B[#34019]
If #30000 == 0 || #30000 > 1 Then M99  ; Error occured In last probing cycle

#34578 = #34562  ; width 1 from boss macro
#34579 = #34563  ; width 2 from boss macro

#100 = 0
M225 #100 "Boss Diameter = %f\n\nPress Cycle Start to Continue" #34579

#100 = 0
M225 #100 "Set WCS:\n\nAxis: X, WCS: Active, Set to: 0\nAxis: Y, WCS: Active, Set to: 0\n\n\n\nPress Cycle Start to Set!\nPress Cycle Cancel to Abort"
#29000 = #4014
G92 X[0 + 0]
G92 Y[0 + 0]
IF #29000 < 60 THEN G[#29000] ELSE E[#29000 - 53]

M99
```

Instead of starting the ProbeApp from the VCP or MPG, you could start it from anywhere in your program with a M58 command when probing is needed. 
As an example, if you need to set the Tool Height Offset after a Tool Change, you could add a M58 command to the end of the mfunc6.mac file.
After a Tool Change the ProbeApp would be started to let you touch off the tool and the program would continue afterwards.

It is also possible to open a certain probing cycle directly by adding a startup parameter to the call that opens the ProbeApp. 
This call will open directly the screen of the Tripple Corner Plate Probing Cycle:

```
G65 "c:\cncm\probing\ProbeApp.exe -cornerplate
```

Use the start-up parameter -help to see all the available parameters

* [Write CNC12 Info Variables](CNC12.md)

![](/images/pp001.PNG)

# Additional Post Processor Logic
Additional logic has been added to the Post Processor to support

* NEW in v2 [Using Inverse Time Feedrate for Rotary Axis](inverseTime.md)
* [Check for conflicting Tool Numbers (same Number but different Tools)](checkDuplicateTools.md)
* [Support for Manual NC Commands](manualNC.md)


