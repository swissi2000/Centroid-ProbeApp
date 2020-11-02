# Overview

* [ProbeApp Installation Instructions](install.md)
* [ProbeApp Update Instructions](update.md)
* [ProbeApp Migrate to a new Version of CNC12](migrate.md)
* [How to get Started with ProbeApp](GetStarted.md)

![](/images/pa001.PNG)

The ProbeApp is a collection of probing cycles that integrate with the Centroid CNC12 control software.

These are the currently supported Probing Cycles (Click the links to get detailed information on each cycle):

* [Bore](Bore.md)
* [Boss](Boss.md)
* [Slot](Slot.md)
* [Web](Web.md)
* [Inside Corner](InCor.md)
* [Outside Corner](OutCor.md)
* [Single Axis](Single.md)
* [Find Angle](Angle.md)
* [3 Axis Center](Cube.md)
* [3 Axis Corner](TripleCorner.md)
* [Bore Plate](BorePlate.md)
* [Corner Plate](CornerPlate.md)
* [Square Plate](InCorPlate.md)
* [Tool Setter](ToolSetter.md)
* [Tool Offsetter](ToolOffsetter.md)

The ProbeApp is using it's own, enhanced probing cycles that are based on the original Centroid probing cycles with some additional, new cycles.
The enhancements of the ProbeApp probing cycles include fully qualified file names for all cycles, Axis Number independence, improved error handling and some Probe Retry features 
(e.g if the given clearance height of a boss measurement is to low, the probing cycle will walk up the Z-Axis until the probe can traverse to the other side or a limit is tripped).

**No Warranties are given. Use at your own risk!**


# ProbeApp Customization
There are two places where customizations can be done:

* [Configuration Button on the Main Screen of the ProbeApp](configuration.md)
* [probe_initialize.cnc File](probe_initilize.md)

Click the links above for a description of the customization options.


# ProbeApp Functionality dependend on CNC12 Version
The ProbeApp checks the version of CNC when being launched and adjusts functionality automatically to the restrictions of the different CNC12 versions.

This is the Main Screen of the ProbeApp when used with the Free version of CNC12:

![](/images/pa059.PNG)

ProbeApp provides the same Probing Cycles for the CNC12 Pro and Digitizing Version.
The only difference is the number of supported WCS:

* CNC12 Free: Only WCS #1 (G54) is available
* CNC12 Pro: WCS #1 - #6 (G54 - G59) are available
* CNC12 Digi: WCS #1 - #18 (E1 - E18 where E1 - E6 is the same as G54 - G59) 


# Metric versus Imperial Units
If no Job File is running, CNC12 will always default to the configured Machine Units which is G20 for an Imperial and G21 for a Metric system.
When the ProbeApp is launched while no job is running, the controller will already be set to the default machine units.

When the ProbeApp is being launched within a running job file (e.g the tool change file mfunc6.mac calls M58 to touch off the new tool with the ProbeApp) it is possible that the running job does not run in the default machine units.
In such a case the ProbeApp will force the machine into the default machine units to complete the probing cycle and switch the units back after the probing cycle has completed.

If the active units don't match the machine units when the ProbeApp generated Probing Cycle is executed by CNC, a warning message will be displayed

![](/images/pa060.PNG)

That means that the ProbeApp will **ALWAYS** run in the machines default units and all values and configuration parameters used in the ProbeApp **MUST BE** in the **Machines Default Units**.
 

# Implementation Details
The ProbeApp is a Windows Application that runs outside of CNC12 and supports Touch Screen Input. 
By default the ProbeApp uses M58 to integrate with CNC12, that means a customized mfunc58.mac will be copied to the c:\cncm folder and the M58 Button on the VCP 2.0 will be replaced with a Probing Icon.

