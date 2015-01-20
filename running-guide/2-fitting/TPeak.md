<a name="top" />

[**HOME**](Home) > [**RUNNING GRSISORT**](Running-GRSISort) > [**FITTING**](Fitting) > [**TPEAK**](TPeak)

TPeaks are used to fit photopeaks to a Gaussian + skewed gaussian. The background includes a step function.

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

