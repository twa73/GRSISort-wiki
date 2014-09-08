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
  
**grsisort NAMEOFMIDASFILE.mid**  -> Converts the midas file into a fragment tree

Currently accepted flags for the midas sort:
  * __-suppress_error__,   suppresses errors from failed data parsing appearing in stdout
  * __-log_error__, sends errors from failed data parsing to file, will not send errors if supress error option is also used!
  * __-no_waveforms__,     does not add the wave form stored for each event in the mid file to the fragment tree
  * __-s__,                quits the program after it finishs turing the input midas files into trees.

<a name="runfragtree" />
**grsisort -s NAME_OF_FRAGMENT_TREE.root**   -> Starts sorting the fragment tree into user defined hists.
  * Histograms are defined in **users/UserInitObj.h**
  * How to fill the histogram is defined in **users/UserFillObj.h** 


