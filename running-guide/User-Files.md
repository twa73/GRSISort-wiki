[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**USER FILES**](User-Files)

User files are used in the TFragmentSelector to create user defined histograms. The two files used for creating user defined histograms from the fragment tree are:

1. [UserInitObj.h](User-Files#userinit), which initializes the histograms
2. [UserFillObj.h](User-Files#userfill), which gives the rules on how to fill the histograms

<a name="userinit" />
#UserInitObj.h
The **UserInitObj.h** file is where we *initialize* the histograms to be created during the fragment sort. A sample of initializing histograms is as follows:
```c++
GetOutputList()->Add(new TH1D("Charge_0x0000","Charge_0x0000",8000,0,4000));
GetOutputList()->Add(new TH1D("Charge_0x0001","Charge_0x0001",8000,0,4000));

GetOutputList()->Add(new TH1D("TriggerPattern","TriggerPattern",256,0,256));

GetOutputList()->Add(new TH2D("hp_charge","Channel vs Charge",64,0,64,4000,0,4000));

````
GetOutputList() allows the user to select one of the data members from the *TFragment class*. These data members can be found in the **_Technical Documentation_** section of the wiki page. A few examples include Cfd, ChannelID and TriggerID as well as the ones listed above. The bin size as well as max/min values for the axes follow the regular root TH1D and TH2D documentation. 

The URLS for those are found here:

1. http://root.cern.ch/root/htmldoc/TH1D.html
2. http://root.cern.ch/root/htmldoc/TH2D.html

If you want to initialize your own *new* histogram in this class you can try the following:

```c++
	GetOutputList()->Add(new TH2D("new_hp_charge","Channel vs Charge no filter",64,0,64,4000,0,4000));
```

<a name="userfill" />
#UserFillObj.h
The **UserFillObj.h** file is we define rules to fill the histograms created during the fragment sort. A sample of filling the histograms is as follows:
```c++
TH1D *hist = (TH1D*)(GetOutputList()->FindObject(Form("Charge_0x%04x",fragment->ChannelAddress)));
if(hist) hist->Fill(fragment->Charge.at(0)/512.0);

TH1D *hist = (TH1D*)(GetOutputList()->FindObject("TriggerPattern"));
if(hist && fragment->TriggerBitPattern>-1) hist->Fill(fragment->TriggerBitPattern);

TH2D *mat = (TH2D*)(GetOutputList()->FindObject("hp_charge"));
if(mat) mat->Fill(channel->GetNumber(),fragment->Charge.at(0)/512.0);

```
Histograms are given a name (**hist** in the first example **mat** in the last) then they search for a name that was given in UserInitObj.h. For a straight forward example "TriggerPattern" is the name given in UserInitObj.h,  this histogram is found and then filled using the proper data member. For another example, using charge, it looks for the specific channel where the charge is coming from by looking at the channel address. The multiple channels will be filled on different plots based on these channel addresses. Here you can also place conditions which will control when the histograms are being filled, these are seen in the if-statements prior to filling.

Following the example above if you want to fill your *new* histogram you can try the following:

```c++
       TH2D *matnofil = (TH2D*)(GetOutputList()->FindObject("new_hp_charge"));
	if(matnofil && fragment->TriggerId < 0) matnofil->Fill(channel->GetNumber(),fragment->Charge.at(0)/512.0); 
``` 

