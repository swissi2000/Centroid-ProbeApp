# Universal Tool Setter

## Description
These Tool Setter Cycles are for machines with spindles that have no fixed tool holders and can be used to set the machines WCS 0 for each tool by measuring the Tools Height Offset
and are designed to be integrated with a M6 Tool Change Macro.

Adding a simple M58 command into the *mfunc6.mac* Tool Change Macro will bring up the Tool Setter screen for every tool change:

![](/images/pa065.PNG)

These Probing Cycles require that all tools are configured in the CNC12 Tool Offset Library with a Height Offset of 0. The ProbeApp will issue a *Warning* if this is not the case.
It is recommended that you run the ProbeApp **Tool Offsetter** first as it will make sure that all Tool Height Offsets in the CNNC12 Tool Offset Library are reset to 0 for the **Tool Setter** cycles to work properly.

Before these probing cycles can be used for the first time, the *Configuration* Button needs to be pressed to confirm Probe settings and probing options.
See the *Configuration* chapter for each Method below for details. 

The Universal Tool Setter Cycles support 3 different Tool Setting Methods. 

Click the links below for details on each Method:

* [Dual Probe Method](ToolSetterMod1.md)
* [Single Tool Touch Off Device Method](ToolSetterMod2.md)
* [Manual Tool Touch Off](ToolSetterMod3.md)

## Integration Options
By default the ProbeApp does integrate with CNC12 via M58 by installing a customized mfunc58.mac file into the *C:\cncm* folder.

Pressing the Probe Button on the VCP or commanding a M58 command will bring up the ProbeApp Main screen by default. This behavior can be customized in the *mfunc58.mac* file:

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

If it is preferred that the Probe Button on the VCP or a M58 command will directly open the Tool Setter screen instead of the Main screen, modify the *mfunc58.mac* file as shown above.

Version 2 of the ProbeApp now offers a new method to temporarely overwrite the default ProbeApp Cycle screen by setting the CNC12 variable #29500 bevore calling the M58 command.

These are the possible values:

``` javascript
#29500 = 0  ; Main Menu (Default)
#29500 = 1  ; Angle
#29500 = 2  ; Bore
#29500 = 3  ; BorePlate
#29500 = 4  ; Boss
#29500 = 5  ; CornerPlate
#29500 = 6  ; Cube
#29500 = 7  ; InCor
#29500 = 8  ; InCorPlate
#29500 = 9  : OutCor
#29500 = 10 ; Single
#29500 = 11 ; Slot
#29500 = 12 ; ToolOffsetter
#29500 = 13 ; ToolSetter
#29500 = 14 ; TripleCorner
#29500 = 15 ; Web
```

Use the #29500 to temporarely overwrite the default opening Probing Cycle specified in the **mfunc58.mac** macro file.

Here's an Example of a minimalistic *mfunc6.mac* file that will open the **ProbeApp-Tool Setter** at every M6 Tool Change:

``` javascript
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
; Display Tool Change Message
;------------------------------------------------------------------------------
IF #50001									;Force lookahead to stop processing
#101 = 0
M225 #101 "#)Insert Tool #%.0f\nD:%.3f    H:%.3f\n\nPress Cycle Start to continue" #4120 #[11000 + #4120] #[10000 + #4120]

;------------------------------------------------------------------------------
; ProbeApp Call
;------------------------------------------------------------------------------
#29500 = 13                                 ;Opens the ProbeApp directly with Tool Setter screen
M58

N1000                                       ;End of macro  
```

Check out and customize the file *mfunc6.mac.customize-for-ProbeApp* that has been placed into the *C:\cncm* folder by the ProbeApp installation process.
Rename the file to *mfunc6.mac* to activate it in CNC12.



[Back](index.md)

