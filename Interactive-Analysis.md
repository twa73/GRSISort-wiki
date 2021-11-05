<a name="top" />

[**HOME**](Home) > [**INTERACTIVE ANALYSIS**](Interactive-Analysis)

# The GROOT library and you.

In addition to the many libraries in TGRSIAnalysis to make analysis easier, GRSISort also takes control of some of the behind the scene functions of ROOT to make gamma-ray analysis a bit easier. 

To take advantage of these features all one has to do is start GRSISort!  They are implemented when examining and natural root classes, whether the where draw from a tree, made fresh or load from a file.
 
  * **GlobalFunctions** <br>
    * **ProjectionX(TH2*,low=0,high=-1)**  Helper function, only passing a TH2 returns the total Projection along the x-axis.  Additionally, using this function allows GROOT to remember which matrix the return total projection belongs too.  This allows one to use the 'p' command (shown in TH1 commands below) to gate along the returned axis.  If low/high arguments are int bin numbers are use, else axis values are used.
    * **ProjectionY(TH2*,low=0,high=-1)**  Helper function, only passing a TH2 returns the total Projection along the y-axis.  Additionally, using this function allows GROOT to remember which matrix the return total projection belongs too.  This allows one to use the 'p' command (shown in TH1 commands below) to gate along the returned axis.  If low/high arguments are int bin numbers are use, else axis values are used.
    * **SaveAll(cons char *Filename, Option_t *opt="recreate")** Open a file with the filename and option given.  Save all objects currently managed by GRootObjectManager.  After saving, returns to the original working TDirectory.
    
  * **GCanvas** <br>  The most notable difference, is the replacement of the TCanvas with GCanvas.  This replacement is done naturally - no changes from either existing ROOT scripts or GRSISort scripts are needed to take advantage of the GCanvas default behavior.  As of right now, the major changes are only for 1d histograms however, however global histogram management and 2d histogram commands are on the way.

## General Commands

|   Key   | Action |
|:--------|:--------|
| **F2**  | Show editor. |
| **F9**  | Toggle crosshairs. |

## TH1 Commands
|   Key   | Action |
|:--------|:--------|
|         | All normal root commands/interactions.|
| **b**   | Set the background, how it is set depends on **B**.|
| **B**   | Cycle through types of automatic background subtraction used when projecting with **p**.  Current types include: No subtraction, Fraction of the total, subtract gate from the 3rd marker (gate size set to the distance between marker 1 and 2). |
| **d**   | Open popup. |
| **e**   | Expand the x-axis range between the last two markers.|
| **E**   | Bring up dialogue box used to set desired x-axis range.|
| **f**   | If markers have been set, do a GPeak fit between last two markers on the last histogram (skewed gaus for gamma-rays with automatic bg). |  
| **F**   | TPeak Fit (skewed gaus for gamma-rays with automatic bg).   |
| **g**   | Simple Gaus fit between the last to marks, displays results of the fit **RESULTS STILL NEED TO BE VERIFIED** |
| **i**   | Raw integral of counts between the two markers |
| **I**   | TODO: Background subtracted integral of counts between the two markers |
| **l/y** | Toggle y-axis from linear to logarithmic and vice versa. |
| **m**   | Toggle on marker mode; when on, the histogram will remember and display the last four clicks as marks on the histogram.|
| **M**   | Toggle off marker mode.|
| **n**   | Remove all markers / functions drawn on the histogram.|
| **N**   | Remove all markers and the LAST function drawn on the histogram.|
| **o**   | Unzoom the entire histogram.|
| **p**   | If the 1d hist was made using the global ProjectionX/ProjectionY; gating the original 2D matrix this histogram came from is possible by placing markers around the gate and pressing p.  The gates spectra is immediately drawn. |
| **P**   | Draws parent histogram??? |
| **q**   | If markers have been set, fit a GPeak between the last two markers on the first histogram (skewed gaus for gamma-rays with automatic bg). |
| **r**   | Expand the y-axis range between the last two markers.|
| **R**   | Bring up the dialogue box used to set the desired y-axis range.|
| **s**   | Show peak values. |
| **S**   | Remove peak values. |
| **Arrow Left/Right** |  When zoomed in, mover the display region to the left/right by one half of the region currently displayed.|
| **Arrow Up/Down** |  Quickly display the next histogram stored in memory, especially useful when gating to go back and forth between gates and the total projection.|
| **F9**  | Increments line color of first one dimensional histogram found by one (colors > 10 are reset to 1). |
| **F10** | TODO: Print any TPeaks currently displayed on the histogram to <histname>.fits. If file already exists, the file is destroyed and recreated. |

## TH2 Commands
|   Key   | Action |
|:--------|:--------|
| **c**   | Add the initialized cut to the list of cuts (see **i**).|
| **e**   | Expand the x- and y-axis between the last two markers.|
| **E**   | Bring up dialogue box used to set desired x-axis range.|
| **g**   | Create a cut from the last two markers and add it to the histogram.|
| **i**   | Initialize a new cut.|
| **l/z** | Toggle z-axis from linear to logarithmic and vice versa. |
| **n**   | Remove all markers / functions drawn on the histogram.|
| **o**   | Unzoom the entire histogram.|
| **P**   | Get projections from this histogram and draw the first one if it exists.|
| **r**   | Expand the y-axis between the last two markers.|
| **R**   | Bring up the dialogue box to set the desired y-axis range.|
| **s**   | Save the cuts that have been created.|
| **x**   | Create projection of the histogram onto the x-axis and draw in a new canvas.|
| **X**   | Create "y summary" of the histogram, i.e. a projection of the first y-bin onto the x-axis that yields a non-empty histogram and draw it on a new canvas.|
| **y**   | Create projection of the histogram onto the x-axis and draw in a new canvas.|
| **Y**   | Create "x summary" of the histogram, i.e. a projection of the first x-bin onto the y-axis that yields a non-empty histogram and draw it on a new canvas.|
| **Arrow Left/Right** |  When zoomed in, mover the display region to the left/right by one half of the region currently displayed.|
| **Arrow Up/Down** |  Quickly display the next histogram stored in memory, especially useful when gating to go back and forth between gates and the total projection.|

## TGraph/TGraphErrors Commands
|   Key   | Action |
|:--------|:--------|
|         | All normal root commands/interactions.|
| **p**   | TODO: Print the graph to the terminal.
