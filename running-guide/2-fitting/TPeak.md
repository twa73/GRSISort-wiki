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



