<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**UTILITIES**](Utilities)

GRSISort contains many different utilities stored in the **util** directory. These utilities include:

1. [Analysis Scripts](Analysis-Scripts)


-----------------------------------------
Compiled Utilities
-----------------------------------------
Utilities, such as analysis scripts, asre kept in the **util** directory. Below are a list of useful utilities and how to use them.
  * **Root2Rad**, Converts the 1D and 2D histograms in a root file to Radware .spe and .mat formats
  To compile:
```
   g++ Root2Rad.cxx -oRoot2Rad `root-config --cflags --libs`
```
  To Run:
```
   ./Root2Rad NAME_OF_ROOT_FILE.root
```
        
-----------------------------------------
Utility Macros
-----------------------------------------
  * **html_generator.C** <br> Creates root-like HTML documentation for each of the defined classes in the script.