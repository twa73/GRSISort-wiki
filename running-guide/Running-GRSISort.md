[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort)

<a name="sortingmodel" />

# The Sorting Model

The steps involved in using GRSISort are:
[[/images/GRSIFlow.png]]

We are running a multistage sorting model with GRSISort.
This has one main advantage: It allows us to sort the very large MIDAS files non-locally and then download the heavily compressed histograms or analysis tree data.

1. [Unpacking the Midas File](#unpackmid)
2. [Analyzing the Fragment Tree](#sortfrags)
3. [Histogramming the Final Results](#runhistos)

By default GRSIsort will open up a root session in the GRSI environment.

<a name="unpackmid" />
# Unpacking the MIDAS File

The strategy used by GRSISort is to unpack the MIDAS file and convert the information into two ROOT Trees.
The first tree in the ```fragment?????_???.root``` file contains <a href="https://rawgit.com/wiki/r3dunlop/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/TFragment.html# TFragment:Data_Members" target="_blank">TFragments</a> and is essentially all of the information from the MIDAS File with higher compression and accessibility.
The second tree in the ```analysis?????_???.root``` file contains built events with detector information (essentially the same fragments, but in groups and translated into detectors).
  
**grsisort --recommended NAME_OF_MIDAS_FILE.mid**  -> Converts the midas file into a fragment tree, and an analysis tree.

The ```--recommended``` flag includes a number of default GRSISort flags (-a, -q, -l, --write-diagnostics, --word-count-offset=-1, --write-fragment-tree, --write-bad-fragments, --separate-out-of-order, --suppress-error, and --reconstruct-timestamp).
These and all other available flags can be seen using ```grsisort -h```.
This shows all the recommended flags in dark green, and the flags used in creating the analysis tree and making histograms in dark cyan.

Some currently accepted flags for the midas sort:
  * __--suppress_error__,  suppresses errors from failed data parsing appearing in stdout
  * __--log_errors__,      sends errors from failed data parsing to file, will not send errors if supress error option is also used!
  * __-q__,                quits the grsi environment after completing the midas sort (important when sorting a file)
  * __-b__,                run in batch mode (useful when running a ```<macro>.C`` script on the sorted file

Only one midas file can be sorted at a time.

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

## Viewing histograms within GRSISort
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
FragmentTree->Draw("fChannelNumber:GetEnergy() >> h(16000,0,16000,64,0,64)", "fChannelNumber!=12", "colz")
```

This is mainly useful for a first look at the data, to produce multiple histograms at once for many root files use [[GRSIProof | GRSIProof]].

#### The Autosort Feature 
If the __-a__ flag is used on the MIDAS file, GRSISort will sort the MIDAS file and complete a fragment sort by applying the flags and sorting the fragment tree, making an analysis tree file.
