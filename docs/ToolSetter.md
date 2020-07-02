# Universal Tool Setter

## Description
These Tool Setter Cycles are for machines with spindles that have no fixed tool holders and can be used to set the machines WCS 0 for each tool by measuring the Tools Height Offset
and are designed to be integrated with a M6 Tool Change Macro.

Adding a simple M58 command into the *mfunc6.mac* Tool Change Macro will bring up the Tool Setter screen for every tool change:

![](/images/pa065.PNG)

These Probing Cycles require that all tools are configured in the CNC12 Tool Offset Library with a Height Offset of 0. The ProbeApp will issue a *Warning* if this is not the case.
Before these probing cycles can be used for the first time, the *Configuration* Button needs to be pressed to confirm Probe settings and probing options.
See the *Configuration* chapter for each Method below for details. 

The Universal Tool Setter Cycles support 3 different Tool Setting Methods:

* Using a Movable and a Fixed Tool Touch Off Plate
* Using a single, Movable Tool Touch Off Plate
* Manual Tool Touch Off

## *Method 1:* Using a Movable and a Fixed Tool Touch Off Plate

![](/images/pa065.PNG)

This method uses two Tool Touch Off Plates (TT). The movable TT is being placed on top of the workpiece to find the exact position of the top surface,
the second TT is installed in a fixed location and is being used to automatically touch off all subsequent tools on the same workpiece.

This method has three cycle options. Read  the description for each option carefully and make sure you select the appropriate cycle for each Tool Change at the right time.


### Cycle 1: Probe Z0 and Reference Height Offset 

![](/images/pa070.PNG)

This cycle is being used on the first Tool Change after a new workpiece has been mounted that has an unknown top surface position. *Cycle 1* involves two probing cycles.

The first probing cycle is using the movable TT on top of the workpiece and will measure the exact position of the top surface and will also set Z0 on top of the surface for the first tool.

The second probing cycle is using the Fixed TT and is needed to calculate the *Reference Height Offset* which is the distance between the top of the Fixed TT and the top of the workpiece.

The Reference Height Offset is used to correctly set the Tool Height Offset for subsequent tool changes on the same workpiece by using the Fixed TT only.


### Cycle 2: Set Z0 with Fixed TT - Ref Height Offset 

![](/images/pa071.PNG)

After a *Reference Height Offset* has been established with *Cycle 1*, all subsequent Tool Changes on the same workpiece can be done with the Fixed TT only.

Use this option for all subsequent Tool Changes on the same workpiece after the *Reference Tool Height* has been set with *Cycle 1*.


### Cycle 3: Continue without Tool Probing 

![](/images/pa072.PNG)

If the Tool Setter screen is being launched by a M6 Tool Change command but no Tool Touch Off is needed, the *Cycle 3* button allows to continue the job without a Tool Touch Off.

### Configuration for Movable and Fixed TT Method

![](/images/pa066.PNG)

The Configuration screen allows for a very flexible customization of the probing cycle. 
These options can be changed at any time and the ProbeApp will dynamically generate the necessary probing commands for CNC12 based on current configuration settings. 

These are the descriptions what each option does:

* *Show Message before Probing Movable TT:* If this option is checked, CNC12 will display a Message before the probing move with the Movable TT is executed. The message can be customized in the *Message* box (\n does create a new line in the message box). Be aware that not checking this box does directly engage the probing cycle when the *Cycle Start* button is pressed.
* *Show Message after Probing Movable TT before Moving to Fixed TT:* Test

## *Method 2:* Using a single, Movable Tool Touch Off Plate

![](/images/pa067.PNG)

![](/images/pa068.PNG)

## *Method 3:* Manual Tool Touch Off

![](/images/pa069.PNG)




[Back](index.md)

