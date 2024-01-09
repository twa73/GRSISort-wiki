Below are the steps involved in using the GRSISort crosstalk correction codes

1. run grsiframe (CrosstalkHelper.cxx) or grsiproof (Crosstalk.C) on analysistrees
   - det_x_x_x shows matrix of all events
   - from here found 'good' peak and see threshold-gate size of appropriate peaks
2. run 'GriffinCTFix matrixfile.root' (GriffinCTFix.cxx) on the matrix file outputed by the first code
   - this produces a calibration file ("ct_corrections.cal")
   - important things in this code are: line 60 (gate), line 102 (threshold), line 198 (energy to be used)
3. apply calibration to fragment or analysis trees

Make sure that the TAnalysisOptions have the flag set to correct cross talk!
This can be done during sorting by adding the flag ```--is-correcting-cross-talk 1```, or by reading the analysis options from a root-file opened in update mode, setting the flag via the SetCorrectCrossTalk(true) function, and then writing the analysis options back to file.
When opening a root-file with grsisort you will get a print out of the analysis options and should see a line ```fIsCorrectingCrossTalk: 1``` if the flag has been set.

Check the GRSISort discussion #1236 to see how working crosstalk corrections should work.

