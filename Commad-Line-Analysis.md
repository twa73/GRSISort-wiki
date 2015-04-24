<a name="top" />

[**HOME**](Home) > [**COMMAND LINE ANALYSIS**](Command-Line-Analysis)

#The GROOT library and you.

In addition to the many libraries in TGRSIAnalysis to make analysis easier, GRSISort also takes control of some of the behind the scene functions of ROOT to make gamma-ray analysis a bit easier. 

To take advantage of these features all one has to do is start GRSISort!  They are implemented when examining and natural root classes, whether the where draw from a tree, made fresh or load from a file.

  * **GCanvas** <br>  The most notable difference, is the replacement of the TCanvas with GCanvas.  This replacement is done naturally - no changes from either existing ROOT scripts or GRSISort scripts are needed to take advantage of the GCanvas default behavior.  AS of right now, the major changes are only for 1d histograms however, however global histogram management and 2d histogram command are on there way.

* **TH1 Commands**
--* All normal root commands/interactions.
--* All normal root commands/interactions.