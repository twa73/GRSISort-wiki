# TSourceCalibration

TSourceCalibration is a GUI class similar to TBGSubtraction. The constructor for the class takes the following arguments
1. sigma to be used to find peaks
2. threshold to be used to find peaks
3. degree of polynomial to be used for calibration
4. number of charge vs. channel matrices provided
5. the matrices

[[/images/TSourceCalibrationSourceSelection.png]]

After constructing an object of the class a window will be shown to select which source corresponds to which matrix. If the names of the matrices include a recognized source isotope, the source selection will be pre-filled with the corresponding isotope, otherwise the user will have to use the drop-down menu to select the correct one. Any source file that is found in ```$GRSISYS/libraries/TAnalysis/SourceData````` (`$GRSISYS/SourceData` for cmake installations) can be used as a source isotope, not just the provided choices.

The matrices should have the charge on the y-axis and the channel address as labels on the x-axis. These kind of matrices can be produced using the AlphanumericalHelper that can be found in ```$GRSISYS/examples``` (or using the AlphanumericalSelector that can be found in ```$GRSISYS/GRSIProof)```.

Once the user presses the "Accept & Continue" button the code will go through all the spectra, find peaks using the TSpectrum class with the provided sigma and threshold for an initial calibration. A progress bar will show the progress through the spectra (this happens twice, once for constructing the tabs and canvases, and once for finding the peaks).

[[/images/TSourceCalibrationProgress.png]]
[[/images/TSourceCalibrationProgress2.png]]

Once the initial calibration has been obtained for a channel, the code will loop through all peak energies for each source that are above the minimum required intensity, calculate the channel position the peak should be at, and attempt to fit the peak.
After the peaks have been found and fitted the resulting accepted peaks and calibration is shown for each source and channel:

[[/images/TSourceCalibrationResult.png]]

The "Navigation" buttons allow the user to cycle through the channel pads, even those that are not directly accesible via the tabs if there are too many channels for the window.
This group of buttons also includes buttons to either discard or accept the individual channel's calibration, or accept the result for all of the channels.

The "Fitting/Calibrating" group of buttons allow the user to re-run the algorithms to find the peaks for the currently selected source of the selected channel, all sources of the selected channel, or all source of all channels. The user can also re-run the calibration with the currently selected degree, or recursively remove all data point that exceeed the given maximum residual.

The parameters the user can change in the "Parameters" group are
- Sigma: this is the sigma used by the `TSpectrum` class to find peaks for the initial calibration, and is also used to determine if peaks have been used (which changes how their fit functions are drawn). It has no influence on the code past the initialization.
- Fit range: the range used to fit the source peaks. If two peaks are too close to each other so that the ranges overlap, the range is automatically reduced to either the middle between the two peaks, or half the initial range, whichever is larger.
- Degree of polynomial: the degree of the polynomial used to calibrate this channel. This can be adjusted for each channel, but generally it is better to use the same degree for all channels.
- Max. residual (in keV): the maximum residual accepted for data points. This parameter is used by the "Recursive Remove" button to recursively remove data points.

There are also two general settings that can be selected here, one is whether the "Fitting/Calibrating" buttons should be applied only to the selected channel (default), or to all channels (in this case "Find Channel Peaks" and "Find All Peaks" are the same thing), the other is whether to write nonlinearities (essentially the residuals).

It should also be possible to add individual peaks to the calibration or remove them from it (this has to be checked).

Once all results have been either discarded or accepted, the program writes out the new calibration file and ends.

There are also two tabs for each channel showing a graph of the FWHM of each fit versus the peak position, and the initial calibration used to find at which channel the source peaks should show up.

[[/images/TSourceCalibrationFwhm.png]]
[[/images/TSourceCalibrationInitial.png]]

Once the calibration is finished it will create a .cal file with the name being the names of all the isotopes used combined.

An example script to start this calibration would be

```
void sources()
{
  TFile *eu152 = new TFile("eu152_15275.root");
  TFile *ba133 = new TFile("ba133_15277.root");
  TFile *co56 = new TFile("co56_15278.root");
  TH2D *hEu152 = static_cast<TH2D *>(eu152->Get("ChargeVsChannel"));
  hEu152->SetName("hEu152");
  TH2D *hBa133 = static_cast<TH2D *>(ba133->Get("ChargeVsChannel"));
  hBa133->SetName("hBa133");
  TH2D *hCo56 = static_cast<TH2D *>(co56->Get("ChargeVsChannel"));
  hCo56->SetName("hCo56");
  TChannel::ReadCalFile("run15275.cal");
  // start the calibration itself, using a sigma of 2. keV, threshold of 5% of
  // the largest peak in the spectrum quadratic calibration, and all three
  // matrices we've set up above
  TSourceCalibration *s = new TSourceCalibration(2., 0.05, 2, 3, hEu152, hBa133, hCo56);
}
```

This opens three files and gets the ChargeVsChannel TH2D histograms from each one, renaming the histogram to h<isotope>. A pre-existing cal-file is read and then TSourceCalibration object is created.

The result will be a file called ```Eu152Ba133Co56.cal``` and a ```TSourceCalibration.root``` root file with the ```TCalibrationGraphSet``` object for each channel (plus the FWHM).
