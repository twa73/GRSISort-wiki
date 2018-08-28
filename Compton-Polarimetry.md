
## Process
Here are the steps to follow for Compton Polarimetry analysis in GRSISort.

1): Use GRSISort to produce an analysis tree for each subrun that have calibrated energies.

2): Make the histograms for Compton Polarimetry using the LeanComptonMatrices script. There will be one file for each subrun. Example command:

 `for i in {001..012}; do LeanComptonMatrices analysis02340_${i}.root; done`

3): Combine all subrun histrogram files into a single file using hadd.

`hadd CompHists_02340_001-015.root CompHists_02340_001.root CompHists_02340_002.root CompHists_02340_003.root CompHists_02340_004.root CompHists_02340_005.root CompHists_02340_006.root CompHists_02340_007.root CompHists_02340_007.root CompHists_02340_008.root CompHists_02340_009.root CompHists_02340_010.root CompHists_02340_011.root CompHists_02340_012.root`

4): Make the final Compton Polarimetry Analysis plots using the LeanAnalysisComptonMatrices script.

`LeanAnalyzeComptonMatrices CompHists_02340_001-015.root`

5): The main results will be printed to the terminal and found in the plots AsymmetryBinFoldNonCo, CHISquared_BinFoldNC in the final file `CompPlots_02340.root`.

## Scripts
The two scripts, LeanComptonMatrices and LeanAnalyzeComptonMatrices, are located in the ~/packages/GRSISort/scripts folder. To compile changes to these scripts execute the `make` command in the ~/packages/GRSISort folder.

### LeanComptonMatrices
The purpose of this script is to produce the coincidence histograms which are used for the Compton Polarimetry analysis. The parameters to change for each case:

-output filename and path (yes they are hardcoded), ~line 112 and 127.

-The energies of the cascade gamma rays, ~line 186. `double gEnergy[2] = { 1173.2 , 1332.5 }; // For scatter of 1173keV, 60Co` 

-Any energies to be excluded (this crude method should be improved), ~line 380. `if(grif->GetGriffinHit(two)->GetEnergy() > 505 && grif->GetGriffinHit(two)->GetEnergy() < 517 ) continue;   // 511 keV from single-escape`

### LeanAnalyzeComptonMatrices
The purpose of this script is to perform the Compton Polarimetry analysis on the histograms. The parameters to change for each case:

-The run number being sorted (used for CompPlots filename), ~line 74. `#define RUNNUMBER 2340`

-The quality factor for this setup, `#define AGATA_Q_Folded 0.2399` and `#define E_Q_Measure 1332`, as well as the energy of the gamma ray that is scattering `#define E_Q_Use 2013`. ~lines 89-91.

-The parameters defined in the double PolarizationCalculation() function which are specific to the cascade under study, ~line 682-691. The origin of the F_JiJx and F_JfJx coefficients are the tabulations of RS. Hager and EC Seltzer, Nuclear Data Sheets 4, 397 (1968).