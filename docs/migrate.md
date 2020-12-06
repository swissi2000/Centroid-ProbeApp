# ProbeApp Migrate to a new Version of CNC12


## Migration Steps using CNC12 Report-File restore

* Follow Centroids Instructions on how to Upgrade CNC12 to the new version
* After the new Version of CNC12 is installed and the Report file has been restored, simply copy over the **C:\cncm\probing** folder from the backup of your old CNC12 version to the new one.
* Verify that the mfunc58.mac (and other files like mfunc6.mac if you customized them for the ProbeApp) have been migrated over properly by the CNC12 Report restore process.
* Launch ProbeApp and it should start up like it did in the old version of CNC12 


## Migration Steps with a fresh install of CNC12 (not restoring old settings with Report-File restore)

* Follow Centroids Instructions on how to install a the new, fresh version of CNC12
* After the new Version of CNC12 is installed open the Windows **Add or remove programs**
* Select **ProbeApp-Setup** and press **Modify**

![](/images/pa131.png)

* Select **Repair ProbeApp Setup**

![](/images/pa132.png)

* Copy the file **ProbeApp.cfg** from the backup of your old **\cncm\probing** folder to the new **C:\cncm\probing** folder. This file contains all the configuration settings for the ProbeApp.



[Back](index.md)

