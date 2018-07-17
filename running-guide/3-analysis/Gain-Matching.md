<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**ANALYSIS**](Analysis) > [**CALIBRATION**](Calibration) > [**GAIN-MATCHING**](Gain-Matching)

   
To easily gain-match, you can use the `TGainMatch` class. A separate TGainMatch is created for each channel. You can create a TGainMatch by invoking:
```c++
TGainMatch *myGain = new TGainMatch; 
```
You can specifically set the channel of the `TGainMatch` using
```c++
myGain->SetChannel(channel_number);
```

# Coarse Gains
To effectively gainmatch automatically, we need to make sure the gains for each channel are set roughly to the correct value. If this isn't already the case, you can use the `CoarseMatch` method of TGainMatch. This method works best on sources with two very strong transitions such as 60Co. To gain match a specific channel, use:
```c++
   myGain->CoarseMatch(histogram,channel_number,low_energy,high_energy);
```
where `histogram` is the `TH1*` to gain match, `channel_number` is used if you want to gain match a specific channel, `low_energy` is the lowest energy peak to gain match, and `high_energy` is the highest energy peak to gain match. `low_energy` and `high_energy` are defaulted to the 60Co values.

Once the gain-matching is complete, the energy coefficients can be written to the TChannel,
```c++
myGain->WriteToChannel();
```
and the TChannels can all be written to a cal file using the normal `TChannel::WriteCalFile("name_of_cal.cal")`

The way coarsematching works is by searching out the two largest peaks, and calling that the low and high energy peaks. This is why this works best for 60Co. These peaks are found, fit, and a gain and offset is calculated. As these peaks are very close together, this is just to roughly determine the calibration parameters as almost every energy will be an extrapolation from these two close points. Once the data is coarse matched, the gains can be more finely gainmatched.

### Using CalManager
You can add TGainMatch instances to a [[TCalManager | Calibration]]. This can be done using:
```c++
myCalManager->AddToManager(myGain);
```
If the channel has not been set for the `TGainMatch`, you can also call
```c++
myCalManager->AddToManager(myGain,channel_number);
```
To automate the gain-matching and filling of a cal manager, you can use the static function:
```c++
TGainMatch::CoarseMatchAll(myCalManager,charge_chan_mat, low_energy, high_energy);
```
where `energy_chan_mat` is the `TH2*` of Charge vs. ChannelNumber, `low_energy` is the lowest energy peak in the gain match, and `high_energy` is the highest energy peak in the gain match. `low_energy` and `high_energy` are defaulted to the 60Co peaks (see above). Once the `TGainMatch::CoarseMatchAll` is finished, `myCalManager` will be filled with the gains of every channel fit. It should be noted that the cal manager is filled with *a deep copy* of the TGainMatch and not a reference to it. [[TCalManager|Calibration]] explains how to access the various TGainMatches contained it.
Finally, to write these gain coefficients to the Channels, use
```c++
myCalManager->WriteToChannel();
```
This calls the `WriteToChannel()` function for each `TGainMatch` contained in the `TCalManager`.

# Fine Gain Matching
Fine gain matching is what we normally think of gain matching as. The fine gain matching class will accept two charge histograms, a test channel, two peak energies, and attempt to calibrate these spectra accordingly. The way this works is first by attempting to match up the test channel (something reasonably calibrated) with the current channel to determine if there were any large gain jumps. Once this is done, the newly roughly matched energy spectrum is used to determine roughly were the gamma rays should be in the charge spectrum. The peaks are found in the charge spectrum, fit, and a new "fine matched" set of energy coefficients are created.

To fine gain match using one source, use:
```c++
   myGain->FineMatch(energy_histogram, test_histogram, charge_histogram, low_energy, high_energy);
```
To fine gain match all of the channels, you can use the `FineMatchAll` function. It works just like the `FineMatch` function, except a `TH2*` is passed instead of a `TH1*`, and a `TCalManager` is required. You must pass the hit-pattern vs. charge as the `TH2*`. For example,
```c++
TGainMatch::FineMatchAll(myCalManager, hp_vs_charge, hp_vs_energy, test_histogram, low_energy, high_energy);
```
To write the fine gains to the channel, you do it the same as with course gains. 
```c++
myGain->WriteToChannel();
```
Or with a `TCalManager` full of gains
```c++
myCalManager->WriteToChannel();
```
# Manual Gain Matching
You can also calculate gains manually. One you have fit two peaks in the charge spectrum, you can use:
```c++
myGainMatch->CalculateGain(centroid_1, centroid_2, energy_1, energy_2);
```
A print out of the `p0` offset and `p1` gain will be created for that channel. This can be copied into your calibration file.