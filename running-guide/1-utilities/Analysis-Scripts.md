<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**UTILITIES**](Utilities) > [**ANALYSYS SCRIPTS**](Analysis-Scripts)
        
# Compiled

These are all automatically compiled from .cxx files in the util folder.

  * **AddToChannelNumber**
    This program might be outdated?
    Command line arguments:
    - int integer to add to each channel
    - .cal file to be updated
  * **AnalyzeComptonMatrices**
    This program is still in work, intended to be used to analyze matrices from the helper.
    Command line arguments:
    - `-h` `-?` `--help` to show help message
    - `--q-value` to provide the quality factor for the predicted asymmetry plot of `Q*P*Cos(2*x_i)/2`.
    - `--measured-q-energy` to provide the energy of the scattered gamma ray used to calculate the q value.
    - `--used-q-energy` to provide the energy of the scattered gamma ray currently being analyzed, used to scale the q value. 
    - `--input` to provide the path of the input file (**required**).
    - `--settings` to provide the path of the settings file.
    - `--output` to provide the name of the output file, default is “ComptonPolarimetry.root”.
  * **AnalyzeDataLoss**
    Potentially outdated program to analyze the data loss based on a fragment tree. Takes the file with the fragment tree as input, and optionally the name of the output file and the maximum number of entries to use.
  * **Compton_Scale_plot**
    Program prints out a list of energies ranging from 100 to 3000 keV in steps of 100 keV and the quality factor scaled to this energy. Currently neither the maximum energy, nor the step size, or the reference energy can be changed via command line, the used would have to change the code and re-compile.
  * **CrossTalkFix**
    Calculates cross talk correction factors from the output of the cross talk helper.
    Commmand line arguments:
    - matrix file (output of the cross talk helper)
    - .cal file
    - optional user settings file (.par extension)
    The user settings file can be used to specify the energy of the gamma ray to be used to calculate the cross talk correction factors, the minimum counts required for each slice of the matrix, the first and last detector, a list of detectors to be excluded, a channel prefix, and a channel postfix.
  * **EnableCrosstalkCorrection**
    Simple program that takes a list of root-files as input, and for each tries to read the analysis options and change the flag for cross talk correction to `true`. At the end it will print out a list of files that could not be opened or that had no analysis options.
  * **GainDrift**
    Program to determine the gain drift and create new .cal-files with the gain drift correction. It requires input files with matrices of energy vs. channel that all have the same calibration applied to them.
    Command line arguments:
    | Flag | Description | Comments |
    |:----|:---:|:---:|
    -if  |input files|                                (required)
    -sf  |settings file|                              (semi-optional, TUserSettings format identifying the peaks to be used)
    -el  |list of energies to be used|                (semi-optional, either this or a settings file need to be used)
    -ul  |list of energy uncertainties to be used|    (optional, default = empty)
    -d   |degree of fit|                              (optional, default = 1, i.e. linear)
    -r   |+-range of fit|                             (optional, default = 10.)
    -mw  |minimum FWHM for good fit|                  (optional, default = 2.)
    -hn  |histogram name|                             (optional, default = "EnergyVsChannel")
    -pre |prefix|                                     (optional, default = "GainDrift")
    -eb  |list of bins to be excluded|                (optional, but can't be used together with "-ib" flag)
    -ib  |list of bins to be included|                (optional, but can't be used together with "-eb" flag)

  * **PlotVsRun**
    Program to plot histograms versus the run number. Useful to get a quick overview of the stability of the system, e.g. to check for gain drifts, changes in timing conditions. If the histograms in question are 1D histograms, they will simply plotted in a 2D matrix against the run number. If the histograms however are 2D histograms, the default behaviour is to create a 2D matrix for each x-axis bin of the input matrices. This behaviour can be changed using the `-x` or `-y` flag which will project the input matrices onto the x- or y-axis and plot the result versus the run number.
    Command line arguments:
    -if list of input files
    -hn list of histogram names
    optional:
    -x or -y for projection on x- or y-axis, otherwise 2D-histograms will get split along x-axis
  * **ReadCalFromRoot**
    Simple program that takes a list of root-files, and writes out a .cal file for each using the run number and sub run number as a name. Prints a list of all files that couldn’t be opened and a list of all files that failed to contain a “Channel” and “RunInfo” object.
  * **Root2Ascii**
    Simple program that loops over all keys in a root file, writing all recognized classes into ASCII files inside a directory of the same name as the root file (minus the .root extension). The ASCII files are named after the objects plus a .dat file extension. Optional command line arguments (after the input .root file) are a list of names for all the object to write (if this list is not provided, any object matching one of the recognized class types is written), and an output format (with a leading `-`). The output format options are:
    - `e` to write a column with the error,
    - `z` to write zeroes,
    - `v` to write values only (writes only the bin content, but not x, y, or z),
    - `p` to write in the pm3d format (gnuplot style for drawing palette-mapped 3D or 4D data),
    - `g` to write in the xyerrorbars format of gnuplot, and
    - `f` to write in the fresco input format.
  The classes the program currently can write are any `TH1`, `TH2`, `TSpline`, `TGraph`, `TGraphErrors`, `TGraphAsymmErrors`, `GHSymF`, and `GHSymD`.
  Histograms (`TH1`, `TH2`, `GHSymF`, or `GHSymD`) respect the `e`, `z`, `v`, and `p` format flags. Selecting the pm3d format will cause errors to be printed for any histogram that is not a `TH2`, or if combined with the `e` or `v` format flag.
  Graphs (`TGraph`, `TGraphErrors`, or `TGraphAsymmErrors`) respect the `e`, `z`, `g`, and `f` format flags.
  Splines (`TSpline`) are always printed the same way (x column and y column).
  * **Root2Rad** <br> Accepts a .root file and converts all of the contained TH1 histograms to RadWare .spe files and the TH2 histograms to RadWare .mat files. Can process 8k matrices, split large matrices, and/or compress large matrices. Useful for professors.
  * **ScaleCalibration**
    Takes a number to scale calibrations by, a .cal file, and optionally a mnemonic. Writes a new .cal file with “out” as a prefix where all parameters of the energy calibration are multiplied by the number provided.
  * **SetOdbFromData**
    Simple program that takes a list of root files, checks if the information in the “PPG” object matches the information from the ODB, sets the ODB information from the data if necessary, and then writes the updated TPPG to the file. Note: Older versions of this code tried to read the “TPPG” object, even after we changed the name of the `TPPG` object written by GRSIsort, so if this program fails for you, check if your GRSISort version is one where the name wasn’t yet changed.
  * **WriteCalToRoot**
    Simple program to write the information from a .cal file to several root files at once. Takes 3 or more arguments, first a flag “update” or “remove” that indicates whether to update the existing information for each channel, or to remove the old information before writing the new information. The second argument is the .cal file to use, and all remaining arguments are the root files to update. This really should only be used with the fragment or analysis files, it makes no sense (and is actually confusing) to change the calibration of a file with histograms (like the output file of a helper).
  * **WriteNonlinearitiesCal**
    Reads energy nonlinearities from a `TGraph` (name is the second command line argument) in a root file (path is the first command line argument), and writes a cal file with only those nonlinearities (full name of that cal file is the third command line argument).
  * **checkSort**
    Program takes a list of analysis root files and checks if they suffer from sorting problems that occurred in some versions of GRSISort (when we used multiple threads to write the analysis root file). It does this by checking (for each branch) if all three of the last hits in this detector type have the same timestamp and energy in which case we have a bad sort with spikes in the energy and time spectra (because apparently a single hit got written thousands of times to the output analyst file). Should not be needed anymore with current versions of GRSISort?
  * **gadd**
    GRSISort’s version of the hadd program from ROOT. Mainly a straight copy of that program (probably a bit out of date by now), with some small adjustments like dealing with the `TChannel` calibration.
  * **grsiframe**
    Program based on ROOT’s RDataFrame class. It is the replacement of grsiproof which was based on ROOT’s now deprecated PROOF. See [here](GRSIFrame) for more information.

# Scripts

These can either be found in the util folder (.C files), or as normal executable commands in the bin directory (ErrorReport\.sh and grsi-config).

  * **autoeffic.C** <br> Reads in a root file and calculates the efficiency of each HPGe Crystal based on a specific source and activity. Currently also gain matches, etc. This script might be out of date by now (June 2022).
  * **ErrorReport\.sh**
    Bash script that prints some basic information about the users system (type and version of OS, ROOT version used, current GRSISort commit, etc.). This output can (and should) be used to start any issue reported on GitHub.
  * **grsi-config**
    Bash script that can be used to get compiler flags needed to compile code against the GRSISort framework. Inspired by (and copied from) the root-config script of ROOT.
