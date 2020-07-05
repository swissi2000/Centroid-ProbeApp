# Universal Tool Setter

## Description
These Tool Setter Cycles are for machines with spindles that have no fixed tool holders and can be used to set the machines WCS 0 for each tool by measuring the Tools Height Offset
and are designed to be integrated with a M6 Tool Change Macro.

Adding a simple M58 command into the *mfunc6.mac* Tool Change Macro will bring up the Tool Setter screen for every tool change:

![](/images/pa065.PNG)

These Probing Cycles require that all tools are configured in the CNC12 Tool Offset Library with a Height Offset of 0. The ProbeApp will issue a *Warning* if this is not the case.
Before these probing cycles can be used for the first time, the *Configuration* Button needs to be pressed to confirm Probe settings and probing options.
See the *Configuration* chapter for each Method below for details. 

The Universal Tool Setter Cycles support 3 different Tool Setting Methods. 

Click the links below for details on each Method:

* [Using a Movable and a Fixed Tool Touch Off Plate](ToolSetterMod1.md)
* [Using a single, Movable Tool Touch Off Plate](ToolSetterMod2.md)
* [Manual Tool Touch Off](ToolSetterMod3.md)

## Integration Options
By default the ProbeApp does integrate with CNC12 via M58 by installing a customized mfunc58.mac file into the *C:\cncm* folder.

A M58 command will bring up the ProbeApp Main screen by default. This behavior can be customized in the *mfunc58.mac* file:

``` javascript
;--------------------------------------------------------------------------------
; Filename: mfunc58.mac 
; M58 macro
; Description: Provides Probing Routines when used with program ProbeApp.exe 
; Author: swissi
;---------------------------------------------------------------------------------
If #50001                                        ;Prevent lookahead from parsing past here
If #4201 || #4202 Then GOTO 1000                 ;Skip macro if graphing or searching
.
.
;---------------------------------------------------------------------------------
;Possible ProbeApp Startup Options to directly open a specific Probing Cycle
;---------------------------------------------------------------------------------
;M130 "C:\cncm\probing\ProbeApp.exe"               ;Startup ProbeApp Main Screen
M130 "C:\cncm\probing\ProbeApp.exe -ToolSetter"   ;Startup ProbeApp directly with Universal Tool Setter Screen
;M130 "C:\cncm\probing\ProbeApp.exe -CornerPlate"  ;Startup ProbeApp directly with Corner Plate Screen
;M130 "C:\cncm\probing\ProbeApp.exe -InCorPlate"   ;Startup ProbeApp directly with Square Plate Screen
;M130 "C:\cncm\probing\ProbeApp.exe -BorePlate"    ;Startup ProbeApp directly with Bore Plate Screen
.
.
```

If it is preferred that a M58 command will directly open the Tool Setter screen instead of the Main screen, modify the *mfunc58.mac* file as shown above.
*NOTE* that changing the ProbeApp startup behavior in the *mfunc58.mac* file will also impact the behavior of the ProbeApp button on the Virtual Control panel. 

By adding a simple M58 into the M6 Tool Change command will integrate the Tool Setter into every Tool Change.

Here's an Example of a minimalistic *mfunc6.mac* file:
```
;------------------------------------------------------------------------------
; File     : mfunc6.mac
; Purpose  : Minimalistic Tool change macro for Acorn CNC12 with ProbeApp integration
;------------------------------------------------------------------------------

IF #50001                         			;Force lookahead to stop processing 
IF #4201 || #4202 THEN GOTO 1000   			;Skip when graphing or searching

;------------------------------------------------------------------------------
; Turn Off Spindle
;------------------------------------------------------------------------------
M5                                   

;------------------------------------------------------------------------------
; Retract Z to preferred Tool Change Position (uncomment your selection below)
;------------------------------------------------------------------------------
IF #50001                         			;Force lookahead to stop processing
G53 Z0                                      ;Default
;G28 G91 Z0
;G30 G91 Z0
;G30 P3 G91 Z0
;G30 P4 G91 Z0

;------------------------------------------------------------------------------
; Display Tool Change Message (uncomment your selection below)
;------------------------------------------------------------------------------
IF #50001									;Force lookahead to stop processing                          			
M225 #101 "#)Insert Tool #%.0f\n%s\nD:%.3f    H:%.3f\n\nPress Cycle Start to continue" #4120 #390 #[11000 + #4120] #[10000 + #4120]

;------------------------------------------------------------------------------
; ProbeApp Call
;------------------------------------------------------------------------------
M58

N1000                                       ;End of macro  
```

