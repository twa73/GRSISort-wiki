<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**UTILITIES**](Utilities) > [**EXTRA MACROS**](Extra-Macros)
        
  * **html_generator.C** <br> Creates root-like HTML documentation for each of the defined classes in the script.
  * **odbgains.C** <br> Creates a bash script that we use to set the odb gains and offsets in the online analyzer. Running ODBGains("*nameofcalfile*.cal") outputs a file called odbgains.sh.
  * **bufferclean.C** <br> Cleans the extraneous data that is dumped into the GRIFFIN stream at the start of a run. This gets written to a new Midas File.
  * **offsetfind.C** <br> Finds the timestamp offset between the different GRIF-16 and 4G digitizer systems (must have buffer cleaned).
  * **offsetfix.C** <br> Finds the timestamp offset between the different GRIF-16 and 4G digitizers and then produces a new midas file with the fixed timestamps (must have buffer cleaned).
  * [**GainMatchGRIFFIN.C**](GainMatchGRIFFIN) <br> Gain-matches GRIFFIN given a set of peak values. Fairly flexible, and written to work with few (but not no) modifications.