```javascript
;--------------------------------------------------------------------------------
; Filename: mfunc58.mac 
; M58 macro
; Description: Provides Probing Routines when used with program ProbeApp.exe 
; Author: swissi
;---------------------------------------------------------------------------------
If #50001                                        ;Prevent lookahead from parsing past here
If #4201 || #4202 Then GOTO 1000                 ;Skip macro if graphing or searching

N10
#33999 = #4006                                   ;Store active units
G65 "c:\cncm\probing\probe_initialize.cnc" A99   ;Run Probe Initialization to check for Setup Errors (A99 = Don't check Inputs)
If #50001                                        ;Prevent lookahead from parsing past here
G65 "c:\cncm\probing\probe_save_parameters.cnc"  ;Save CNC12 Parameters to make them available in ProbeApp
If #50001                                        ;Prevent lookahead from parsing past here
;---------------------------------------------------------------------------------
;Possible ProbeApp Startup Options to directly open a specific Probing Cycle
;---------------------------------------------------------------------------------
M130 "C:\cncm\probing\ProbeApp.exe"               ;Startup ProbeApp Main Screen
;M130 "C:\cncm\probing\ProbeApp.exe -ToolSetter"   ;Startup ProbeApp directly with Universal Tool Setter Screen
;M130 "C:\cncm\probing\ProbeApp.exe -CornerPlate"  ;Startup ProbeApp directly with Corner Plate Screen
;M130 "C:\cncm\probing\ProbeApp.exe -InCorPlate"   ;Startup ProbeApp directly with Square Plate Screen
;M130 "C:\cncm\probing\ProbeApp.exe -BorePlate"    ;Startup ProbeApp directly with Bore Plate Screen

#30000 = 0                                         ;No probing Cycle saved when this remains at 0 
N100
G4 P1

;+=============================================
;|                     Probing Cycle in Progress!"
;|
;|                 Press "Cycle Cancle" to terminate
;+=============================================
M200 "Press Cycle Start to begin Probing Cycle\nPress Cycle Cancel to Exit"





IF #50001 ;sync
G65 "c:\cncm\ncfiles\probing_cycle.cnc"

; restore active unit of meassure after probing
IF #33999 == 20 THEN G20 ELSE G21

; display message if no Probing Cycle has been found
IF #30000 == -1 THEN M200 "No Probing Cycle found!\n\nPress Cycle Start to Continue, Cycle Cancel to Abort"

; display Error Message if Probing Cycle was aborted
IF #30000 > 1 THEN G65 "#301\probe_error.cnc" A[#30000]

IF #30000 == -99 Then GOTO 10  ; -99 indicates to reopen ProbeApp after Cycle
#29500 = 0  ; reset ProbeApp Startup Overwrite flag
N1000	;End of Macro
```
The ProbeApp and all related probing cycle files will be installed to the newly created folder
```
c:\cncm\probing
```

When the probing icon on the VCP is touched, the modified mfunc58.mac file will start up the ProbeApp and let you make any selections.
You can also use one of the Macro buttons on the MPG to start up the ProbeApp. If M58 is already taken on your system, you can move it to any other AUX button you have available.


![](/images/pa002.PNG)


This is the screen of the Boss Probing Cycle as an example where the start of the probing cycle is selected at the left side of the Boss.

Check out the links on top of this page for screen shots and details about all supported Probing Cycles.


![](/images/pa003.PNG)

When the START button is pressed, the ProbeApp will generate all the probing moves based on the selection and will give the control back to CNC12 to execute them.

![](/images/pa004.PNG)

The probing moves that CNC12 will execute are saved in the file
```
c:\cncm\ncfiles\probing_cycle.cnc
```
This file name and location can be changed in the configuration settings.

The output for the Boss Probing Cycle example above looks like this:
```
;Boss Probing Macro Start

#30000 = 1    ; Global Error Variable reset to 1
#34570 = 1    ; Start Axis X=1 Y=2
#34571 = 1    ; Probing Cycle Orientation Plus Direction = 1, Negative Direction = -1
#34572 = 50   ; Boss Diameter
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
As an example for Router users, if you need to set the Tool Height Offset after a Tool Change, you could add a M58 command to the end of the mfunc6.mac file.
After a Tool Change the ProbeApp would be started to let you touch off the tool and the program would continue afterwards.

It is also possible to open a certain probing cycle directly by adding a startup parameter to the ProbeApp launch command. 
This example will open directly the screen of the Tripple Corner Plate Probing Cycle:

```
M130 "c:\cncm\probing\ProbeApp.exe -cornerplate"
```

Use the start-up parameter -help to see all the available parameters

![](/images/pa005.PNG)


The values from each Probing Cycle screen are stored in the html configuration file 
```
c:\cncm\probing\ProbeApp.cfg. 
```
When re-opening the ProbeApp the next time, the probing cycle will have all the values from the last time the cycle was used.
The "*Last Cycle*" Button on the Main Screen will open the Probing Cycle that was last used in the ProbeApp.

