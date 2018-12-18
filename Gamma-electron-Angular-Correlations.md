# Introduction

Internal conversion electrons (ICEs) compete with gamma-decay to remove energy from excited nuclear states. In particular this decay mode is favoured in cases where the atomic number and transition multipolarity are high, and where the transition energy is low (or combinations of all three). The ICE energy is equal to the difference between the transition energy and the atomic binding energy of a given electronic orbital. The _K_ shell (followed by _L1-3_, _M1-5_ etc.) possesses the highest binding energy and is also the shell from which the electron is most likely to be removed. 

# PACES

The Pentagonal Array for Conversion Electron Spectroscopy (PACES) at TRIUMF is an ancillary detection system used for high-resolution conversion electron spectroscopy. PACES is comprised of 5 individual lithium-drifted silicon (Si(Li)) detectors arranged within a Delrin sphere and a cold finger providing thermal contact with liquid nitrogen enclosed in an insulated dewar. It is installed at the GRIFFIN facility facing downstream, forming the upstream portion of the vacuum chamber.

# GRIFFIN-PACES angles

The installation of PACES necessitates the removal of one GRIFFIN HPGe clover (position 13 in the upstream lampshade) such that the maximum number of coincidence angles between GRIFFIN and PACES detectors is **300** (60 HPGe crystals x 5 Si(Li)). In the GRIFFIN co-ordinate system, the PACES _Theta_ and _Phi_ co-ordinates are as follows, with a diagram of PACES included for clarity (beam travels out of the page in this view). 

[[https://github.com/MikeBowry/WikiPics/blob/master/thetaphi.PNG|alt=thetaphi]]
[[https://github.com/MikeBowry/WikiPics/blob/master/paces_schematic.png|alt=schematic]]

In the analysis of gamma-electron angular correlations, it is essential that both detectors share the same co-ordinate system. GRSISort contains co-ordinates of all HPGe crystals in `TGriffin.cxx` so that the relative angles can be extracted via `TVector`. Similarly the PACES co-ordinates can be defined in `TPaces.cxx`.

[[https://github.com/MikeBowry/WikiPics/blob/master/TPaces_angles.png|alt=TPaces]]

Note that GRSISort will need to recompiled after this modification. The angle between GRIFFIN and PACES detectors can now be found simply:

`double angle = TGriffin::GetPosition(gd, gc, distance).Angle(TPaces::GetPosition(p))*180./TMath::Pi();`,

where `gd` is the GRIFFIN HPGe clover position (1-16), `gc` is the HPGe crystal number (0-3) and `p` is the PACES detector number (0-4). The distance of the HPGe clovers from the source, `distance` is 110 or 145 mm (the latter used in for full Compton suppression). A histogram of calculated angles is included here: the number of angles per bin is 4.

[[https://github.com/MikeBowry/WikiPics/blob/master/paces_coinc_angles.png|alt=paces_coinc_angles]]

# Constructing an angle map

Examples are provided on how to incorporate the GRIFFIN-PACES angles into a data processing script. Here, _GRSIProof_ is used to process the `AnalysisTree` which includes time-correlated GRIFFIN and PACES events. The general procedure is outlined as follows:

1. A map of all available coincidence angles is constructed and grouped appropriately into a given number of bins.
2. The angle between correlated GRIFFIN-PACES detector hits is calculated event-by-event using the AnalysisTree.
3. Depending on the angle, a different electron-gamma matrix is filled.
4. Final electron-gamma matrices are processed to extract the angular correlation.

In practice, calculating the angle for each correlation is not necessary. Instead each unique combination of detector pairs is assigned an **index** based on the following formula:

`double index = (64*p)+(((gd-1)*4))+gc`,

where `index` takes the values 0-319. Due to the missing HPGe clover (position 13), index values from 48-51, 112-115, 176-179, 240-243 and 304-307 are not used. As the angle corresponding to each index is known, indexes may be grouped together to form a manageable number of angle bins in the analysis. This is typically 20-30 bins. For example, consider the coincidence angles available for a single PACES detector (e.g. #2 at _phi_=94 degrees). The index map (grouped into bins of 3) is as follows:

`double group[n][3]={{90,85,-1},{86,118,91},{119,84,88},{87,94,-1},{117,81,116},{95,65,93},{125,80,66},{82,92,70},{124,64,126},{83,122,67},{98,71,121},{69,127,109},{99,123,97},{68,108,120},{110,77,96},{111,102,105},{101,78,106},{79,103,75},{104,74,-1},{100,72,107}};`

More complex maps can be constructed if including more PACES detectors. In general, the number of angles per bin (the _weight_) should be managed so that the difference in the weights between each bin is minimised. This reduces the statistical scatter observed in the final angular correlation. However, the relative geometry of the GRIFFIN-PACES detector systems results in an angular coverage strongly peaked around 90 degrees. This enhances the variation in weights from 0-180 degrees. 

The angle map can be included in the header file (`.h`) of the EventSelector and recalled in the main program (`.C`). 

# Electron-Gamma matrices
# Electron-Gamma angular correlation