# TSourceCalibration

TSourceCalibration is a GUI class similar to TBGSubtraction. The constructor for the class takes the following arguments
1. sigma to be used to find peaks
2. threshold to be used to find peaks
3. number of charge vs. channel matrices provided
4. the matrices 

[[/images/TSourceCalibrationSourceSelection.png]]

After constructing an object of the class a window will be shown to select which source corresponds to which matrix. If the names of the matrices include a recognized source isotope, the source selection will be pre-filled with the corresponding isotope, otherwise the user will have to use the drop-down menu to select the correct one. Any source file that is found in ```$GRSISYS/libraries/TAnalysis/SourceData``` can be used as a source isotope, not just the provided choices.

The matrices should have the charge on the y-axis and the channel address as labels on the x-axis. These kind of matrices can be produced using the AlphanumericalSelector that can be found in ```$GRSISYS/GRSIProof```.

Once the user presses the "Accept & Continue" button the code will go through all the spectra, find peaks using the TSpectrum class with the provided sigma and threshold. A progress bar will show the progress through the spectra.

[[/images/TSourceCalibrationProgress.png]]

After the peaks have been found and fitted the resulting accepted peaks and calibration is shown for each source and channel:

[[/images/TSourceCalibrationPeakSelection.png]]

Here the user can change the sigma and threshold parameters, run the algorithm to find peaks again (using a slower version or the default faster version), calibrate the channel with a polynomial of the desired degree, and either discard or accept the individual channel's calibration, or accept the result for all of the channels for the selected source tab.

It should also be possible to add individual peaks to the calibration or remove them from it (this has to be checked).

Once all results have been either discarded or accepted, the results of the different sources are all combined and shown. At this point the user can change the degree of the polynomial used to fit the data and re-calibrate the channel using the new polynomial, accept the individual channel, or all of them (which finishes the calibration).

At the time there is a bug that causes the buttons from the previous window to still be shown in this window.

[[/images/TSourceCalibrationResult.png]]

There is also a separate tab where the user can see the relative efficiencies based on the intensities of the sources gamma-rays and the peak areas. This hasn't been fully debugged, and nothing is done with these efficiencies at the moment.

[[/images/TSourceCalibrationEfficiency.png]]

Once the calibration is finished it will create a .cal file with the name being the names of all the isotopes used combined. 

An example script to start this calibration would be

```
void sources()
{
	TFile* eu152 = new TFile("eu152_15275.root");
	TFile* ba133 = new TFile("ba133_15277.root");
	TFile* co56 = new TFile("co56_15278.root");
	TH2D* hEu152 = static_cast<TH2D*>(eu152->Get("ChargeVsChannel"));
	hEu152->SetName("h152Eu");
	TH2D* hBa133 = static_cast<TH2D*>(ba133->Get("ChargeVsChannel"));
	hBa133->SetName("h133Ba");
	TH2D* hCo56 = static_cast<TH2D*>(co56->Get("ChargeVsChannel"));
	hCo56->SetName("h56Co");
	TChannel::ReadCalFile("run15275.cal");
	TSourceCalibration* s = new TSourceCalibration(2., 0.05, 3, hEu152, hBa133, hCo56);
}
```

This opens three files and gets the ChargeVsChannel TH2D histograms from each one, renaming the histogram to h<isotope>. A pre-existing cal-file is read and then TSourceCalibration object is created.

The result will be a file called ```Eu152Ba133Co56.cal``` and a ```TSourceCalibration.root``` root file with the ```TCalibrationGraphSet``` object for each channel (plus the efficiencies).