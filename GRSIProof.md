<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**GRSIProof**](GRSIProof)
GRSIProof is used to run a batch analysis script on either fragment trees or analysis trees. It uses the PROOF system to distribute the sorting of the tree over multiple threads (slaves) before adding the resulting histograms back together.

1. [Creating your own analysis selector] (GRSIProof#create_script)
2. [Creating Histograms] (GRSIProof#create_histos)
3. [Filling Histograms] (GRSIProof#fill_histos)
4. [Running GRSIProof] (GRSIProof#running)
5. [Running Options] (GRSIProof#running_options)

-----------------------------------------
#How To Use
-----------------------------------------
Effort has gone into ensuring that the `PROOF` framework is very easy to use for beginners still learning to use `ROOT`. For those interested in the `GRSIProof` code, the main program is located in the `GRSIProof` directory of `GRSISort`. PROOF uses the concept of a **selector** which is a class that knows how to process a single entry within a tree. In the context of `GRSISort`, this means either a single fragment, or a packaged event. First we will go through how and where to manipulate your own selectors for analysis.
##Creating your own analysis selector <a name="create_script" />
There are examples of selectors in the `GRSIProof` directory. The more useful, and more complicated event selector  (`ExampleEventSelector`) will be used as an example here. Everything is analogous for fragment files.

The first thing that must be done is to change the class name. This must be done in both the `.C` and `.h` files within the `GRSIProof` directory. This must match the name of the scripts themselves! This also includes the namespace in front of every function. For example, 
```c++
void ExampleEventSelector::InitializeBranches(TTree* tree){
```
Must become:
```c++
void MyAwesomelyNamedSelector::InitializeBranches(TTree* tree){
```
These **must be changed** in both the `MyAwesomelyNamedSelector.h` and the `MyAwesomelyNamedSelector.C`. You must ensure include files and `ifdef`s are correctly named as well.
###Setting up detector classes
The detector systems you plan on analyzing using this script must also be declared in the `.h` file. For example, in `ExampleEventSelector`, there are lines of code with:
```c++
TGriffin * fGriff;
TSceptar * fScep;
```
A pointer to your detectors are setup at this point:
```c++
TAwesomeDetector * fAwesomeDet;
```
The code also needs to be told where to find the branch that contains your detector. This is done in the `.h` file in the function `InitializeBranches`. In our example, we would change
```c++
tree->SetBranchAddress("TGriffin",&fGrif);
tree->SetBranchAddress("TGriffin",&fGrif);
``` 
to:
```c++
tree->SetBranchAddress("TAwesomeDetector",&fAwesomeDet);
tree->SetBranchAddress("TGriffin",&fGrif);
``` 

###Change Output file name
This simple change can be done in the header file in the constructor of the selector. For example, the prefix of the file is changed by changing
```c++
SetOutputPrefix("ExampleEvent");
```
to:
```c++
SetOutputPrefix("AwesomeSauce");
```
For an analysis tree `analysis07654_002.root`, this would create an output root file called `AwesomeSauce07654_002.root`. You can put whatever you want for the prefix, it does not have to be related to the class name. 

##Creating Histograms <a name="create_histos" />
The histograms are defined in the `.C` file in the function `CreateHistograms` (in our case `MyAwesomeSelector::CreateHistograms()`). We have setup a map that allows for easy access of histograms across various locations in the selector. This means to create a 1D histogram, one should use `fH1`, a 2D histogram in `fH2` etc. If the histogram type you want does not exist, please place a request on the issue tracker. As an example, the line:
```c++
fH1["gE"] = new TH1D("gE","#gamma Singles",12000,0,3000);
```
creates a 1D histogram with `double` precision called `"gE"` and puts it in a map position of `"gE"`. You do not need to match the name to the map position, but it is likely easier to keep track of things if you do this. The loops automatically add these histograms to the output root file using the lines:
```c++
for (auto it: fH1) {
   GetOutputList()->Add(it.second);
}
```
These **must not** be changed.

##Filling Histograms <a name="fill_histos" />
To fill the histograms you just created, you can change the function located in the `.C` file named `FillHistograms` (in our case `MyAwesomeSelector::FillHistograms()`). You use the map to lookup the histogram you want to fill, and then fill it like a normal histogram. For example, to fill the 1D energy spectrum created above, we would loop over all `Awesome` hits, find the energy of the hit, and put it in our `"gE"` histogram.
```c++
for(auto i = 0; i < fAwesomeDet->GetMultiplicity(); ++i){
   fH1["gE"]->Fill(fAwesomeDet->GetAwesomeHit(i)->GetEnergy());
} 

##Running GRSIProof <a name="running" />
You run `grsiproof` as a separate program from GRSISort. The basic way to do so is:
```bash
grsiproof root_file_to_sort.root location_to_selector.C
```
for example:
```bash
grsiproof analysis06578_000.root $GRSISYS/GRSIProof/MyAwesomeSelector.C
```
a list of analysis files can be used and they will all be chained and sorted at the same time. This includes regular expressions. The output file name will be named after the first file in the list. For example:
```bash
grsiproof analysis06578_000.root analysis06578_001.root analysis06578_002.root $GRSISYS/GRSIProof/MyAwesomeSelector.C
```
would work and create a file called `AwesomeSauce06578_000.root`. One could also use:
```bash
grsiproof analysis06578_???.root $GRSISYS/GRSIProof/MyAwesomeSelector.C
```
to accomplish the same task.

##Running GRSIProof <a name="running_options" />
There are options that can be provided on the command line to change the way `grsiproof` runs. Some options are listed below with what they do. A full list can be viewed with `grsiproof --help`.
  * __--max-workers #__,   Sets the number of slaves to be used. Each worker creates each histogram you create, so this is a good option to reduce memory consumption when using many histograms. Defaults to number of cores on computer.
  * __--selector-only__,    Bypasses proof, and just runs the selector on one thread. Good for debugging, as proof redirects output messages.