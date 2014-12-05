<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**UTILITIES**](Utilities) > [**ANALYSYS SCRIPTS**](Analysis-Scripts)
        
#Compiled
  * **Root2Rad** <br> Accepts a .root file and converts all of the contained TH1 histograms to RadWare .spe files and the TH2 histograms to RadWare .mat files. Useful for professors.
  * **MakeMatrices** <br> Currently the go-to code. This takes an analysis tree and creates many useful coincidence matrices and spectra. It is compiled from `MakeMatrices.C`. If you are looking for an example of how to create an analysis macro, start here.
  * **MakeTimeDiffSpec** <br> Run on a Fragment tree to get time difference spectra as well as other diagnostic histograms. 

#Scripts
  * **auto_effic.C** <br>Reads in a root file and calculates the efficiency of each HPGe Crystal based on a specific source and activity. Currently also gain matches, etc. This script is changing frequently.