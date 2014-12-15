[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**CAL FILES**](CalFiles)

# Cal Files #
Calibration files (Cal Files) are the files ending in a __.cal__ extension. They are used to perform a variety of functions:
- Map digitizer address to Channel Number
- Set a mnemonic for each channel
- Calibrate energy
- Set a charge integration parameter
- Calibrate CFD, LED and Time
- Calibrate Efficiency

Some of this information is contained in the ODB which is read out by __grsisort__ from the start of the __.mid__ file. 

## Using Cal Files #
When a cal file is input during the MIDAS sorting stage, the calibration data is saved to the fragment tree. After this, `grsisort` defaults to reading the cal file from the fragment tree. If a different calibration is desired, `Channel::ReadCalFile("*name_of_cal_file*")` can be called to read in a different cal file. 
__NOTE:__ This data is not written to the tree unless the midas file is sorted with the cal file.

Once a calibration from a cal file is active, access to the various calibrations is straight forward. For example, one can access the calibrated energy for a fragment using the `GetEnergy()` command.

Finally, if you would like to create a new calibration file from a fragment tree you can run the command `TChannel::WriteCalFile("<calibrationfile>.cal")`.

#### ODB #
Using **odbgains.C** on a cal file writes a bash script that will allows easy setting of the odb gains parameters for GRIFFIN.