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
|SingleCrystal       | false   | true or false   | when using addback whether or not restrict it to events with a single crystal per detector firing |
|Folding             | false   | true or false   | whether to fold all angles around 90 degrees, i.e. map 180 degree to 0 degree, 135 degree to 45 degree and so on |
|Grouping            | false   | true or false   | whether to group specific angles together which are close to each other, this also activates `Folding` unless custom grouping is used |
|CustomGrouping      | empty   | comma separated list of integers | list of groups for angles, needs to be as many entries as there are angles |
|ExcludeDetector     | empty   | comma separated list of integers (1-16) | list of all detectors that will be excluded in calculating the angles and creating the spectra |
|ExcludeCrystal      | empty   | comma separated list of integers (1-64) | list of all crystals that will be excluded in calculating the angles and creating the spectra |
|MaxPromptTime       | 200.    | positive floating point number | maximum absolute time difference between two gammas for them to be considered to be in coincidence with each other |
|TimeRandom.Low      | 400.    | positive floating point number | minimum of the absolute time difference between two gammas for them to be considered time random |
|TimeRandom.High     | 600.    | positive floating point number | maximum of the absolute time difference between two gammas for them to be considered time random |
|CycleTime.Low       | -1.     | positive floating point number | minimum time in cycle in s for event to be used, -1. means no limit |
|CycleTime.High      | -1.     | positive floating point number | maximum time in cycle in s for event to be used, -1. means no limit |
|NumberOfBins        | 3000    | positive integer | number of bins in the x- and y-axis of the matrices |
|MinimumEnergy       | 0.      | positive floating point number | minimum on the x- and y-axis of the matrices |
|MaximumEnergy       | 3000.   | positive floating point number | maximum on the x- and y-axis of the matrices |
|NumberOfMixedEvents | 10      | positive integer | number of events to be used for event mixing |

This helper will create prompt, time random, and event mixed histograms for each angle. 
"Prompt" means these are matrices for the energies of two gamma rays that are in coincidence with each other, as determined by the "MaxPromptTime" parameter.
"Time random" means these matrices are for gamma rays that are not in coincidence with each other but are randomly within a certain time frame of each other, as determined by the "TimeRandom.Low" and "TimeRandom.High" parameters.
These time random matrices can be used to correct for background from time random gamma rays within the coincidence window.
"Mixed" means these matrices are generated using two gamma rays from two different events, ensuring that they are completely uncorrelated with each other.
The number of previous events that are held in memory for this mixing is determined by the "NumberOfMixedEvents" (default is 10).

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
This program takes the matrices created by the helper in the previous step, projects an energy range around a peak on the y-axis onto the x-axis, subtracts projections of background areas from this projection, and fits these corrected projections with the second peaks.
The areas of these peaks are then used to calculate the angular distributions by dividing the areas in the projections of the time random corrected prompt matrices by the areas in the projections of the event mixed matrices.
This division ensures that any effects from the different number of combinations of crystals that can contribute to a specific angle, or the different efficiencies of the detectors are accounted for.

Parameters that control how the projections are created are:

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|Projection.Low  | positive floating point number | the low edge of the range of values on the y-axis that will be projected onto the x-axis, can be overwritten by the `projection-low` flag |
|Projection.High | positive floating point number | the high edge of the range of values on the y-axis that will be projected onto the x-axis, can be overwritten by the `projection-high` flag |
|Background.N.Low | positive floating point number | the low edge of the Nth background projection to be subtracted from the projection of the first gamma peak, n is give by N which is an integer starting at 1 |
|Background.N.High| positive floating point number | the high edge of the Nth background projection to be subtracted from the projection of the first gamma peak, n is give by N which is an integer starting at 1 |
|TimeRandomNormalization | positive floating point number | the ratio between the time window of the prompt gamma-gamma spectra, and the time window of the time random gamma-gamma spectra, can be overwritten by `time-random-normalization` flag |
|Histograms.BaseName | string | the base name of the prompt histograms, defaults to “AngularCorrelation” |
|Histograms.BackgroundName | string | the base name of the time-random background histograms, defaults to “AngularCorrelationBG” |
|Histograms.MixedName | string | the base name of the mixed-event histograms, defaults to “AngularCorrelationMixed” |

Parameters that control how the peaks of interest are fitted in the projections are:

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|Peak.Low| positive floating point number | the low edge of the fit range in the projections, can be overwritten by the `peak-low` flag |
|Peak.High| positive floating point number | the high edge of the fit range in the projections, can be overwritten by the `peak-high` flag |
|Peak.Position   | positive floating point number | the position of the peak to be fitted in the projections, can be overwritten by the `peak-pos` flag |

