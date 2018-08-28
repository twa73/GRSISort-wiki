Here are the steps to follow for Compton Polarimetry analysis in GRSISort.

1): Use GRSISort to produce an analysis tree for each subrun that have calibrated energies.

2): Make the histograms for Compton Polarimetry using the LeanComptonMatrices script. There will be one file for each subrun. Example command:

 `for i in {001..012}; do LeanComptonMatrices analysis02340_${i}.root; done`

3): Combine all subrun histrogram files into a single file using hadd.

`hadd CompHists_02340_001-015.root CompHists_02340_001.root CompHists_02340_002.root CompHists_02340_003.root CompHists_02340_004.root CompHists_02340_005.root CompHists_02340_006.root CompHists_02340_007.root CompHists_02340_007.root CompHists_02340_008.root CompHists_02340_009.root CompHists_02340_010.root CompHists_02340_011.root CompHists_02340_012.root`

4): Make the final Compton Polarimetry Analysis plots using the LeanAnalysisComptonMatrices script.

`LeanAnalyzeComptonMatrices CompHists_02340_001-015.root`

5): The main results will be printed to the terminal and found in the plots AsymmetryBinFoldNonCo, CHISquared_BinFoldNC in the final file `CompPlots_02340.root`.