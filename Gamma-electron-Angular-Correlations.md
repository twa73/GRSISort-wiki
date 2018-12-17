# Introduction

Internal conversion electrons (ICEs) compete with gamma-decay to remove energy from excited nuclear states. In particular this decay mode is favoured in cases where the atomic number and transition multipolarity are high, and where the transition energy is low (or combinations of all three). The ICE energy is equal to the difference between the transition energy and the atomic binding energy of a given electronic orbital. The _K_ shell (followed by _L1-3_, _M1-5_ etc.) possesses the highest binding energy and is also the shell from which the electron is most likely to be removed. 

# PACES

The Pentagonal Array for Conversion Electron Spectroscopy (PACES) at TRIUMF is an ancillary detection system used for high-resolution conversion electron spectroscopy. PACES is comprised of 5 individual lithium-drifted silicon (Si(Li)) detectors arranged within a Delrin sphere and a cold finger providing thermal contact with liquid nitrogen enclosed in an insulated dewar. It is installed at the GRIFFIN facility facing downstream, forming the upstream portion of the vacuum chamber.

# GRIFFIN-PACES angles

The installation of PACES necessitates the removal of one GRIFFIN HPGe clover (position 13 in the upstream lampshade) such that the maximum number of coincidence angles between GRIFFIN and PACES detectors is **300** (60 HPGe crystals x 5 Si(Li)). In the GRIFFIN co-ordinate system, the PACES _Theta_, _Phi_ co-ordinates are as follows:

_angle-table-here_

In the analysis of gamma-electron angular correlations, it is essential that both detectors share the same co-ordinate system. GRSISort contains co-ordinates of all HPGe crystals in `TGriffin.cxx` so that the relative angles can be extracted via `TVector`. Similarly the PACES co-ordinates can be defined in `TPaces.cxx`.

[[https://github.com/MikeBowry/WikiPics/blob/master/TPaces_angles.png|alt=TPaces]]

Note that GRSISort will need to recompiled after this modification. The angle between GRIFFIN and PACES detectors can now be found simply:

`double angle = TGriffin::GetPosition(gd, gc, distance).Angle(TPaces::GetPosition(p))*180./TMath::Pi();`,

where `gd` is the GRIFFIN HPGe clover position (1-16), `gc` is the HPGe crystal number (0-3) and `p` is the PACES detector number (0-4). The distance of the HPGe clovers from the source, `distance` is 110 or 145 mm (the latter used in for full Compton suppression). 

