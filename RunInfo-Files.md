RunInfo files set non-channel-related variables like the event construction window.

# Using RunInfo files #

RunInfo files have an extension of `.info` and are called in the command line just like a calibration file:
```
grsisort run04242_000.mid mycal.cal myruninfo.info
```
or
```
grsisort -a fragment04242_000.root mycal.cal myruninfo.info
```

# RunInfo file format #
An [example RunInfo file](https://github.com/GRIFFINCollaboration/GRSISort/blob/master/util/grsiruninfoexample.info) is located in the `util` directory and is copied here:
```
/////////////////////////////////////
/////    Example *.info file   //////
/////////////////////////////////////


//The event building time, 10ns units. 
BuildWindow: 200     // 2useconds

//The Addback event window, 10ns units.
AddBackWindow: 15.0  // 150.0 nseconds

//The Array position in mm.
HPGePos:       110.0 // 110.0 mm 
```

As with the calibration file, the RunInfo file interprets `//` as a comment and the parameter names are case-insensitive. This example specifies the following parameters:
- The event building window is set with `BuildWindow` and in this example is set to 200 (which, with our 10 ns clock ticks is actually 2 microseconds).
- The addback time window is set with `AddbackWindow` and in this examples is set to 15 (or 150 ns).
- The radial position of the clover faces is set with `HPGePos`. Here, it is set to 110.0 mm. Other GRIFFIN values are 145 and 165 mm.