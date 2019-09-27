# BEP001 examples

This branch of the BEP001 repository contains some practical examples of what a BIDS-compatible qMRI dataset will look like. 

You can find more about the BEP001 addition to the BIDS standard, and the rationale for the additions/changes that it brings to the BIDS format, here: https://github.com/bids-standard/bep001.

 *The datasets in this branch contain empty data files, which might be useful for building simple software tests. 
 The current branch may eventually be merged with the bids-examples branch of the main bids-standard repo: https://github.com/bids-standard/bids-examples, in which case this disclaimer will be redundant.*


## Example 1: MEGRE, multi-echo
This is a sample dataset from a single MEGRE sequence, which yields multiple images, corresponding to different echo times. This dataset has been used to calculate a quantitative susceptibility map (QSM), as well as a T2* map.

## Example 2: MP2RAGE, single echo
This is a sample dataset from a standard MP2RAGE sequence, which includes two GRE images acquired after a single inversion pulse. This data have been used to create a T1-map, as well as a T1-weighted image, corrected for B0 and B1-inhomogeneities, which are both part of the example dataset.

## Example 3: MP2RAGE, multi-echo 
This is a sample dataset from an MP2RAGE sequence with two inversion times, and, for the second inversion time, 4 different echoes.

## Example 4: MultiParameter Mapping (MPM) dataset
This is a sample dataset from a MultiParameter Mapping (MPM) sequence (as per Weiskopf et al., 2013), which yields multi-echo FLASH scans that are predominantly T1-, PD-, or MT-weighted by changing repetition time and flip angle.
Note that the raw data for these scans are not called "FLASH" (or MEGRE, following our recommended vendor-neutral notation) in their suffices, rather they are grouped together with the "MPM" suffix and the "acq" tag is used to clarify which weightings the scans are highlighting.
