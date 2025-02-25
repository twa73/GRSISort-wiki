The angle between two sequentially emitted gamma rays has a distribution that is dependent on the spins of the three states involved and the mixing ratio of the transitions. Measuring an angular correlation can assist in spin assignment or measurements of particular transition strengths.

To create an angular distribution for GRIFFIN two main steps are needed:
1. Creating 2D histograms of gamma-gamma energies for all possible angle combinations using grsiframe and the provided AngularCorrelationHelper.
2. Analysing a specific cascade using the AngularCorrelations program.

## grsiframe and AngularCorrelationHelper

To create the 2D histograms we use the AngularCorrelationHelper and grsiframe like this:
`grsiframe <grsiframe options, i.e. --max-workers 2> $GRSISYS/examples/AngularCorrelationHelper.cxx <analysis file(s)> <parameter file with specfic settings> 2>&1 | tee <log-file name>`
where the parameter file (see `$GRSISYS/examples/AngularCorrelation.par` for an example) holds all the specific settings that determine what angles to use:

| Parameter name     | default | possible values | comments |
|--------------------|---------|-----------------|----------|
|GriffinDistance     | 145.    | 145. or 110.    | distance of the detector front face from the center of the arrya, note the trailing . |
|Addback             | true    | true or false   | whether to use addback or not when creating the spectra |
|Folding             | false   | true or false   | whether to fold all angles around 90 degrees, i.e. map 180 degree to 0 degree, 135 degree to 45 degree and so on |
|Grouping            | false   | true or false   | whether to group specific angles together which are close to each other, this also activates `Folding` unless custom grouping is used |
|CustomGrouping      | empty   | comma separated list of integers | list of groups for angles, needs to be as many entries as there are angles |
|ExcludeDetector     | empty   | comma separated list of integers (1-16) | list of all detectors that will be excluded in calculating the angles and creating the spectra |
|ExcludeCrystal      | empty   | comma separated list of integers (1-64) | list of all crystals that will be excluded in calculating the angles and creating the spectra |
|MaxPromptTime       | 200.    | positive floating point number | maximum absolute time difference between two gammas for them to be considered to be in coincidence with each other |
|TimeRandom.Low      | 400.    | positive floating point number | minimum of the absolute time difference between two gammas for them to be considered time random |
|TimeRandom.High     | 600.    | positive floating point number | maximum of the absolute time difference between two gammas for them to be considered time random |
|NumberOfBins        | 3000    | positive integer | number of bins in the x- and y-axis of the matrices |
|MinimumEnergy       | 0.      | positive floating point number | minimum on the x- and y-axis of the matrices |
|MaximumEnergy       | 3000.   | positive floating point number | maximum on the x- and y-axis of the matrices |
|NumberOfMixedEvents | 10      | positive integer | number of events to be used for event mixing |

This helper will create prompt, time random, and event mixed histograms for each angle. 
"Prompt" means these are matrices for the energies of two gamma rays that are in coincidence with each other, as determined by the "MaxPromptTime" parameter.
"Time random" means these matrices are for gamma rays that are not in coincidence with each other but are randomly within a certain time frame of each other, as determined by the "TimeRandom.Low" and "TimeRandom.High" parameters.
These time random matrices can be used to correct for background from time random gamma rays within the coincidence window.
"Mixed" means these matrices are generated using two gamma rays from two different events, ensuring that they are completely uncorrelated with each other.
The number of previous event that are held in memory for this mixing is determined by the "NumberOfMixedEvents" (default is 10).

The names of the histograms are "AngularCorrelationN" for the prompt matrices, "AngularCorrelationBGN" for the time random matrices, and "AngularCorrelationMixedN" for the mixed event matrices, and N is an integer index starting at 0.
The instances of TGriffinAngles and TUserSettings are also written to the output file, so one can see what settings have been used to create the root-file.

The difference in angles under which we consider the angles identical defaults to 0.001.
This value can not be changed by a user setting, but could be changed by adding a line `TGriffinAngles::Rounding(new value)` to the constructor of the helper.
This setting should only be changed if you are absolutely certain you know need it to be changed and know what you are doing.

If the statistic of the data set is limited, it can help to use folding and/or grouping to increase the statistics per histogram.
Folding means that we take advantage of the symmetry of the angular distributions around 90 degree and map 180 degree to 0 degree, 135 degree to 45 degree, and so on.
This will double the statistics in the histograms (apart from the 0 degree histogram since we only have the 180 degree data in there), and reduce the number of histograms from 51 to 26.
If folding alone does not help, grouping can be enabled which groups angles that are close to each other together.
This reduces the number of histograms further down to 11 (110 mm distance) or 12 (145 mm distance).
Using grouping will reduce the information in the data (unlike folding), so the `TGriffinAngles` class enforces folding to be used if grouping is enabled.

It is also possible to use custom grouping by providing a list of the indices of the group each angle belongs to.
Adding a line
`
CustomGrouping: 0, 1, 2, 3, 3, 4, 4, 4, 4, 5, 5, 6, 7, 8, 8, 8, 8, 9, 9, 10, 11, 12, 13, 14, 15, 15
`
would create a total of 16 groups with 1 to 4 angles in each group.
If custom grouping is provided folding does not have to be enabled, i.e. custom grouping allows to create groups for all 51 angles without mapping angles above 90 degree to those below 90 degree.

## AngularCorrelations program