If needed the parameters of the `TRWPeak` can be limited (or fixed) using these settings:

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|Peak.Parameter.N | floating point number | initial value for Nth parameter of the peak, this value is only used if the limits are also set |
|Peak.Parameter.N.Low | floating point number | lower limit for Nth parameter of the peak, if this is the same as the upper limit, the parameter is fixed to this value |
|Peak.Parameter.N.High| floating point number | upper limit for Nth parameter of the peak, if this is the same as the lower limit, the parameter is fixed to this value |

If there are other peaks close to the peak of interest in the projection, one can add as many background peaks as wanted (and limit/fix the parameters for these peaks) using the settings below. These background peaks are included in the fit of the projection (within the fit range defined with the parameters above), but their areas are ignored for the calculation of the angular distributions. The “Background.Peak.N.Low” and “Background.Peak.N.High” parameters can be used to limit the position of the Nth background peak. These should be used instead of using parameter #1 (the centroid of the peak), as these parameter limits apply to all background peaks, not just the Nth peak.

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|Background.Peak.N.Position | positive floating point number | positions of background peaks that are included in the fit of the projection but whose areas are ignored, N is the index starting at 1 |
|Background.Peak.N.Low | positive floating point number | the lower limit for the peak position parameter for background peak N, N is the index starting at 1 |
|Background.Peak.N.High | positive floating point number | the upper limit for the peak position parameter for background peak N, N is the index starting at 1 |
|Background.Peak.Parameter.N | floating point number | initial value for Nth parameter of the background peak(s), this value is only used if the limits are also set |
|Background.Peak.Parameter.N.Low | floating point number | lower limit for Nth parameter of the background peak(s), if this is the same as the upper limit, the parameter is fixed to this value |
|Background.Peak.Parameter.N.High| floating point number | upper limit for Nth parameter of the background peak(s), if this is the same as the lower limit, the parameter is fixed to this value |

If the peak fitted in the projection is very close in energy to the gate itself, it might be necessary to specify an additional background peak only for the mixed event spectra. These spectra are by the nature not corrected for time-random events (all events in these spectra are time random), and thus self-coincidences are possible (and indeed expected).

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|Mixed.Background.Peak.Position | positive floating point number | position of background peak that is included only in the fit of the mixed event projection but whose area is ignored |
|Mixed.Background.Peak.Low | positive floating point number | the lower limit for the peak position parameter for mixed event background peak |
|Mixed.Background.Peak.High | positive floating point number | the upper limit for the peak position parameter for mixed event background peak |

Finally it is possible to set the parameters (with limits or fixed) for the background function of the fit using these user settings:

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|Background.Offset(.Low/.High) | floating point number | value, lower, and upper limit for the constant offset in the global background of the fit |
|Background.Linear(.Low/.High) | floating point number | value, lower, and upper limit for the linear factor in the global background of the fit |
|Background.Quadratic(.Low/.High) | floating point number | value, lower, and upper limit for the quadratic factor in the global background of the fit |
|Background.Xoffset(.Low/.High) | floating point number | value, lower, and upper limit for the offset in x used in the global background of the fit |

If specific data points should be excluded, the user setting “ExcludedAngles” can be used, which takes a vector of floating point numbers (angles to be excluded). Each angle listed in this vector will be completely excluded, i.e. we won’t even create the projections for this angle.

The created angular distributions can be compared to simulated angular distributions.
To do so, the user has to provided the path to a .root file via the `theory` flag or the “Theory” user setting.
That .root file is expected to contain three `TGraphErrors` called "graph000", "graph010", and "graph100", which are the simulated "Z<sub>0</sub>", "Z<sub>2</sub>", and "Z<sub>4</sub>" distributions as described in method 2 of the paper by Smith et al.
If these are provided, the following parameters are being used:

| Parameter name | possible values | comments |
|----------------|-----------------|----------|
|TwoJ.Low        | comma separated integers | two times J for the lowest state involved in the cascade, this has to be a vector, even if it has only one entry that entry must be followed by a comma |
|TwoJ.Middle     | comma separated integers | two times J for the middle state involved in the cascade, this has to be a vector, even if it has only one entry that entry must be followed by a comma |
|TwoJ.High       | comma separated integers | two times J for the highest state involved in the cascade, this has to be a vector, even if it has only one entry that entry must be followed by a comma |
|MixingAngle.1   | floating point number | the mixing angle of the high -> middle transition as atan(&delta;) (range -&pi;/2; to &pi;/2) |
|MixingAngle.2   | floating point number | the mixing angle of the middle -> low transition as atan(&delta;) (range -&pi;/2 to &pi;/2) |
|ConfidenceLevel | positive floating point number | the value for the 99% confidence level that the reduced &chi;<sup>2</sup> is smaller than this (depends on the degrees of freedom) |

The output of the program is written to a file called “AngularCorrelations.root”, which can be changed using the user setting “Output” or the command line flag `--output` (takes a string).

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
If your data have poor enough statistics that you don't necessarily need simulations, you'll at least need some attenuation factors, which can be found [here](https://smithjk.github.io/AngularCorrelationCoefficients/index.html) for the 16-clover setup.

