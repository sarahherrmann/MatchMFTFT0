# MatchMFTFT0
Codes using the matching information of MFT tracks to FT0-C signals.
These codes need to be added to O2Physics to work.

Codes for MFT-FT0C matching: available in O2Physics/Common if the PR https://github.com/AliceO2Group/O2Physics/pull/7584 is merged
The source code: match-mft-ft0.cxx
-->For each MFT tracks it finds the interval of BCs of the corresponding MFT ROF
using the method getCompatibleBCs (the method gets you a slice of BCs compatible to one MFT track)
-->It then does propagation of the MFT track to the FT0-C plane and matching
-->Finally it fills a table containing for each MFT track a list of BCs in which there is a match in FT0-C

The dataformat code for the table is in MatchMFTFT0.h


Additionnal codes to study the matching of MFT to forward tracks:

A code to build a table linking one MFT track to its matched forward track: mft2fwtracks.cxx
Indeed, in AO2D tables there is a link from muons to MFT track, but not from one MFT track to its matched muon track, thus this code.
The definition of the table is in FwdIndex.h

The analysis code allowing to derive histograms is nch-mft.cxx
This code contains a lot of things but one can look only at the method processMuonsAndMFTFT0 for starters

At the time I used these codes I put them in PWGMM and used this command line:


o2-analysis-timestamp -b --configuration json://configuration.json | o2-analysis-event-selection -b --configuration json://configuration.json | o2-analysis-mm-nch-mft -b --configuration json://configuration.json | o2-analysis-track-propagation -b --configuration json://configuration.json | o2-analysis-mm-mft-to-fwd -b --configuration json://configuration.json | o2-analysis-mm-track-propagation -b --configuration json://configuration.json | o2-analysis-fwdtrack-to-collision-associator -b --configuration json://configuration.json --nSigmaForTimeCompat 1 --aod-file AO2D.root

Depending on the processes the user wants to run, one also need to add the o2-analysis-match-mft-ft0 workflow to the command line.

The file configuration.json must be changed to have the default parameters in some tasks (ask me if needed), the one available here is a bit obsolete
