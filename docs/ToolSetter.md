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




[Back](index.md)

