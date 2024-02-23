<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**GRSIFrame**](GRSIFrame)

GRSIFrame is used to run an analysis script on fragment trees, analysis trees, or other trees.
It uses the RDataFrane system to distribute the sorting of the tree over multiple threads (slots) before adding the results back together.

1. [Creating your own helper](GRSIFrame#create_helper)
2. [Creating Histograms](GRSIFrame#create_histos)
3. [Filling Histograms](GRSIFrame#fill_histos)
4. [EndOfSort](GRSIFrame#end_of_sort)
5. [Running GRSIFrame](GRSIFrame#running)
6. [Running Options](GRSIFrame#running_options)
7. [Creating a helper from an existing GRSIProof selector](GRSIFrame#change_selector)

-----------------------------------------
# How To Use
-----------------------------------------
The grsiframe program has been created to facilitate an easy to use interface between GRSISort and RDataFrame.
grsiframe itself
- takes care of creating a stopwatch to time itself,
- loads the proper settings from .grsirc and the command line arguments,
- loads the correct parser library, and
- creates and runs an instance of TGRSIFrame.

TGRSIFrame is a class that 
- reads the PPG, calibration, and run info of each file, and adds the file to a TChain,
- adds those as well as all command line cal-files, gValue files, and cut files to the input list of the helper,
- creates that helper and get's the output prefix from it, and finally
- set's the file output from that prefix and the run info, creates a progress bar, get's the ouput of the helper (which actually runs it), and writes the results to the output file.

The helper inherits from the TGRSIHelper class which provides
- maps to store 1D-, 2D-, or 3D-ROOT histograms, 2D-, or 3D-GRSISort histograms, or trees,
- access to all the input from the input list provided by TGRSIFrame, 
- a method that merges the output histograms and/or trees together, and
- a function to check the sizes of all objects (to ensure the maximum object size of 1 GB in ROOT).

The helper also provides the user code that creates and fills the histograms and/or trees.
This is done via the CreateHistograms and Exec functions, respectively.
While Exec will be called in parallel and thus has to be thread-safe, CreateHistograms will be called in a loop over all slots/workers and does not have to be threadsafe.

## Creating your own helper <a name="create_helper" />
There are examples of helpers in the `example` directory.
We will use the event helper (`ExampleEventHelper`) as an example here, everything is analogous for fragment files.

The first thing that must be done is to change the class name.
This must be done in both the `.cxx` and `.hh` files.
This should match the name of the files themselves!
This also includes the namespace in front of every function. For example, 
```c++
void ExampleEventHelper::CreateHistograms(unsigned int slot)
```
Must become:
```c++
void MyAwesomelyNamedHelper::CreateHistograms(unsigned int slot)
```
These **must be changed** in both the `MyAwesomelyNamedHelper.hh` and the `MyAwesomelyNamedHelper.cxx`.
You must ensure include files and `ifdef`s are correctly named as well.
### Setting up detector classes
The detector systems you plan on analyzing using this helper must also be included in the `.hh` file. For example, in `ExampleEventHelper`, there are lines of code with:
```c++
#include "TGriffin.h"
#include "TGriffinBgo.h"
#include "TZeroDegree.h"
```
The code also needs to be told what classes and branches are used in the `Book` function:
```c++
   ROOT::RDF::RResultPtr<TList> Book(ROOT::RDataFrame* d) override {
      // TODO: edit the template specification and branch names to match the detectors you want to use!
      return d->Book<TGriffin, TGriffinBgo, TZeroDegree>(std::move(*this), {"TGriffin", "TGriffinBgo", "TZeroDegree"});
   }
``` 
and the signature of the `Exec` function::
```c++
void Exec(unsigned int slot, TGriffin& grif, TGriffinBgo& grifBgo, TZeroDegree& zds);
``` 

### Change Output file name
This simple change can be done in the header file in the constructor of the selector.
For example, the prefix of the file is changed by changing
```c++
Prefix("ExampleEvent");
```
to:
```c++
Prefix("AwesomeSauce");
```
For an analysis tree `analysis12345_678.root`, this would create an output root file called `AwesomeSauce12345_678.root`.
You can put whatever you want for the prefix, it does not have to be related to the class name but it can help later on. 

## Creating Histograms <a name="create_histos" />
The histograms are defined in the `.cxx` file in the function `CreateHistograms` (in our case `MyAwesomeHelper::CreateHistograms(unsigned int slot)`).
We have setup maps that allow for easy access of histograms across various locations in the selector.
This means to create a 1D histogram, one should use `fH1`, a 2D histogram in `fH2` etc.
If the histogram type you want does not exist, please place a request on the issue tracker. As an example, the line:
```c++
fH1[slot]["griffinE"] = new TH1F("griffinE", Form("Unsuppressed griffin energy;energy [keV];counts/%.1f keV", (highEnergy-lowEnergy)/energyBins), energyBins, lowEnergy, highEnergy);
```
creates a 1D histogram with `float` precision called `"griffinE"` and puts it in a map position of `"griffinE"` for the slot/worker `slot`.

## Filling Histograms <a name="fill_histos" />
To fill the histograms you just created, you can change the function located in the `.cxx` file named `Exec` (in our case `MyAwesomeHelper::Exec(unsigned int slot, TGriffin& grif, TGriffinBgo& grifBgo, TZeroDegree& zds)`).
You use the map to lookup the histogram you want to fill, and then fill it like a normal histogram.
For example, to fill the 1D energy spectrum created above, we would loop over all `Griffin` hits, find the energy of the hit, and put it in our `"griffinE"` histogram.
```c++
   for(int g = 0; g < grif.GetMultiplicity(); ++g) {
      auto grif1 = grif.GetGriffinHit(g);
      fH1[slot].at("griffinE")->Fill(grif1->GetEnergy());
   }
```
Note the use of ```.at()``` instead of ```[]``` here.
This is because the ```.at()``` function creates an exception when you try to access a name that is not in the corresponding map (either because the name is mispelled or because it's the wrong map).
In this case the selector will print out a corresponding message including all available names in that map in the .log file.
This helps track down any of these mistakes.

## EndOfSort <a name="end_of_sort" />
The EndOfSort function can be used to manipulated histograms after they have been filled.
By adding
```c++
void EndOfSort() override;
```
to the selectors header file and 
```c++
void <SelectorClassName>::EndOfSort() {
<code to manipulate histograms>
}
```
to the selectors source file one can add code to create a background subtracted spectrum from a coincident and a time-random histogram, set bins to specific values, etc.
When setting bins to non-zero values, keep in mind that this happens before the histograms from the different slaves are merged, so the final value in the histogram will multiplied by the number of workers.

## Running GRSIFrame <a name="running" />
You run `grsiframe` as a separate program from GRSISort.
The basic way to do so is:
```bash
grsiframe root_file_to_sort.root location_to_selector.cxx
```
for example:
```bash
grsiframe analysis12345_678.root $GRSISYS/examples/ExampleEventSelector.cxx
```
A list of analysis files can be used and they will all be chained and sorted at the same time.
This includes regular expressions.
The output file name will be named using the combined run info of all files.
For a single file the format will be `prefix``run number`_`sub run number`.root, for multiple (consecutive) sub runs of a single run it will be `prefix``run number`_`first sub run number`-`last sub run number`.root, and for multiple consecutive runs it will be `prefix``first run number`-`last run number`.root.
If the sub runs or runs are not consecutive, the output file name will be `prefix``first run number`_-001.root.
Consecutive means that not all sub runs have to be present, but if a subrun `n` is present all subruns from 0 to `n-1` have to be present, i.e. the last subruns can be missing (since the program has no way of knowing how many sub runs there should be for a given run).

## Running Options <a name="running_options" />
There are options that can be provided on the command line to change the way `grsiframe` runs.
Some options are listed below with what they do.
A full list can be viewed with `grsiframe -h` or `grsiframe --help`.

  * __--max-workers  arg__,          Maximum number of nodes to use when running a grsiframe session, 0 means no multithreading enabled, whereas 1 means multithreading enable with a single thread.
  * __--tree-name  arg__,            Name of tree to be proofed, default is empty, i.e. FragmentTree, AnalysisTree, and Lst2RootTree are checked

The analysis options that are used by grsiframe are:

  * __--addback-window  arg__,       Addback window, time in ns
  * __--suppression-window  arg__,   BGO suppression window, time in ns
  * __--suppression-energy  arg__,   Minimum BGO energy for suppression
  * __--is-correcting-cross-talk__,  Correct cross-talk

## Creating a helper from an existing selector <a name="change_selector" />
These are the steps to take to convert an existing GRSIProof selector into a grsiframe helper:
optional:
- Replace "Selector" with "Helper" (:%s/Selector/Helper/g) in both files.
- Remove unnecessary include statements in header-file.
mandatory:
Header file:
- Remove members that are pointers to detectors (like ```TGriffin* fGrif;```).
- Remove ClassDef line.
- Remove destructor.
- Remove ```Version()``` member function.
- Remove ```InitializeBranches()``` declaration and implementation.
- Maybe change include from TGRSISelector.h to TGRSIHelper.h (if not already done by a general replace, see above)
- Change constructor to take ```TList* list``` as argument, pass ```list``` to TGRSIHelper constructor, remove initialization of the previously removed members.
- Change ```SetOutputPrefix("...");``` to ```Prefix("...");```.
- Add call to ```Setup();``` in the constructor;
- Add inheritance ```, public ROOT::Detail::RDF::RActionImpl<HelperName>``` with HelperName being the name of your helper action class.
- Add new member function:
```
   ROOT::RDF::RResultPtr<TList> Book(ROOT::RDataFrame* d) override {
      return d->Book<TGriffin>(std::move(*this), {"TGriffin"});
   }
```
with the correct detectors as template specification and branch names.
- Add ```unsigned int slot``` argument to ```CreateHistograms()``` member function.
- Replace FillHistograms line with ```void Exec(unsigned int slot, TGriffin& grif);``` or whatever detector you are using. Make sure you use references here ("&" after class name)
- Add two external functions:
```
// These are needed functions used by TDataFrameLibrary to create and destroy the instance of this helper
extern "C" HelperName* CreateHelper(TList* list) { return new HelperName(list); }

extern "C" void DestroyHelper(TGRSIHelper* helper) { delete helper; }
```
with HelperName being the name of your helper class action.
Source file:
- Remove first line with ```#ifdef```.
- Remove lines with loops over maps that add histograms to output list.
- Change include of helper file to point to correct .hh file.
- Add ```unsigned int slot``` argument to CreateHistograms member function.
- Add ```[slot]``` to all calls to maps, like fH1, fH2, etc. (:%s/fH2\[/fH2\[slot\]\[/g and :%s/fH2\.at/fH2\[slot\]\.at/g).
- Change ```FillHistograms()``` to ```Exec(unsigned int slot, TGriffin& grif)``` or whatever detectors you are using (matching the declaration in the header file).
- Change calls to detector pointers (like ```fGrif->```) to the reference provided as arguments to the Exec member function (like ```grif.```) (:%s/fGrif->/grif./g).
- If necessary, change the name of the hit to not clash with the argument name (e.g. rename hits from grif to grif1).

