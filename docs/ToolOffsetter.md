# Tool Offset Manager

## Description
Choosing the correct Tool Height Offset Method and measuring Tool Height Offsets properly is something many people struggle with.
The Tool Offset Manager is designed to make the selection and configuration of the correct Tool Height Offset Measurement Method an easy and straightforward process.

When the Tool Offsetter is launched for the first time, a guided setup will walk you trough a questionnaire and will suggest the appropriate Tool Height Offset Measurement Method based on the answers to the questions.

## Guided Setup with Questionnaire
### Tool Holding

![](/images/pa102.PNG)

If the machine spindle does not have exchangeable Tool Holders where each tool has its own holder that allows the tool to have a constant tool height after each tool change, the CNC12 Tool Offset Library cannot be used for Tool Height Offsets.

Select here the type of Tool Holding the machines has.

### Probing Devices

![](/images/pa103.PNG)

Check all the Probing Devices that are configured for this system. 
If the movable Tool Touch Off Device has a double role as a movable and fixed device, check both Movable and Fixed Touch Off Device. 

If a system does not have any Tool Touch Off Devices and all Tools are being touched off manually at a fixed location, select **No Tool Touch Off Device but fix Tool Touch Off Location**.

### Machine Type

![](/images/pa104.PNG)

Select if the machine has a constant distance between the Z-Home position and the machine table.
If the distance is fixed, there will be the option to measure the repeatable accuracy of the Z-Home position. 
If the Z-Home switch doesn't provide a good enough repeatable accuracy, the **Variable Distance between Z-Home Position and Machine Table** should be used.

### Tool Offset Manager Configuration

![](/images/pa105.PNG)

Based on the answers to the Guided Setup, the configuration settings will be pre-set with the applicable Reference Height Method.
Additional settings allow to further customizing the Tool Height Measurement cycle.

For details on these additional settings check one of the following chapters that apply for the selected Reference Height Method for your machine:

* [Fix Z-Height Method](FixHeight.md)
* [Reference Tool Method](ReferenceMethod.md)
* [Don't use Offset Library](NoOffsetLibrary.md)



[Back](index.md)

