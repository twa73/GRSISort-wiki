<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**GRSIProof**](GRSIProof)
GRSIProof is used to run a batch analysis script on either fragment trees or analysis trees. It uses the PROOF system to distribute the sorting of the tree over multiple threads (slaves) before adding the resulting histograms back together.

1. [Creating your own analysis selector] (GRSIProof#create_script)

-----------------------------------------
#How To Use
-----------------------------------------
Effort has gone into ensuring that the `PROOF` framework is very easy to use for beginners still learning to use `ROOT`. For those interested in the `GRSIProof` code, the main program is located in the `GRSIProof` directory of `GRSISort`. PROOF uses the concept of a **selector** which is a class that knows how to process a single entry within a tree. In the context of `GRSISort`, this means either a single fragment, or a packaged event. First we will go through how and where to manipulate your own selectors for analysis.
##Creating your own analysis selector <a name="create_script" />
There are examples of selectors in the `GRSIProof` directory. The more useful, and more complicated event selector  (`ExampleEventSelector`) will be used as an example here.

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
