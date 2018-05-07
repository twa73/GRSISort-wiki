[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort)

<a name="sortingmodel" />
# The Sorting Model
The steps involved in using GRSISort are:
[[/images/GRSIFlow.png]]

We are running a multistage sorting model with GRSISort. This has one main advantage: It allows us to sort the very large MIDAS files non-locally and then download the heavily compressed fragment tree data.

1. [Unpacking the Midas File](#unpackmid)
2. [Analyzing the Fragment Tree](#sortfrags)
3. [Histogramming the Final Results](#runhistos)

By default GRSIsort will open up a root session in the GRSI environment.
<a name="unpackmid" />
# Unpacking the MIDAS File
The strategy used by GRSISort is to unpack the MIDAS file and convert the information into a ROOT Tree format. This tree contains <a href="https://rawgit.com/wiki/r3dunlop/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/TFragment.html# TFragment:Data_Members" target="_blank">TFragments</a> and is essentially all of the information from the MIDAS File with higher compression and accessibility.
  
**grsisort NAME_OF_MIDAS_FILE.mid**  -> Converts the midas file into a fragment tree

Some currently accepted flags for the midas sort:
  * __--suppress_error__,  suppresses errors from failed data parsing appearing in stdout
  * __--log_errors__,      sends errors from failed data parsing to file, will not send errors if supress error option is also used!
  * __-q__,                quits the grsi environment after completing the midas sort (important when sorting a file)
  * __-b__,                run in batch mode (useful when running a ```<macro>.C`` script on the sorted file

Only one midas file can be sorted at a time.
<a name="sortfrags" />
# Sorting the Fragment Tree
**grsisort [-a -H] NAME_OF_FRAGMENT_TREE.root**   -> Starts sorting the fragment tree into (-s) user defined hists and/or (-a) analysis trees.
  * Histograms are defined in **users/UserInitObj.h**
  * How to fill the histogram is defined in **users/UserFillObj.h** 
  
If **-a** or **-H** are left off of the command, the fragment tree will be loaded into the grsi environment for the user to enjoy.

Some currently accepted flags for the fragment tree sort:
  * __-a__,                creates an analysis tree to be used for doing a full analysis of the data
  * __-H__,                sorts the fragment tree into user defined hists 
  * __-q__,                quits the grsi environment after completeing the fragment sort

For more information about available flags run ```grsisort -h```.
## Calibrating the gamma spectra
When sorting the fragment tree, you can read in a [calibration file](CalFiles) at the same time and it will perform the calibrations while sorting. This can be done with the command:
```c++
grsisort -flags <midasfile>.mid <calibrationfile>.cal
```
If a different calibration is supposed to be used it can be either loaded temporarily by giving the corresponding .cal file after the fragment/analysis tree file, or it can be written permanently to the files by using the **WriteCalToRoot** program which can be used to update or replace the calibration.
It is possible to use multiple calibration files, in which case for duplicate entries the value will be taken from last file provided. This can be used e.g. to provide energy calibrations and cross-talk corrections is separate files.

## Displaying parameters of fragment tree
You can list the parameters included in the fragment tree with:
```c++
FragmentTree->Print()
```

Alternatively, you view them in a graphical interface with:
```c++
FragmentTree->StartViewer()
```

## Viewing histograms within `grsisort`
After sorting the data and creating a fragment tree, you can create histograms by running normal ROOT commands:
```c++
FragmentTree->Draw("<variable> >> <histogramName>(<xbins>,<xmin>,<xmax>)", "<cuts>", "<displayOptions>")
```

Here is an example that displays a 1D histogram of calibrated energy of all detectors:
```c++
FragmentTree->Draw("GetEnergy() >> h(16000,0,16000)", "", "")
```

Another example, of a 2D histogram that displays calibrated energy vs. detector number ignoring channel 12, is:
```c++
FragmentTree->Draw("fChannelNumber:GetEnergy() >> h(16000,0,16000,64,0,64)", "fChannelNumber!=12", "LEGO")
```

#### The Autosort Feature 
If the __-a__ flag is used on the MIDAS file, GRSISort will sort the MIDAS file and complete a fragment sort by applying the flags and sorting the fragment tree, making an analysis tree file.