To analyze a specific gamma cascade we use the AngularCorrelations program.
This program takes the matrices created by the helper in the previous step, projects an energy range around the first peak on the y-axis onto the x-axis, subtracts projections of background areas, and fits these with the second peaks.
The areas of these peaks are then used to calculate the angular distributions by dividing the areas in the projections of the time random corrected prompt matrices by the areas in the projections of the event mixed matrices.
This division ensures that any effects from the different number of combinations of crystals that can contribute to a specific angle, or the different efficiencies of the detectors are accounted for.
Parameters for this part of the program are:

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|Projection.Low  | positive floating point number | the low edge of the range of values on the y-axis that will be projected onto the x-axis, can be overwritten by the `projection-low` flag |
|Projection.High | positive floating point number | the high edge of the range of values on the y-axis that will be projected onto the x-axis, can be overwritten by the `projection-high` flag |
|TimeRandomNormalization | positive floating point number | the ratio between the time window of the prompt gamma-gamma spectra, and the time window of the time random gamma-gamma spectra, can be overwritten by `time-random-normalization` flag |
|Peak.Position   | positive floating point number | the position of the peak to be fitted in the projections, can be overwritten by the `peak-pos` flag |
|Peak.Position.Low| positive floating point number | the low edge of the range of the peak fit in the projections, can be overwritten by the `peak-low` flag |
|Peak.Position.High| positive floating point number | the high edge of the range of the peak fit in the projections, can be overwritten by the `peak-high` flag |
|Background.N.Low | positive floating point number | the low edge of the Nth background projection to be subtracted from the projection of the first gamma peak, n is give by N which is an integer starting at 0 |
|Background.N.High| positive floating point number | the high edge of the Nth background projection to be subtracted from the projection of the first gamma peak, n is give by N which is an integer starting at 0 |
|BackgroundPeak.Position.N | positive floating point number | positions of background peaks that are included in the fit of the projection but whose areas are ignored, N is the index starting at 0 |
|Peak.Parameter.N | floating point number | initial value for Nth parameter of the peak, this value is only used if the limits are also set |
|Peak.Parameter.N.Low | floating point number | lower limit for Nth parameter of the peak, if this is the same as the upper limit, the parameter is fixed to this value |
|Peak.Parameter.N.Hihg| floating point number | upper limit for Nth parameter of the peak, if this is the same as the lower limit, the parameter is fixed to this value |
|BackgroundPeak.Parameter.N | floating point number | initial value for Nth parameter of the background peak(s), this value is only used if the limits are also set |
|BackgroundPeak.Parameter.N.Low | floating point number | lower limit for Nth parameter of the background peak(s), if this is the same as the upper limit, the parameter is fixed to this value |
|BackgroundPeak.Parameter.N.High| floating point number | upper limit for Nth parameter of the background peak(s), if this is the same as the lower limit, the parameter is fixed to this value |
|Background.Offset(.Low/.High) | floating point number | value, lower, and upper limit for the constant offset in the global background of the fit |
|Background.Linear(.Low/.High) | floating point number | value, lower, and upper limit for the linear factor in the global background of the fit |
|Background.Quadratic(.Low/.High) | floating point number | value, lower, and upper limit for the quadratic factor in the global background of the fit |
|Background.Xoffset(.Low/.High) | floating point number | value, lower, and upper limit for the offset in x used in the global background of the fit |

The created angular distributions can be compared to simulated angular distributions.
To do so, the user has to provided the path to a .root file via the `theory` flag.
That .root file is expected to contain three `TGraphErrors` called "graph000", "graph010", and "graph100", which are the simulated "Z<sub>0</sub>", "Z<sub>2</sub>", and "Z<sub>4</sub>" distributions as described in method 2 of the paper by Smith et al.
If these are provided, the following parameters are being used:

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|TwoJ.Low        | comma separated integers | two times J for the lowest state involved in the cascade, this has to be a vector, even if it has only one entry that entry must be followed by a comma
|TwoJ.Middle     | comma separated integers | two times J for the middle state involved in the cascade, this has to be a vector, even if it has only one entry that entry must be followed by a comma
|TwoJ.High       | comma separated integers | two times J for the highest state involved in the cascade, this has to be a vector, even if it has only one entry that entry must be followed by a comma
|ConfidenceLevel | positive floating point number | the value for the 99% confidence level that the reduced chi<sup>2</sup> is smaller than this (depends on the degrees of freedom)

Of the three vectors only one can have more than one entry, and the program will produce for each entry a graph showing the reduced chi<sup>2</sup> vs. the mixing angle.
These graphs are all saved to the output file, and also plotted on a canvas that is also saved to the output file under the name "MixingCanvas".

Finally the program also does a free fit of a<sub>2</sub> and a<sub>4</sub>, with the results saved to the output file and plotted on a canvas called "A2a4Canvas".

The parameters for the best fit for each spin as well as the parameters of the free fit are also saved to the output file as ParametersN (N being an index starting with 0), or ParametersA2a4Fit.
These parameters can be retrieved from the output file by first declaring a pointer to a vector of doubles, and then using TFile::GetObject to read the desired parameter set:

`
std::vector<double>* par;
_file0->GetObject("ParametersSpin0", par);
`

Running the program as `AngularCorrelations -h` shows all possible command line options.

## Further information about angular correlations

The [angular correlation NIM paper](https://arxiv.org/pdf/1807.07570.pdf) is an excellent resource, as well as [these example scripts](https://github.com/SmithJK/AngularCorrAnalysisExamples) and the README within.
You may need simulations, in which case the GitHub repos [here](https://github.com/GRIFFINCollaboration/detectorSimulations_v10) will be helpful.
If your data have poor enough statistics that you don't necessarily need simulations, you'll at least need some attenuation factors, which can be found for the 16-clover setup [here](https://smithjk.github.io/AngularCorrelationCoefficients/index.html).

