[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort)

#The Sorting Model
The steps involved in using GRSISort are:
[[/images/GRSIFlow.png]]

1. [Unpacking the Midas File](#rununpack)
2. [Analyzing the Fragment Tree](#runfragtree)
3. [Histogramming the Final Results](#runhistos)

By default GRSIsort will open up a root session in the GRSI environment.
<a name="rununpack" />
#Unpacking the MIDAS File
The strategy used by GRSISort is to unpack the MIDAS file and convert the information into a ROOT Tree format. This tree contains <a href="https://rawgit.com/wiki/r3dunlop/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/TFragment.html#TFragment:Data_Members" target="_blank">TFragments</a> and is essentially all of the information from the MIDAS File with higher compression and accessibility.
  
**grsisort NAME_OF_MIDAS_FILE.mid**  -> Converts the midas file into a fragment tree

Currently accepted flags for the midas sort:
  * __-suppress_error__,   suppresses errors from failed data parsing appearing in stdout
  * __-log_error__,        sends errors from failed data parsing to file, will not send errors if supress error option is also used!
  * __-no_waveforms__,     does not add the wave form stored for each event in the mid file to the fragment tree
  * __-q__,                quits the grsi environment after completing the midas sort

<a name="runfragtree" />
**grsisort [-a -s] NAME_OF_FRAGMENT_TREE.root**   -> Starts sorting the fragment tree into (-s) user defined hists and/or (-a) analysis trees.
  * Histograms are defined in **users/UserInitObj.h**
  * How to fill the histogram is defined in **users/UserFillObj.h** 
  
If **-a** or **-s** are left off of the command, the fragment tree will be loaded into the grsi environment for the user to enjoy.

Currently accepted flags for the fragment tree sort:
  * __-a__,                creates an analysis tree to be used for doing a full analysis of the data
  * __-s__,                sorts the fragment tree into user defined hits. 
  * __-q__,                quits the grsi environment after completeing the fragment sort
  * __-no_speed__,         suppresses the output from the PROOF spedometer

If the __-a__ or __-s__ flags are used during the MIDAS sorting phase, GRSISort will complete sorting the MIDAS file and then complete an *autosort* by applying the flags and sorting the fragment tree.

