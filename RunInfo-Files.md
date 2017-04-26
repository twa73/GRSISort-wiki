RunInfo files are parse like normal command line arguments minus the leading dash(es). The argument and its value are separated by a colon.

# Using RunInfo files #

RunInfo files have an extension of `.info` and are called in the command line just like a calibration file:
```
grsisort run04242_000.mid mycal.cal myruninfo.info
```
or
```
grsisort -a fragment04242_000.root mycal.cal myruninfo.info
```
Multiple .info file inputs on the command line overwrite each other, i.e. if one parameter is set in multiple .info files, the value from the last .info on the command line is the one that GRSISort uses.
Also, if a command line argument in one of the .info files is given on the command line after the .info file, the one on the command line overwrites the one in the .info file.

# RunInfo file format #
An [example RunInfo file](https://github.com/GRIFFINCollaboration/GRSISort/blob/master/example/grsiruninfoexample.info) is located in the `example` directory and is copied here:
```
build-window:   200
addback-window: 150.0
write-diagnostics: 
separate-out-of-order: 
```
At the moment (version 3.1.3.1) the RunInfo file doesn't recognize any comments, so it isn't possible to add comments to it.

This example specifies the following parameters:
- The event building window is set with `build-window` and in this example is set to 200 (which, with our 10 ns clock ticks is actually 2 microseconds).
- The addback time window is set with `addback-window` and in this examples is set to 150 ns.
- Diagnostics from the sorting are written to file with `write-diagnostics` which doesn't take any argument.
- Events that were out of order during the event building are written separately with `separate-out-of-order` which also doesn't take any arguments.

For more parameters see the output of `grsisort -hq`.
