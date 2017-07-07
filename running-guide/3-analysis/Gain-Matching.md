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
To effectively use `TGainMatch`, we need to make sure the gains for each channel are set roughly to the correct value. If this isn't already the case, you can use the `CoarseMatch` method of TGainMatch. This method works best on sources with two very strong transitions such as 60Co. To gain match a specific channel, use:
```c++
   myGain->CoarseMatch(histogram,channel_number,low_energy,high_energy);
```
where `histogram` is the `TH1*` to gain match, `channel_number` is used if you want to gain match a specific channel, `low_energy` is the lowest energy peak to gain match, and `high_energy` is the highest energy peak to gain match. `low_energy` and `high_energy` are defaulted to the 60Co values.

Once the gain-matching is complete, the energy coefficients can be written to the TChannel,
```c++
myGain->WriteToChannel();
```
and the TChannels can all be written to a cal file using the normal `TChannel::WriteCalFile("name_of_cal.cal")`

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
TGainMatch::CoarseMatchAll(myCalManager,energy_chan_mat, low_energy, high_energy);
```
where `energy_chan_mat` is the `TH2*` of Charge vs. ChannelNumber, `low_energy` is the lowest energy peak in the gain match, and `high_energy` is the highest energy peak in the gain match. `low_energy` and `high_energy` are defaulted to the 60Co peaks. Once the `TGainMatch::CoarseMatchAll` is finished, `myCalManager` will be filled with the gains of every channel fit. It should be noted that the cal manager is filled with *a deep copy* of the TGainMatch and not a reference to it. [[TCalManager|Calibration]] explains how to access the various TGainMatches contained it.
Finally, to write these gain coefficients to the Channels, use
```c++
myCalManager->WriteToChannel();
```
This calls the `WriteToChannel()` function for each `TGainMatch` contained in the `TCalManager`.

# Fine Gain Matching
Fine gain matching is what we normally think of gain matching as. The fine gain matching class will accept two charge histograms, and two peak energies, and attempt to calibrate these spectra accordingly. To fine gain match using one source, use:
```c++
   myGain->FineMatch(histogram,low_energy,high_energy);
```
If there are two different sources contained in two different spectra, use:
```c++
   myGain->FineMatch(low_histogram,low_energy, high_histogram, high_energy);
```
You may also pass a `TPeak*` instead of an energy to these functions. To fine gain match all of the channels, you can use the `FineMatchAll` function. It works just like the `FineMatch` function, except a `TH2*` is passed instead of a `TH1*`, and a `TCalManager` is required. You must pass the hit-pattern vs. charge as the `TH2*`. For example,
```c++
TGainMatch::FineMatchAll(myCalManager,hp_vs_charge, low_energy, high_energy); //One source
TGainMatch::FineMatchAll(myCalManager,hp_vs_charge_low, low_energy, hp_vs_charge_high, high_energy); //Two Sources
```
To write the fine gains to the channel, you do it the same as with course gains. 
```c++
myGain->WriteToChannel();
```
Or with a `TCalManager` full of gains
```c++
myCalManager->WriteToChannel();
```
