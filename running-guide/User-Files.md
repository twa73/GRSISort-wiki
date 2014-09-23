[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**USER FILES**](User-Files)

The two files used for creating user defined histograms from the fragment tree are:

1. [UserInitObj.h](User-Files#userinit), which initializes the histograms
2. [UserFillObj.h](User-Files#userfill), which gives the rules on how to fill the histograms

<a name="userinit" />
#UserInitObj.h
The **UserInitObj.h** file is where we *initialize* the histograms to be created during the fragment sort. A sample of initializing histograms is as follows:
```c++
GetOutputList()->Add(new TH1D("Charge_0x0000","Charge_0x0000",8000,0,4000));
GetOutputList()->Add(new TH1D("Charge_0x0001","Charge_0x0001",8000,0,4000));

GetOutputList()->Add(new TH2D("hp_charge","Channel vs Charge",64,0,64,4000,0,4000));

```



<a name="userfill" />
#UserFillObj.h
