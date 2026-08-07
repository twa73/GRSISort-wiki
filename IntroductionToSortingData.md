# Introduction to Sorting (GRIFFIN) Data with GRSISort

This is a short introduction to the basic steps for analyzing/sorting data with GRSISort. This guide is oriented towards the steps necessary for GRIFFIN data, but most of these are applicable to data from other experiments as well.
While the steps are given in an order, not all of them need to necessarily be performed in that order, if a step depends on another step before it, it will be explicitly mentioned.

There are a number of terms used throughout this guide that are "slang" and can be referred to in the [[glossary of GRSISort terms|Glossary]].

- [Preparation](#preparation)
- Check the (rough) [timing](#detector-timing) of detector systems.
- Perform an [energy calibration](#energy-calibration).
- Check and (if necessary) correct data for [gain drift](#gain-drift), this relies on an existing energy calibration.
- Determine the [cross-talk correction factors](#cross-talk-correction), relies on an existing energy calibration.
- Determine the [efficiency of the array](#efficiency), relies on an existing energy calibration and good time alignment.

## Preparation

Create a .cal-file either by copying one used during the online analysis, or by sorting a MIDAS file (.mid) without a .cal-file, and then using `TChannel::WriteCalFile("name.cal")` to create a new cal-file called "name.cal" (this can be changed to any other name you want, just keep the .cal extension) based on the online ODB information.

## Detector timing

One important thing to check before sorting too much of the data from the raw MIDAS files to analysis trees is the (rough) timing of the detector systems. To check this create a helper that plots the time difference of two different detector systems.
The easiest thing to do is to correct all detector systems to one specific detector (e.g. the ZDS detector) or detector system (e.g. all GRIFFIN detectors).
The resulting spectra should have a single peak from coincident events sitting on a more or less flag background from time random events. If this peak is not close to zero, the "TimeOffset" parameter should be set for the detector system to be adjusted (this parameter is an integer in ns units). This parameter is subtracted from the timestamp value when the `GetTime` or `GetTimeStampNs` functions are being used and thus is applied when hits are being sorted into events. This parameter should be used to get all different detector systems to a rough time alignment, the time difference of the different systems needs to be significantly smaller than the sorting window used to ensure that all coincident hits end up being sorted into the same events.
This also means that if this parameter is changed by a significant amount (compared to the sorting window) all raw data files should be re-sorted into analysis trees (which is why this is listed as the first step and it is recommended to do this before sorting too much of the raw data files).

## Energy Calibration

There are two ways of doing the energy calibration, either by using well known calibration source (e.g. <sup>133</sup>Ba, <sup>152</sup>Eu, <sup>56</sup>Co, and <sup>60</sup>Co), or by using in beam data (if there are enough well known gamma ray energies in the decay being studied).
In either case the runs should be sorted with the alphanumeric helper that can be found in the examples folder.
The resulting files can then be used as input for `TSourceCalibration` with either the provided .sou-files for source data or a user generated .sou-file for in beam data.

## Gain Drift

To check if gain drift can be observed, we need to compare the energy spectra for different runs throughout the experiment using the `PlotVsRun` program that is included in GRSISort. This can either be done by using histograms from just a few files (e.g. a run at the beginning of the experiment and one towards the end of the experiment), or by using histograms from all runs.
The histograms can be simple 1D energy histograms for all GRIFFIN detectors, or (preferably) 2D histograms with the energy versus the crystal number. The latter makes it easier to see if single detectors/crystals show gain drifts even if the majority of the crystals does not have any variation in gain.

If gain drift is observed, the `GainDrift` program can be used to calculate a "secondary" gain drift calibration to be applied on top of the existing energy calibration.
This program needs a list of input files (with matrices of energy versus the crystal number, as can be produced with the alphanumeric helper), and a list of the energies to use to determine the gain drift. This list can alternatively be provided via a `TUserSettings` compatible .par-file together with optional settings like a list of energy uncertainties, the degree of the gain drift correction (default is 1), the +- range for the fit of each energy, the minimum FWHM for a fit to be considered good, a non-default name for the histogram, a prefix for the output files, and either a list of included or a list of excluded bins/crystals.

## Cross Talk Correction

The cross talk correction aims to reduce the effects of electronic cross talk between different crystals of one GRIFFIN detector.
This effect is naturally more dominant when looking at addback data compared to singles data, and is especially obvious when looking at addback hits with more than one crystal involved.
The `CrossTalkCheckHelper.cxx` in the examples directory can be used to plot this effect and to compare the energy spectra before and after applying the cross talk correction factors.
To determine the cross talk correction factors one can use the `CrossTalkHelper.cxx` in the examples directory with an optional `TUserSettings` compatible .par-file that can be used to change the number of bins and the energy range of the produced histograms (default is 10000 bins from 0 to 2000 keV).
This helper can be used with a <sup>60</sup>Co source run or any other source or in beam data, as long as there is a high energy gamma ray present with enough statistics.
The output file from this helper will have 2D histograms showing the energy of one crystal versus another crystal in a GRIFFIN detector for each detector and for each crystal pair combination.

This file can then be used as input for the `CrossTalkFix` program included in GRSISort.
The `CrossTalkFix` program takes the matrix file, a .cal-file, and an optional user settings file as input.'
The user settings file can be used to change the energy of the gamma-ray peak used (default is 1332 keV), the minimum number of counts required to determine the mean and root mean square, the first and last detector, a list of detectors to be excluded, a channel prefix (default is "GRG"), and a channel postfix (default is "N00A").

## Efficiency

To determine the efficiency of GRIFFIN, we use several standard gamma-ray sources like <sup>133</sup>Ba, <sup>152</sup>Eu, <sup>56</sup>Co, and <sup>60</sup>Co. Since these sources in general emit multiple gamma rays of different energies at once, we need to correct for "summing in" and "summing out" (see also [[here|summing-corrections]]) to determine the effiency to detect a single gamma ray of a given energy.
The `SummingCorrectionsHelper.cxx` in the examples directory can be used to generate all histograms needed for this for any of the five methods to analyze the gamma-ray data, using crystals without suppression, crystals with suppression, addback without suppression, addback with suppression, or the so called single crystal method (where we only use suppressed addback hits where only one crystal was involved, i.e. using the other three crystals in the detector into additional Compton scattering suppressors).