If it is preferred to have the ProbeApp button on the Virtual Control panel to bring up the ProbeApp Main screen but the M6 Tool Change command should bring up the Tool Setter,
use the file *mfunc6.mac.customize-for-ProbeApp* that has been placed into the *C:\cncm\* folder, customize it to your requirements and save it as *mfunc6.mac*.

```
;------------------------------------------------------------------------------
; File     : mfunc6.mac - to be customized for use with ProbeApp and/or Fusion 360 Post Processor with enhanced Features
; Purpose  : Tool change macro for Acorn CNC12 - Extended Information Diplay and ProbeApp support
;
; Usage    : Customize the sections below to your needs
;            Requires -swissi's Fusion 360 Post Processor Property "Write CNC12 Info Variables" to be enabled for extended Info Display
;            Requires -swissi's ProbeApp to integrate extensive Probing Functionality into the M6 Tool Change
; Author   : -swissi
; Date     : 22 March 2020
;------------------------------------------------------------------------------

IF #50001                         			;Force lookahead to stop processing 
IF #4201 || #4202 THEN GOTO 1000   			;Skip when graphing or searching

;------------------------------------------------------------------------------
; Turn Off Spindle
;------------------------------------------------------------------------------
M5                                   

;------------------------------------------------------------------------------
; Retract Z to preferred Tool Change Position (uncomment your selection below)
;------------------------------------------------------------------------------
IF #50001                         			;Force lookahead to stop processing
G53 Z0                                      ;Default
;G28 G91 Z0
;G30 G91 Z0
;G30 P3 G91 Z0
;G30 P4 G91 Z0

;------------------------------------------------------------------------------
; Display Tool Change Message (uncomment your selection below)
;------------------------------------------------------------------------------
IF #50001									;Force lookahead to stop processing                          			

;Default
M225 #101 "#)Insert Tool #%.0f\n%s\nD:%.3f    H:%.3f\n\nPress Cycle Start to continue" #4120 #390 #[11000 + #4120] #[10000 + #4120]

;use this for extended Info display when using -swissi's Fusion 360 Post Processor Property "Write CNC12 Variables"
;M225 #101 "#)Insert Tool #%.0f\n\nCNC12: T%.0f  D=%.3f  Height-Offset=%.3f\nFusion : %s\n%s\n\nFlutes=%s  Spindle Speed=%s %s  Cooling=%s\n\nWOC=%s  DOC=%s  Feed per Tooth=%s  Feed per Rev=%s\n\nFeed=%s  Ramp=%s  Plunge=%s\n\n      Press Cycle Start to continue\n" #4120 #4120 #[11000 + #4120] #[10000 + #4120] #330 #314 #311 #315 #316 #312 #317 #318 #320 #323 #319 #321 #322

;------------------------------------------------------------------------------
; ProbeApp Support Section (do not change this section below)
;------------------------------------------------------------------------------
IF #50001	
#33999 = #4006                                   ;Store active units
G65 "c:\cncm\probing\probe_initialize.cnc" A99   ;Run Probe Initialization to check for Setup Errors (A99 = Don't check Inputs)
If #50001                                        ;Prevent lookahead from parsing past here
G65 "c:\cncm\probing\probe_save_parameters.cnc"  ;Save CNC12 Parameters to make them available in ProbeApp
If #50001                                        ;Prevent lookahead from parsing past here

;---------------------------------------------------------------------------------------
;ProbeApp Startup Options to directly open a specific Probing Cycle (select as required)
;---------------------------------------------------------------------------------------
;M130 "C:\cncm\probing\ProbeApp.exe"               ;Startup ProbeApp Main Screen
M130 "C:\cncm\probing\ProbeApp.exe -ToolSetter"   ;Startup ProbeApp directly with Universal Tool Setter Screen
;M130 "C:\cncm\probing\ProbeApp.exe -CornerPlate"  ;Startup ProbeApp directly with Corner Plate Screen
;M130 "C:\cncm\probing\ProbeApp.exe -InCorPlate"   ;Startup ProbeApp directly with Square Plate Screen
;M130 "C:\cncm\probing\ProbeApp.exe -BorePlate"    ;Startup ProbeApp directly with Bore Plate Screen

;-----------------------------------------------------------------------------
;Section below required for ProbeApp (do not change section below)
;-----------------------------------------------------------------------------

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

N1000                                       ;End of macro  
``` 


[Back](index.md)

