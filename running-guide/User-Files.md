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
<a name="userfill" />
#UserFillObj.h
The **UserFillObj.h** file is we define ruls to fill the histograms created during the fragment sort. A sample of filling the histograms is as follows:
```c++
TH1D *hist = (TH1D*)(GetOutputList()->FindObject(Form("Charge_0x%04x",fragment->ChannelAddress)));
if(hist) hist->Fill(fragment->Charge.at(0)/512.0);

hist = (TH1D*)(GetOutputList()->FindObject("TriggerPattern"));
if(hist && fragment->TriggerBitPattern>-1) hist->Fill(fragment->TriggerBitPattern);

TH2D *mat = (TH2D*)(GetOutputList()->FindObject("hp_charge"));
if(mat) mat->Fill(channel->GetNumber(),fragment->Charge.at(0)/512.0);

```


