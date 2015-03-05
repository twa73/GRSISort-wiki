[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort)

<a name="sortingmodel" />
#The Sorting Model
The steps involved in using GRSISort are:
[[/images/GRSIFlow.png]]

We are running a multistage sorting model with GRSISort. This has one main advantage: It allows us to sort the very large MIDAS files non-locally and then download the heavily compressed fragment tree data.

1. [Unpacking the Midas File](#unpackmid)
2. [Analyzing the Fragment Tree](#sortfrags)
3. [Histogramming the Final Results](#runhistos)

By default GRSIsort will open up a root session in the GRSI environment.
<a name="unpackmid" />
#Unpacking the MIDAS File
The strategy used by GRSISort is to unpack the MIDAS file and convert the information into a ROOT Tree format. This tree contains <a href="https://rawgit.com/wiki/r3dunlop/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/TFragment.html#TFragment:Data_Members" target="_blank">TFragments</a> and is essentially all of the information from the MIDAS File with higher compression and accessibility.
  
**grsisort NAME_OF_MIDAS_FILE.mid**  -> Converts the midas file into a fragment tree

Currently accepted flags for the midas sort:
  * __-suppress_error__,   suppresses errors from failed data parsing appearing in stdout
  * __-log_error__,        sends errors from failed data parsing to file, will not send errors if supress error option is also used!
  * __-no_waveforms__,     does not add the wave form stored for each event in the mid file to the fragment tree
  * __-q__,                quits the grsi environment after completing the midas sort
  * __-work_harder__,      inputting a `<macro>.C` file, runs that macro on the sorted file

If multiple **.mid** files are included at the time of sort, this can even include the wildcard character `*`, grsisort will sequentially sort all of these MIDAS files and write them to their own fragment trees.


<a name="sortfrags" />
#Sorting the Fragment Tree
**grsisort [-a -s] NAME_OF_FRAGMENT_TREE.root**   -> Starts sorting the fragment tree into (-s) user defined hists and/or (-a) analysis trees.
  * Histograms are defined in **users/UserInitObj.h**
  * How to fill the histogram is defined in **users/UserFillObj.h** 
  
If **-a** or **-s** are left off of the command, the fragment tree will be loaded into the grsi environment for the user to enjoy.

Currently accepted flags for the fragment tree sort:
  * __-a__,                creates an analysis tree to be used for doing a full analysis of the data
  * __-s__,                sorts the fragment tree into user defined hits. 
  * __-q__,                quits the grsi environment after completeing the fragment sort
  * __-no_speed__,         suppresses the output from the PROOF speedometer
  * __-work_harder__,      inputting a `<macro>.C` file, runs that macro on the sorted file

If multiple fragment trees are included at the time of the sort, or are created from multiple MIDAS files during the **autosort**, they will be sorted sequentially. However, it should be noted that although different fragment trees will be sorted into different analysis trees, if a **-s** sort is called, all of the fragment trees will be histogrammed into a single **hists.root**. If you would like to output multiple **hists.root** files, please run an external script to call grsisort multiple times.

##Calibrating the gamma spectra
When sorting the fragment tree, you can read in a [calibration file](CalFiles) at the same time and it will perform the calibrations while sorting. This can be done with the command:
```c++
grsisort -flags <midasfile>.mid <calibrationfile>.cal
```
A sample calibration file can be found in the users director and is called *NewGrifCal.cal*

##Viewing histograms within `grsisort`
After sorting the data and creating a fragment tree, you can view histograms by running the command:
```c++
FragmentTree->Draw("<variable> >> <histogramName>", "<cuts>", "<displayOptions>")
```

Here is an example that displays a 1D histogram of calibrated energy of all detectors:
```c++
FragmentTree->Draw("GetEnergy() >> h(16000,0,16000)", "", "")
```

Another example, of a 2D histogram that displays calibrated energy vs. detector number ignoring channel 12, is:
```c++
FragmentTree->Draw("ChannelNumber:GetEnergy() >> h(16000,0,16000,64,0,64)", "ChannelNumber!=12", "LEGO")
```

####The Autosort Feature (NOT IMPLEMENTED)
If the __-a__ or __-s__ flags are used on the MIDAS file, GRSISort will sort the MIDAS file and then complete a fragment sort by applying the flags and sorting the fragment tree.
