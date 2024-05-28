<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**ANALYSIS**](Analysis) > [**FITTING**](Fitting)

As part of the GRSISort analysis package, there are a number of classes and methods available for fitting:

## For gamma-ray peaks
* [GGaus](https://rawgit.com/wiki/GriffinCollaboration/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/class_g_gaus.html) Used to fit peaks with a gaussian and a linear background. This class is used when the 'g' key is used on a GCanvas (see [Canvas Hotkeys](Interactive-Analysis)).
* [GPeak](https://rawgit.com/wiki/GriffinCollaboration/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/class_g_peak.html) Used to fit peaks with a gaussian + skewed gaussian and a background of a step function + constant. This class is used when the 'f' key is used on a GCanvas (see [Canvas Hotkeys](Interactive-Analysis)).
* [TPeak](TPeak) Used to fit peaks with a gaussian + skewed gaussian. This class is used when the 'F' key is used on a GCanvas (see [Canvas Hotkeys](Interactive-Analysis)).
* [TPeakFitter](https://rawgit.com/wiki/GriffinCollaboration/GRSISort/technical-docs/ROOT-Gen-Html/htmldoc/class_t_peak_fitter.html) This is a versatile class than can fit any number of peaks of different shapes plus a global background (by default a quadratic polynomial). Each peak has to be added individually and can be one of the classes derived from TSinglePeak:
  * TGauss - a simple gaussian.
  * TRWPeak - a radware like peak.
  * TABPeak - an addback peak with two different gaussians for single hits and double hits.
  * TAB3Peak - an addback peak with three different gaussians for up to three hits.

Below is a comparions of different fits of a standard photopeak with or without including a skewed gaussian:

[[/images/PeakFitComparison1.png]] [[/images/PeakFitComparison2.png]]

In order to fix parameters of specific peaks, use the fit function of the peak:
```
TPeakFitter pf(<low range>,<high range>);
TRWPeak peak(<centroid>);
pf.AddPeak(&peak);
TF1* peakFunc = peak.GetFitFunction();
peakFunc->FixParameter(<index>, <value>);
pf.Fit(<histogram>);
```
To get a list of the parameters use the TF1::Print("v") function (```peakFunc->Print("v")``` in the example above).
To access parameters of the global background function use TPeakFitter::GetBackground() (```pf.GetBackground()->FixParameter(<index>, <value>)``` in the example above). See also [#1262](../issues/1262).

## For decay curves
* [TDecay](TDecay) Used to fit nuclear decay curves.

