<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**ANALYSIS**](Analysis) > [**GAIN-MATCHING**](Gain-Matching)

   
To easily gain-match, you can use the `TGainMatch` class. A separate TGainMatch is created for each channel. You can create a TGainMatch by invoking:
```c++
TGainMatch *myGain = new TGainMatch; 
```
#Coarse Gains
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

###Using CalManager
You can add TGainMatch instances to a [[TCalManager | TCalManager]]



