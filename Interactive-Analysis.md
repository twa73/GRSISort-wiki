<a name="top" />

[**HOME**](Home) > [**COMMAND LINE ANALYSIS**](Command-Line-Analysis)

#The GROOT library and you.

In addition to the many libraries in TGRSIAnalysis to make analysis easier, GRSISort also takes control of some of the behind the scene functions of ROOT to make gamma-ray analysis a bit easier. 

To take advantage of these features all one has to do is start GRSISort!  They are implemented when examining and natural root classes, whether the where draw from a tree, made fresh or load from a file.

  * **GCanvas** <br>  The most notable difference, is the replacement of the TCanvas with GCanvas.  This replacement is done naturally - no changes from either existing ROOT scripts or GRSISort scripts are needed to take advantage of the GCanvas default behavior.  As of right now, the major changes are only for 1d histograms however, however global histogram management and 2d histogram commands are on there way.

## TH1 Commands
  |  Key  | Action |
  |:------|:------:|
  |   test    | All normal root commands/interactions.|
  | **m**  | Toggle on marker mode; when on, the histogram will remember and display the last four clicks as marks on the histogram.|
  | **M**  | Toggle off marker mode.|
  | **n**  | Remove all markers / functions drawn on the histogram.|
  | **e**  | Expand the region between the last two markers.|
  | **o**  | Unzoom the entire histogram.|
  | **Arrow Left/Arrow Right** |  When zoomed in, mover the display region to the left/right by one half of the region currently displayed.|
  | **f**  | Ryan D's TPeak Fit (proper skewd gaus for gamma-rays with automatic bg) with minimum output. |  
  | **F**  | Ryan D's TPeak Fit (proper skewd gaus for gamma-rays with automatic bg) with full output.   |
  | **g**  | Simple Gaus fit between the last to marks, displays results of the fit **RESULTS STILL NEED TO BE VERIFIED** |
  | **G**  | Gaus fit with linear background, displays results of the fit **RESULTS STILL NEED TO BE VERIFIED** |
  | **S**  | Toggle histogram Stats on/off. |

