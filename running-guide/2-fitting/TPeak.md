<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**ANALYSIS**](Analysis) > [**FITTING**](Fitting) > [**TPEAK**](TPeak)

TPeaks are used to fit photopeaks to a Gaussian + skewed gaussian. The background includes a step function. TPeaks inherit from TF1, which means they can be named, and can be written to TTrees and TFiles.

[[/images/TPeakExample.png]]

-----------------------------------------
How To Use
-----------------------------------------
TPeaks can be created as follows:
```c++
TPeak *peak = new TPeak(centroid, low_range, high_range);
```
To fit the TPeak, call:
```c++
peak->Fit(histogram);
```
where `histogram` is the `TH1*` attempting to be fit. You can also use the same options as when fitting a TF1. For example,
```c++
peak->Fit(histogram,"+");
```
to add the TPeak to the histogram.

Once the TPeak is fitted, you can call 
```c++
peak->Print();
```
To see the basic properties of the TPeak
```
   Name:        Chan1173_1160_to_1190
   Centroid:    1173.172270 +/- 0.005742
   Area:          1885626.302501 +/- 1409.848616
```
Or, to see more properties, such as the value and uncertainty in each parameter, use the option `Print("+")`.

## Fitting Technique
One should always fit peaks using the proper statistical model. Gamma-rays are detected following a Poisson probability distribution. Therefore, when fitting a standard gamma ray spectrum, one should fit the peak using a *maximum likelihood distribution*. If background is subtracted, one is left with the difference of two Poisson distributed data sets, which is no longer a Poisson distributed data set. Thus, one would prefer to use least-squares fitting in this scenario.
TPeak accommodates for this using a default maximum-likelihood fitting flag. To see what the current status of the flag is use `TPeak::GetLogLikelihoodFlag();`. To set the status of the flag, use `TPeak::SetLogLikelihoodFlag(true);` to turn maximum likelihood on and `TPeak::SetLogLikelihoodFlag(false);` to turn change it to least-squares fitting. It defaults to on.

## Parameters
The first time a TPeak is fit, the parameters are initialized based on the histogram passed to the `TPeak::Fit` function. 
When a TPeak is created, the parameters are uninitialized. The names of the paramters are as follows:

- "Height": The height of the unskewed gaussian
- "centroid": The centroid of the unskewed gaussian
- "sigma": The width of the unskewed gaussian
- "beta": The "skewedness" of the skewed gaussian
- "R": The relative height of the skewed gaussian
- "step": The size of the step function in the background
- "A": The constant part of the background
- "B": The linear part of the background
- "C": The quadratic part of the background
- "bg_offset": The offset in the background in the x direction

To fix a particular component of the TPeak, for example, the width to 1.0, use:
```c++
peak->FixParameter(GetParNumber("sigma"),1.00);
```
To Release the paramater use:
```c++
peak->ReleaseParameter(GetParNumber("sigma"));
```
and to set the paramater to an initial value use:
```c++
peak->SetParameter("sigma",1.00);
```
which maintains its fixed/freeness.


