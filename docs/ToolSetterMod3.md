# Method 3: Manual Tool Touch Off

![](/images/pa069.PNG)

This method does not use any TT and requires to touch off the tool manually on top of the workpiece.

Two options can be selected on the bottom right to customize the probing cycle:

* *Retract Z to:* Allows to set a specific return position the Z-Axis will move to after the WCS has been set.
* *Use WCS#:* This allows to select the WCS# that's being reset by this probing move. Default is always the currently active WCS#. If another WCS# than the currently active is selected, the machine will stay in the active WCS and only switch to the selected WCS for Reset and then switch back again.  


Two cycle options are available for this method. 


### Cycle 1: Manually Set WCS for active Tool to Z0 

![](/images/pa088.PNG)

Uses the Movable TT on top of the workpiece to probe the active tool and set Z0 (plus an optional offest) for the selected WCS#.

An optional *Offset* value can be specified in the box if needed.
As an example, if a shim is being used to touch off the tool on the surface that has a know thickness of 0.1mm, an Offset of 0.1 could be entered to account for the thickness of the shim.


### Cycle 2: Continue without Tool Probing 

![](/images/pa072.PNG)

If the Tool Setter screen is being launched by a M6 Tool Change command but no Tool Touch Off is needed, the *Cycle 2* button allows to continue the job without a Tool Touch Off.




[Back](ToolSetter.md)

