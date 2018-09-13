The angle between two sequentially emitted gamma rays has a distribution that is dependent on the spins of the three states involved and the mixing ratio of the transitions. Measuring an angular correlation can assist in spin assignment or measurements of particular transition strengths.

The angular correlation class is designed to assist in the creation of angular correlation spectra. This includes the bookkeeping of identifying detector pairs that share the same opening angle and fitting gamma-gamma coincidence peaks. In the future, it will include easy mechanisms for folding the angular correlation spectra as well grouping the opening angle binning.

This class is designed to be used primarily in the interactive shell of GRSISort after all the data has been sorted, similar to the investigation of gamma-gamma coincidences. This is why we assume the initial creation of a 3D matrix with all gamma-gamma coincidences included - you don't want to have to sort through all the data when you need a new angular correlation. The class uses angular indices because it saves space, provides a clean way to collect all coincidences between geometrically equivalent detectors without worrying about your bin size, and allows you assign the angles at the end, just in case you want to apply energy-corrected opening angles.

The expected work flow for the creation of event-mixed angular correlation spectra is drawn out below. Red boxes will be created by the user outside of the class. Red arrow indicate expected actions that are outside the scope of the class. Each black arrow corresponds to a function provided by the class. The green and orange boxes  correspond to data members inside two different instances of the angular correlations class.

![](https://raw.githubusercontent.com/wiki/GRIFFINCollaboration/GRSISort/images/correlation_creation_workflow_english_simple.png)

Or, if  you prefer lists, the workflow could be described as follows:

1. Create 3D array/matrix of gamma energy vs. gamma energy vs. angular index.
2. Apply energy gates to select the cascade of interest.
3. Normalize out the weights and efficiencies.
4. Convert a histogram of counts vs. angular index to a graph of counts vs. angle.

## Create 3D array

### Map creation
The first step is to create an angular index for each possible opening angle in the array. This can be done with `GenerateAngleMap`.

The second step is to create a map between detector pairs and angular index. Here, a GRIFFIN detector is identified by its array number. This map can be used in the creation of the 3D arrays of angular index vs. gamma energy vs. gamma energy. This can be done with `GenerateIndexMap`.
The third (optional) step is to create a list of how many detector pairs contribute to each angular index (the weight of that index). This can be done with `GenerateWeights`.

All three of these can be done at once with `GenerateMaps`. If you have a typical GRIFFIN set-up with 8, 11, 12, 15, or 16 clovers, and all of them at the same radial distance, you can create these maps with the command:
```
GenerateMaps(16,110)
```
where the first argument (16 in the example) is the number of clovers and the second (110 in the example) is the radial distance of the detectors in mm.
###3D array creation
This is performed entirely outside the scope of the class, though the maps will be useful in assigning gamma-gamma coincidences to the proper angular index. Please create your 3D array as a `ThnSparse` with axes of gamma energy, gamma energy, and angular index. It should be symmetric in gamma-gamma energy. Time-random background subtraction should be performed here.

## Apply energy gates

### First gamma-energy gate
The first gamma energy gate is applied with `Create2DSlice`. This function takes in your 3D array and projects out the events that are coincident with a gamma with an energy between a minimum and maximum. Here is where you should do energy-dependent background subtraction. To do such a subtraction, create two instances of the class with different 2D slices, and subtract one from the other.

### Second gamma-energy gate / Peak fitting
A second gamma-energy gate can be applied with `IntegralSlices`. This function will take in a 2D histogram and project onto the angular index axis the events that have a gamma with an energy between a minimum and maximum.

Alternatively, the coincident peaks can be fit with `FitSlices`. This function will project out the gamma energy spectrum for each angular index, fit a peak it, and then return a histogram of peak area vs. angular index. Because fitting algorithms are notoriously fragile, all the projections and fits are saved to the instance. Diagnostic plots of the peak area, peak centroid, peak FWHM, and fit chi^2 are printed to screen. Optionally, all the peak fits can also be projected to the screen. Individual fits can be corrected with the `UpdatePeak` function, which will also update the diagnostic plots.

## Normalize the histogram

The spectrum that we have now is heavily skewed by the number of detector pairs that contribute to each index and (to a lesser degree) the relative efficiencies of each detector. The weights can be divided out with the `DivideByWeights` function. Relative efficiencies are best dealt with in the initial creation of the 3D matrix.

Alternatively (and probably preferably), one can use the event-mixing technique to normalize the histogram.

## Convert angular indices to angles
...or more accurately, to a value of cos(theta). This can be done with the `CreateGraphFromHst` function.

## After the data
The impact of finite detector size will still impact the shape of your event-mixed distribution. You'll need to account for that. The angular correlation NIM paper will be an excellent resource for you, as well as [these example scripts](https://github.com/SmithJK/AngularCorrAnalysisExamples) and the README within. You may need simulations, in which case the GitHub repos [here](https://github.com/GRIFFINCollaboration/detectorSimulations_v10) and [here](https://github.com/GRIFFINCollaboration/Geant4GammaGammaAngularCorrelations10.01.p01) will be helpful. If your data have poor enough statistics that you don't necessarily need simulations, you'll at least need some attenuation factors, which can be found for the 16-clover setup [here](https://smithjk.github.io/AngularCorrelationCoefficients/index.html).
