# Probe Initialization File
Every probing cycles calls the initialization file at the beginning of the cycle to get all the parameters initialized properly:
```
c:\cncm\probing\probe_initilize.cnc
```

The relevant parameters for the ProbeApp are in the *General Parameters* section of this file:

```
;---------------------------------------------------------------------------------------------
; General Parameters
;---------------------------------------------------------------------------------------------
#33999 = #4006                     ; Save active Units (Metric/Inch). Will be restored at the end of the probing cycle
#34000 = #9014                     ; Fast Probing Rate from System Parameter #14
#34017 = #9015                     ; Slow Probing Rate from System Parameter #15
#34002 = 50001                     ; Sync Variable to stop look ahead
#34005 = #9011                     ; Probe PLC Input from System Parameter #11
#34009 = #[11000+#[13000+[#9012]]] ; Probe Diameter - diameter_offset[tool_D_number[probe_tool_number]]
#34018 = #9395                     ; Probing Setup Traverse Speed from System Parameter #395
#34019 = #9396                     ; Probing Setup Plunge Speed from System Parameter #396
#30021 = 1                         ; Unit Convertion Factor always 1 for ProbeApp as probing is forced to machine units
#34957 = #9013                     ; Probing Recovery Distance from System Parameter #13
```

## Fast Probing Rate (#34000)
The default fast probing feedrate is the *Fast Probing Rate* configured in the CNC12 System Parameter #14

This feedrate can be changed in the Probe Tab of the CNC12 Wizard   

![](/images/pa054.PNG)

## Slow Probing Rate (#34017)
The default slow probing feedrate is the *Slow Probing Rate* configured in the CNC12 System Parameter #15

This feedrate can be changed in the Probe Tab of the CNC12 Wizard (see picture above).  


## Probing Recovery Distance (#34957)
This parameter specifies the distance the probe will retract after a probe trigger contact. 

The default distance is the *Probing Recovery Distance* configured in the CNC12 System Parameter #13

This distance can be changed in the Probe Tab of the CNC12 Wizard (see picture above).  


## Sync Variable (#34002)
The sync variable is being used to stop CNC12 from doing a code look ahead that could result in unpredictable parameter changes ahead of the actual block execution.
There should be no need to modify this variable as this is Centroid standard.

## Probe PLC Input (#34005)
This parameter holds the Probe Input # specified in CNC12 System Parameter #11 which will be configured properly by the CNC12 Wizard.

Make sure your Touch Probe and/or Tool Touchoff Plate are properly configured in the CNC12 Wizard and are working correctly in CNC12 before using the ProbeApp.

## Probe Diameter (#34009)
If a Touch Probe has been correctly configured in the CNC12 Wizard, the CNC12 System Parameter #12 holds the Tool # of the Touch Probe.

![](/images/pa055.PNG)  ![](/images/pa056.PNG)

Based on this Tool # the Diameter of the Probe will be retrieved from the CNC12 Offset Library and stored in parameter #34005.

![](/images/pa057.PNG)

If you are using a Touch Probe with the ProbeApp, make sure all these settings are correctly configured for your Touch Probe.

## Probing Setup Traverse Speed (#34018)
This is the feedrate the probing cycle will use to travel from one probing location to another.
The default traverse speedrate is the *Probing Setup Traverse Speed* configured in CNC12 System Parameter #395.

Note that this parameter cannot be changed with the CNC12 Wizard and needs to be configured directly within CNC12 (Setup[F1] --> Config[F3] --> Parms[F3]).

![](/images/pa058.PNG)

All traverse probing moves are protected moves. That means the machine movement will stop when an unexpected probe trip occurs while traversing. 
To reduce probing cycle times, this feedrate can be increased to the point where the machine can still stop within a distance that is save for the probe.

## Probing Setup Plunge Speed (#34019)
This is the feedrate the probing cycle will use to drop down on the Z-Axis to get to the Z-level for the next probing move. 
It's also been used to retract the probe up to the Z-Clearance Height.

The default plunge speedrate is the *Probing Setup Plunge Speed* configured in CNC12 System Parameter #396.

Note that this parameter cannot be changed with the CNC12 Wizard and needs to be configured directly within CNC12 (Setup[F1] --> Config[F3] --> Parms[F3]).

All plunge probing moves are protected moves. That means the machine movement will stop when an unexpected probe trip occurs while plunging. 
To reduce probing cycle times, this feedrate can be increased to the point where the machine can still stop within a distance that is save for the probe.

## Unit Conversion Factor (#30021)
This factor is always 1 as all probing cycles in the ProbeApp are forced to the default machine units.

That means that all the values and configuration parameters used in the ProbeApp must be in the units your machine is configured in.

See section *Metric versus Imperial* for more information on units.



[Back](index.md)

