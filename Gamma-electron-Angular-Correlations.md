# PACES

The Pentagonal Array for Conversion Electron Spectroscopy (PACES) at TRIUMF is an ancillary detection system used for in-vacuum high-resolution conversion electron spectroscopy. PACES is comprised of 5 lithium-drifted silicon (Si(Li)) detectors.

# GRIFFIN-PACES angles

The installation of PACES necessitates the removal of one GRIFFIN HPGe clover (position 13 in the upstream lampshade) such that the maximum number of coincidence angles between GRIFFIN and PACES detectors is **300** (60 HPGe crystals x 5 Si(Li)). In the GRIFFIN co-ordinate system, the PACES _Theta_ and _Phi_ co-ordinates are as follows, with a diagram of PACES included for clarity (beam travels out of the page in this view). 

[[https://github.com/MikeBowry/WikiPics/blob/master/thetaphi.PNG|alt=thetaphi]]
[[https://github.com/MikeBowry/WikiPics/blob/master/paces_schematic.png|alt=schematic]]

In the analysis of gamma-electron angular correlations, it is essential that both detectors share the same co-ordinate system. GRSISort contains co-ordinates of all HPGe crystals in `TGriffin.cxx` so that the relative angles can be extracted via `TVector`. Similarly the PACES co-ordinates can be defined in `TPaces.cxx`.

[[https://github.com/MikeBowry/WikiPics/blob/master/TPaces_angles.png|alt=TPaces]]

Note that GRSISort will need to recompiled after this modification. The angle between GRIFFIN and PACES detectors can now be found simply:

`double angle = TGriffin::GetPosition(gd, gc, distance).Angle(TPaces::GetPosition(p))*180./TMath::Pi();`,

where `gd` is the GRIFFIN HPGe clover position (1-16), `gc` is the HPGe crystal number (0-3) and `p` is the PACES detector number (0-4). The distance of the HPGe clovers from the source, `distance` is 110 or 145 mm (the latter used for full Compton suppression). A histogram of calculated angles is included here: the number of angles per bin is 4.

[[https://github.com/MikeBowry/WikiPics/blob/master/paces_coinc_angles.png|alt=paces_coinc_angles]]

# General procedure

Examples are provided on how to incorporate the GRIFFIN-PACES angles into a data processing script. Here, _GRSIProof_ is used to process the `AnalysisTree` which includes time-correlated GRIFFIN and PACES events. The procedure is outlined as follows:

1. A map of coincidence angles is constructed and grouped into a given number of bins.
2. Relative angle of GRIFFIN-PACES detector hits is found event-by-event.
3. Depending on the angle, a different electron-gamma matrix is filled.
4. Final electron-gamma matrices are processed to extract the angular correlation.

In practice, calculating the angle for each correlation is not necessary. Instead each detector pair is assigned an **index** based on the following formula:

`double index = (64*p)+(((gd-1)*4))+gc`,

where `index` takes the values 0-319. Due to the missing HPGe clover (position 13), index values from 48-51, 112-115, 176-179, 240-243 and 304-307 are not used. Indexes may be grouped together on the basis of their corresponding angle, forming angle bins. This is typically 20-30 bins but may vary depending on the analysis. Grouping is useful as it results in a more managable number of coincidence spectra to analyze, as well as summing together statistics. Consider the coincidence angles available for a single PACES detector (e.g. #2 at _phi_=94 degrees). The index map (grouped into bins of 2-3 angles) is as follows:

`double group[nbin][w]={{90,85,-1},{86,118,91},{119,84,88},{87,94,-1},{117,81,116},{95,65,93},{125,80,66},{82,92,70},{124,64,126},{83,122,67},{98,71,121},{69,127,109},{99,123,97},{68,108,120},{110,77,96},{111,102,105},{101,78,106},{79,103,75},{104,74,-1},{100,72,107}};`,

where `nbin`=20, `w`=3 and `-1` is a dummy variable/placeholder. More complex maps can be constructed if including more PACES detectors. In general, the number of angles per bin (the _weight_) should be managed so that the difference in weight between each bin is minimised. This helps reduce the scatter observed in the final angular correlation. However, the relative geometry of the GRIFFIN-PACES detector systems results in an angular coverage strongly peaked around 90 degrees and this enhances the variation. 

## Constructing the angle map (header)

The angle map can be included in the header file (`.h`) of the GRSIProof Selector. For example, 

`std::vector<std::pair<double, double>> PACESAngleCombinations(){`
`double group[nbin][w]={...};`
`std::vector<std::pair<double, double>> result;`
`      for(int i=0;i<int(nbin);++i){`
`         for(int j=0;j<int(w);++j){`
`	    if(group[i][j]<0) continue;`
`	    result.push_back(std::make_pair(group[i][j], double(i)));`
`         }`
`      }`   
`   return result; }`

In `..public TGRSISelector{` define:

`std::vector<std::pair<double, double>> fPACESAngleCombinations;`
`std::map<double, double> fPACESAngleMap;`
`int maxanglebin;`

and in the nested loop `EventSelector(TTree * /*tree*/ =0) : TGRSISelector(), fGrif(0), fPaces(0) {`, define:

`for(int i = 0; i < static_cast<int>(fPACESAngleCombinations.size()); ++i) {`
`   if(fPACESAngleCombinations[i].second>0) maxanglebin = fPACESAngleCombinations[i].second;`
`   fPACESAngleMap.insert(std::make_pair(fPACESAngleCombinations[i].first, fPACESAngleCombinations[i].second));`
`}`

Tip: remember to define the `PACESAngleCombinations` vector at the beginning of the header file.

## Constructing the angle map (main)

In the main PROOF program (`.C`) we can now refer to `PACESAngleMap` to determine the angle bin to which each detector pair belongs. In `FillHistograms() {`,

`double index = (64*p)+(((gd-1)*4)+gc);`
`auto angle_bin = fPACESAngleMap.lower_bound(index-0.5);`
`int whichindex = int(angle_bin->first);`
`int whichbin = int(angle_bin->second);`

The `PACESAngleMap` **key** (by default the key is the first column of the array) is queried to find the bin number corresponding to the index. In this example, the map using a single PACES detector would return bin **2** for indexes 84, 88 and 119. The variables `whichindex` and `whichbin` can be used as diagnostics to check the map is functioning properly. Examples are shown here using only PACES detector #2 with no grouping applied.

[[https://github.com/MikeBowry/WikiPics/blob/master/index.png|alt=index]]
[[https://github.com/MikeBowry/WikiPics/blob/master/bin.png|alt=bin]]

# Electron-gamma coincidence matrices

As a minimum requirement, two versions of the electron-gamma matrix should be produced by PROOF for each angle bin corresponding to `time-correlated' and `time-random' coincidence events, defined by appropriate cuts on the electron-gamma time-difference spectrum. The time-random matrix is scaled by a factor equal to the ratio of the time-correlated and time-random cuts, then subtracted from the time-correlated matrix. 

A projection of the (time-random background-subtracted) time-correlated matrix is produced by gating on the ICE or gamma-peak of interest. A second projection gated on the Compton background near the peak must be subtracted from the first projection. This subtraction and (importantly) the scaling of the Compton background are handled automatically by the **jROOT** and **TBGSubtraction()** environments available via GitHub (J.Smallcombe) and GRSISort respectively. It is recommended to use either of these applications when performing this step. The peak areas extracted from the appropriately-subtracted projections form the basis of the angular correlation measurement.

# Normalization
# Final gamma-electron angular correlation