#GRSISort

A lean, mean, sorting machine. 


## Quick navigation

| Setup Guide             |  How to Run          | Technical Documentation                  |
|----------------------------|-------------------------------|---------------------------|
| \centerline{\includegraphics[height=256px]{/images/tools.png}} | [[/images/running.png]] | [[/images/blueprint.png =256x]] |
| [[Setup Guide|Setting-up-GRSISort]] | [[Running GRSISort|broken]] | [[Technical Documentation|broken]]|
| A step-by-step guide to install GRSISort  | Instructions for running GRSISort | Detailed technical documentation on GRSISort |

    


-----------------------------------------
Running
-----------------------------------------
grsisort will open up a root session in the grsi environment
Commands:

**grsisort NAMEOFMIDASFILE.mid**  -> Converts the midas file into a fragment tree

Currently accepted flags for the midas sort:
  * __-suppress_error__,   suppresses errors from failed data parsing appearing in stdout
  * __-log_error__, sends errors from failed data parsing to file, will not send errors if supress error option is also used!
  * __-no_waveforms__,     does not add the wave form stored for each event in the mid file to the fragment tree
  * __-s__,                quits the program after it finishs turing the input midas files into trees.

**grsisort -s NAME_OF_FRAGMENT_TREE.root**   -> Starts sorting the fragment tree into user defined hists.
  * Histograms are defined in **users/UserInitObj.h**
  * How to fill the histogram is defined in **usrer/UserFillObj.h** 
