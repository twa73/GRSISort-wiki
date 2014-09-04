<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort)) > [Utilities](Utilities)

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
Analysis Macros
-----------------------------------------
  * **auto_eff.C** <br>Reads in a root file and calculates the efficiency of each HPGe Crystal based on a specific source and activity

-----------------------------------------
Utility Macros
-----------------------------------------
  * **html_generator.C** <br> Creates root-like HTML documentation for each of the defined classes in the script.