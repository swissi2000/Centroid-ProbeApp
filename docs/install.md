# ProbeApp Installation Instructions


## Precautions
Be aware that the ProbeApp is using M58 as the default to integrate with CNC12.

ProbeApp will install the following files, overwriting the existing files:
```
C:\cncm\mfunc58.mac
C:\cncm\resources\vcp\Buttons\m58\m58.svg

[New Directory will be created]
C:\cncm\probing
(Directory contains ProbeApp.exe and all Probing Cycle files)
```

If you have customized one of these files already, you need to take steps to save your changes before installing the ProbeApp.
After the installation, you can customize the ProbeApp integration in any way you like but make sure the logic from mfunc58.mac is migrated accordingly for the ProbeApp to work correctly.

## Installation Steps

* Download *ProbeApp-Setup.msi* from the link that was provided
* Make sure CNC12 is shut down
* Double click *ProbeApp-Setup.msi* and accept all Defaults
* Start Up CNC12
* The Virtual Control Panel should show the ProbeApp Icon:

![](/images/pa062.PNG)

* Pressing the ProbeApp Icon should launch the ProbeApp now:

![](/images/pa001.PNG)

* Make sure you read this User Guide for details on Integration and Functionality

## Installing a newer Version of ProbeApp (Updating)
When trying to install a newer version of the ProbeApp over an existing Installation, you might get this message:

![](/images/pa063.PNG)

In this situation, launch Windows *Add or remove programs*

![](/images/pa064.PNG)

Click the *ProbeApp-Setup* icon and select *Uninstall*.

Note that the file *ProbeApp.cfg* that holds the history and configuration parameters of the ProbeApp will not be removed.






[Back](index.md)

