Here are some random tips and tricks, mostly instigated by the question: how I do X?

* GRSISort has a `.grsirc` file located in `${GRSISYS}/`. Set-up variables and preferences are included here.
* You can create your own logon script that will be executed every time GRSISort is started. Just set `Rint.Logon` in `.grsirc` to the script's filename.
* If you want to switch back to using `TRootBrowser`, edit `Browser.Name` in `.grsirc` from `GRootBrowser` to `TRootBrowser`.
* If you want the statistics box to display something different than the default (name, entries, mean, and RMS) you can use e.g. ```gStyle->SetOptStat("neuoi");``` to set it to show name, entries, underflow, overflow, and integral.
