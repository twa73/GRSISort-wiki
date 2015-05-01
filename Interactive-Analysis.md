<a name="top" />

[**HOME**](Home) > [**INTERACTIVE ANALYSIS**](Interactive-Analysis)

#The GROOT library and you.

In addition to the many libraries in TGRSIAnalysis to make analysis easier, GRSISort also takes control of some of the behind the scene functions of ROOT to make gamma-ray analysis a bit easier. 

To take advantage of these features all one has to do is start GRSISort!  They are implemented when examining and natural root classes, whether the where draw from a tree, made fresh or load from a file.
 
  * **GlobalFunctions** <br>
    * ProjectionX(TH2*,low=0,high=-1)  Helper function, only passing a TH2 returns the total Projection along the x-axis.  Additionally, using this function allows GROOT to remember which matrix the return total projection belongs too.  This allows one to use the 'p' command (shown in TH1 commands below) to gate along the returned axis.  If low/high arguments are int bin numbers are use, else axis values are used.
    * ProjectionY(TH2*,low=0,high=-1)  Helper function, only passing a TH2 returns the total Projection along the y-axis.  Additionally, using this function allows GROOT to remember which matrix the return total projection belongs too.  This allows one to use the 'p' command (shown in TH1 commands below) to gate along the returned axis.  If low/high arguments are int bin numbers are use, else axis values are used.

  * **GCanvas** <br>  The most notable difference, is the replacement of the TCanvas with GCanvas.  This replacement is done naturally - no changes from either existing ROOT scripts or GRSISort scripts are needed to take advantage of the GCanvas default behavior.  As of right now, the major changes are only for 1d histograms however, however global histogram management and 2d histogram commands are on the way.

## TH1 Commands
|  Key  | Action |
|:------|:------|
|        | All normal root commands/interactions.|
| **m**  | Toggle on marker mode; when on, the histogram will remember and display the last four clicks as marks on the histogram.|
| **p**  | If the 1d hist was made using the global ProjectionX/ProjectionY; gating the original 2D matrix this histogram came from is possible by placing markers around the gate and pressing p.  The gates spectra is immediately drawn. |
| **M**  | Toggle off marker mode.|
| **n**  | Remove all markers / functions drawn on the histogram.|
| **N**  | Remove all markers and the LAST function drawn on the histogram.|
| **e**  | Expand the x-axis range between the last two markers.|
| **E**  | Bring up dialogue box used to set desired x-axis range.|
| **o**  | Unzoom the entire histogram.|
| **Arrow Left/Right** |  When zoomed in, mover the display region to the left/right by one half of the region currently displayed.|
| **Arrow Up/Down** |  Quickly display the next histogram stored in memory, especially useful when gating to go back and forth between gates and the total projection.|
| **f**  | Ryan D's TPeak Fit (proper skewd gaus for gamma-rays with automatic bg) with minimum output. |  
| **F**  | Ryan D's TPeak Fit (proper skewd gaus for gamma-rays with automatic bg) with full output.   |
| **g**  | Simple Gaus fit between the last to marks, displays results of the fit **RESULTS STILL NEED TO BE VERIFIED** |
| **G**  | Gaus fit with linear background, displays results of the fit **RESULTS STILL NEED TO BE VERIFIED** |
| **S**  | Toggle histogram Stats on/off. |
| **l**  | Make y-axis linear. |
| **L**  | Make y-axis logarithmic. |
| **F10**  | Print any TPeaks currently displayed on the histogram to <histname>.fits. If file already exists, the file is destroyed and recreated. |


## TGraph/TGraphErrors Commands
|  Key  | Action |
|:------|:------|
|        | All normal root commands/interactions.|
| **p**  | Print the graph to the terminal.