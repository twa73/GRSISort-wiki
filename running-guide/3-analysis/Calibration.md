<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**ANALYSIS**](Analysis) > [**CALIBRATION**](Calibration)

GRSISort calibrations are read from *cal files*. These are the files with the **.cal** extension. Various calibration classes are used to set calibration parameters. These classes currently include:
- [[TGainMatch | Gain-Matching]] for gainmatching

#TCalManager
A Calibration Manager is used to make doing multiple channel calibrations easier. A `TCalManager` can be filled with one type of calibration class at any time. To create a calibration manager use:
```c++
TCalManager *myCalManager = new TCalManager;
```
You can then put a calibration in the cal manager by using
```c++
myCalManager->AddToManager(myCal);
```
where `myCal` is a pointer to the calibration being added to the manager. It should be noted that a *deep copy* of the calibration is made and the cal manager appropriately handles it's copy. If the channel for the calibration has not been set yet, it can be set while being added to the calibration manager using
```c++
myCalManager->AddToManager(myCal,channel_number);
```
and to overwrite the cal at a given channel, you must use the option `"overwrite"`. Fore example,
```c++
myCalManager->AddToManager(myCal,channel_number,"overwrite");
```
To access the calibration for a specific channel in the cal manager you can use
```c++
myCalManager->GetCal(channel_number);
```
and to write the set of calibrations to `TChannel` call 
```c++
myCalManager->WriteToChannel();
```


