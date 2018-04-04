Below are the steps involved in using the GRSISort crosstalk correction codes

1. run grsiproof (Crosstalk.C) on analysistrees
   - det_x_x_x shows matrix of all events
   - from here found 'good' peak and see threshold-gate size of appropriate peaks
2. run 'GriffinCTFix matrixfile.root' (GriffinCTFix.cxx) on the matrix file outputed by the first code--this 
   produces a calibration file ("ct_corrections.cal")
   -important things in this code are: line 60 (gate), line 102 (threshold), line 198 (energy to be used)
3. apply calibration to fragment or analysis trees