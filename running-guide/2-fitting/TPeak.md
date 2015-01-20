<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**FITTING**](Fitting) > [**TPEAK**](TPeak)

TPeaks are used to fit photopeaks to a Gaussian + skewed gaussian. The background includes a step function. TPeaks inherit from TF1, which means the can be named, and can be written to TTrees and TFiles.

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
where `histogram` is the TH1 attempting to be fit.
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
Or, to see more properties, such as the value and uncertainty in each parameter, use the option "+".

##Parameters
The first time a TPeak is fit, the parameters are initialized based on the histogram passed to the `TPeak::Fit` function. 
When a TPeak is created, the parameters are uninitialized. The names of the paramters are as follows:

- "Height": The height of the unskewed gaussian
- "centroid": The centroid of the unskewed gaussian
- "sigma": The width of the unskewed gaussian
- "beta": The "skewedness" of the skewed gaussian
- "R": The relative height of the skewed gaussian
- "step": The size of the step function in the baskground
- "A": The constant part of the background
- "B": The linear part of the background
- "C": The quadratic part of the background
- "bg_offset": The offset in the background in the x direction

To fix a particular component of the TPeak, for example, the width to 1.0, use:
```c++
peak->FixParameter("sigma",1.00);
```
or to fix it to its current value
```c++
peak->FixParamter("sigma");
```
To Release the paramter use:
```c++
peak->ReleaseParameter("sigma");
```
and to set the paramater to an initial value use:
```c++
peak->SetParameter("sigma",1.00);
```
which maintains its fixed/freeness.


