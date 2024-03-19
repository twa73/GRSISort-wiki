<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**UTILITIES**](Utilities)

GRSISort contains many different utilities stored in the **util** directory. These utilities include:

1. [Analysis Scripts](Analysis-Scripts)
2. [[Extra Macros|Extra-Macros]]

-----------------------------------------
Compiled Utilities
-----------------------------------------
The utilities are automatically compiled when `make` is run in the `$GRSISYS` top-level directory. The compiled binaries are stored in the **bin** directory and can be called from anywhere.

-----------------------------------------
List of Utilities
-----------------------------------------
Utilities, such as analysis scripts, are kept in the **util** directory.
Below is an **incomplete** list of useful utilities and how to use them.
The compiled utilities are listed as the binary name (i.e. without the ".cxx" extension).
  * **Root2Rad** this program converts all histograms (1D and 2D) into radware-type histograms and matrices and put's them in a new folder with the name of the root-file they come from.
  * **WriteCalToRoot** this programs takes a cal-file and either updates or replaces the calibration of all provided root-files with it.
  * **ReadCalFromRoot** this programs takes root-files and writes calibration files (one for each root-file provided).
  * **html_generator.C** this script generates the old html documentation of the classes and functions in GRSISort (deprecated, use doxygen instead).
  * **gadd** this program is equivalent to ROOT's hadd (with added knowledge about some special GRSISort classes) and can be used to add histogram root-files together.
  * **PlotVsRun** this program allows plotting histograms from multiple files as 2D histograms with the original information vs. the run number. This can be useful e.g. to check if the calibration is stable over the whole experiment. 2D histograms can either be projected onto one axis (plotting that projection vs. the run number), or split into multiple 2D histograms (one each for each bin of the x-axis).

        
