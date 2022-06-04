[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**CAL FILES**](CalFiles)

# Cal Files #
Calibration files (cal files) are the files ending in a __.cal__ extension.
They are used to perform a variety of functions:
- Map digitizer address to Channel Number
- Set a mnemonic for each channel
- Set a digitizer type (this determines the number of ns per timestamp unit)
- Calibrate energy
- Set a charge integration parameter
- Calibrate CFD, LED and time
- Calibrate efficiency
- Set a time offset for a channel's timestamps (LaBr)

Some of this information is contained in the ODB which is read out by __grsisort__ from the start of the __.mid__ file.
The [[Calibration | Calibration ]] classes can be used to set these variables in TChannel and create cal files as well.

## Using Cal Files #
When a cal file is input during the MIDAS sorting stage, the calibration data is saved to the fragment tree.
After this, `grsisort` defaults to reading the cal file from the fragment tree.
If a different calibration is desired, `TChannel::ReadCalFile("*name_of_cal_file*")` can be called to read in a different cal file.

__NOTE:__ This data is not written to the tree unless `TChannel::WriteToRoot()` is called.

Once a calibration from a cal file is active, access to the various calibrations is straight forward.
For example, one can access the calibrated energy for a fragment using the `GetEnergy()` command.

Finally, if you would like to create a new calibration file from a fragment tree you can run the command `TChannel::WriteCalFile("<calibrationfile>.cal")`.

## Cal File Format #
Calibration files are in a plain-text format that is easily modifiable.
The labels are case-insensitive, but colons must be used to separate labels from values.
Lines that begin with `//` are comments.

The file is a listing of calibration parameters for individual mnemonics, like the listing below for the blue crystal in GRIFFIN clover 1:
```
GRG01BN00A	{
Name:      GRG01BN00A
Number:    1
Address:   0x00000000
Digitizer: 
EngCoeff:  -0.743423	1.419230	
Integration: 0
ENGChi2:     0.000000
EffCoeff:  
EFFChi2:   0.000000
TIMECoeff:  0.000000	271.279000	-0.739159	
TimeOffset: 0.0
}
```
``Name`` sets the mnemonic for the data from the address listed with ``Address`` (the address is the actual electronic channel this data came from).
If this calibration file is read in while sorting a MIDAS file to a `FragmentTree`, `Number` will be the `ChannelNumber` assigned to this address in the `FragmentTree`.

Beyond these three primary numbers, any additional lines will set calibration values for this channel.
- `EngCoeff` sets the energy calibration coefficients, assuming a polynomial calibration from charge to energy.
The first value is the zeroth-order coefficient (offset, set to `-0.743423` in the example above), the second is the first-order coefficient (slope, set to `1.419230` in the example above), etc.
- `Integration` sets the integration parameter that is used in the energy calibration.
- `TIMECoeff` or `Walk` sets the coefficients for the low-energy walk correction of the form t<sub>diff</sub> = a<sub>0</sub>+a<sub>1</sub>E<sup>a<sub>2</sub></sup>.
  In the example above, a<sub>0</sub> is 0, a<sub>1</sub> is 271.279, and a<sub>2</sub> is -0.739159.
- `TimeOffset` is an offset to be removed from the time stamp from the hit before sorting the fragments into events.
  Useful for TAC's.
  The returned value is Timestamp - TimeOffset.
- `Digitizer` sets the digitizer type which determines how many nanoseconds each timestamp unit corresponds to.
- `CFDCoeff` similar to `EngCoeff` this can set parameters for a polynomial function using the CFD value as input.
  This is mainly used to set an offset for small corrections in the timing **after** the fragments have been sorted into events.

Other possible calibration parameters include:
- `Number`
- `Stream`
- `EngChi2`
- `CFDChi2`
- `LEDChi2`
- `TimeChi2`
- `EffChi2`
- `LEDCoeff`
- `EffCoeff`
- `FileInt`

## ODB #
Using **odbgains.C** on a cal file writes a bash script that will allows easy setting of the odb gains parameters for GRIFFIN.
