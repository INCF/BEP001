# Proposed changes to BIDS specification

This document captures all of the changes that the BEP001 team are proposing to the BIDS specification.

Table of contents:

* [Repetition Time](#repetition-time)

## Repetition Time

### Proposed Change

Adjust the definition of `RepetitionTime` in section [4.1.x Task (including resting state) imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) and add two new fields to section [4.1.y Anatomy imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data).

### Justification

`RepetitionTime` is currently defined very specifically as relating to functional imaging data.
However there are structural scans that collect multiple volumes during an acquisition.
Here we adjust the definition of `RepetitionTime` in section 4.1.x and add `RepetitionTimeExcitation` and `RepetitionTimePreparation` as two additional terms for structural acquisitions that include multiple contrasts in 4.1.y.

* [B1plus fieldmaps](#b1plus-fieldmaps)

## Repetition Time

### Proposed Change

Extend the part on `Fieldmaps` in [4.1.x Task (including resting state) imaging data](https://github.com/bids-standard/bids-specification/blob/master/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) by also allowing
for storing B1+ fielmaps.

### Justification

For some anatomical MRI acquisitions, especially when doing quantiative MRI (qMRI), B1+ fieldmaps can be useful to get better estimates of the underlying physical parameters (e.g., T1 in T1 maps obtained with MP2RAGE-sequence, see Marques et al., 2013).

## Suffix 

### Proposed Change


### Justification 

Unlike other key labels that exist in the main specification (e.g. `task`), a designated key label (e.g. `modality_name` or `sequence_name`) can not semantically hold true for all the values that it may list for multiple quantitative MRI (qMRI) techniques. This is because qMRI methods can be addressed with respect to the following:
 * sequence names (MP2RAGE)
 * generated outputs (MTR, MTsat) 
 * developer-defined acronyms (DESPOT1, hMRI,etc). 
  
As a result, semantic relevance of any specific key label can get easily overloaded. We therefore suggest the use of the `_suffix`, as it is free from a designated key label. Nevertheless, this approach calls for a systematically created definition list of the `_suffix` entries. Otherwise, the approach becomes prone to the violation of one-to-one referral principle of the `_suffix` field. On the other hand, unorganized profileration of `_suffix` entries would eventually yield a noisy and unadaptable list of entries. 

Based on the reasons given above, to maintain a viable and well-balanced list of suffixes, BEP001 classify suffixes in three categories:

1. `Grouping suffixes` (`G`)
     * **Role:** Groups together files that belong to parametrically linked multiple scans intended for a well-defined qMRI application (e.g. `MPM`, `VFA`).

2. `Designation suffixes for qMRI maps` (`M`) 
     * **Role:** Denotes the parameter contained within a single file of a quantitative map (e.g. `T1map`,`MTsat`).  

3. `Suffixes for conventional MRI contrasts` (`W`)
     * **Role:** Denotes the type of the predominant contrast conveyed by a single file of a conventional anatomical image (e.g. `T1w`, `T2w`, `PDw`, `T2starw`). 

#### An important note on minimizing the use of weight tags for grouping qMRI inputs

Altering some acquisition parameters while keeping the remaining constant across multiple repetitions of the same sequence is a mainstream approach to data collection for quantitative mapping. As a matter of course, such intentional modifications on the acquisition parameters yield multiple images with "relatively different contrasts". Within the scope of a single qMRI application,distinguishing these images using conventional weight tags (i.e. `T1w`, `PDw` and `T2w`) emerges as an intuitive solution. However, outside the scope of that qMRI application, interchangeability of the images which are designated by conventional weight tags becomes questionable. 

Assume that `PDw` and `T1w` labels were assigned to the volumes acquired with a GRE sequence at long TR and different flip angles for a hypothetical qMRI application of `qFoo`. Also assume that the TR is long enough, so that both images have a quite similar T1 contrast. Even though these labels would be somewhat acceptible for the sake of distinguishing them from each other due to the difference in flip angles, the `PDw` label would no longer be tenable for the interchangeability of `PDw` images outside the scope of the `qFoo` protocol. 

Similarly, the description of the magnetization transfer saturation index map (`MTsat`) inputs as grouped by the `MTS` suffix constitutes a good real-world example from the curent proposal (please see the list of available suffixes). The method involves acquisition of three volumes, where files are named by `MTw`, `PDw` and `T1w` tags _by tradition_. The `MTw` refers to the volume acquired by playing magnetization transfer pulse as a preperation step, all the remaining parameters of `MTw` ideally match that of `PDw`. When compared side by side, the contrast distinction implied by the `PDw` and `T1w` labels is apprehensible. Given that these two volumes are acquired using a GRE sequence and with the same TR, the one with the higher flip angle is expected to attain a higher T1 contribution. However, outside the scope of the `MTsat` protocol, demarcation of `PDw` vs `T1w` for the identical set of sequence and parameters can easily become a subjective matter. Therefore, to avoid potentially unrepresentative use of the weight tag of `PDw`, following solution is provided: 

* Group input files needed to compute an `MTsat` map by the `MTS` grouping suffix.
* Change `MTw` tag to `MTon` and `PDw` tag to `MToff` to better denotate the relationship between these images.  
* Use `MTon`, `MToff` and `T1w` tags as values in the `acq-<label>` key-value pair to distinguish `MTS` files from each other.

```
sub-01_MTS.json [*]
sub-01_acq-MTon_MTS.nii.gz 
sub-01_acq-MTon_MTS.json [**]
sub-01_acq-MToff_MTS.nii.gz 
sub-01_acq-MToff_MTS.json [**]
sub-01_acq-T1w_MTS.nii.gz 
sub-01_acq-T1w_MTS.json [**]

* JSON file that contains non-varying metedata fields betweeen different `MTS` files. 
** JSON file that contains all the varying metadata fields, corresponding to the companion nifti file. 
```

In conclusion, weight tags denote the type of predominant contrast conveyed by a single file of a conventional anatomical image ONLY when used as a suffix. In all other cases,  the `acq-<label>` key-value pair can be still used to accodomate legacy naming conventions for the sake of accustomed familiarity, yet without interfering with the interchangeability principle. Nevertheless, minimizing the use of weight tags is highly encouraged to avoid confusion.  

>  Answers to the plausible questions:

Q1 - For the `MTS` example given above, why two `<indexable_metadata>-<index>` key-value pairs of `fa` and (a new key tag of) `mt` are not used to distinguish `MTS` images from each other? 
* A-1. `<indexable_metadata>-<index>` is applicable when the variable entries are _enumerable_. Whereas `mt` would attain  _categorical_ entries. Therefore, `mt` cannot be defined as a new key tag in the list of allowed key tags.

Q2 - For the `MTS` example given above, the list of `acq-<label>` key-value pairs would only define `MTon` and `MToff` instead of including `T1w`, which then could be used in conjunction with the `<indexable_metadata>-<index>` key-value pair of `fa`. Why deviate from a more weight tag agnostic convention?

* A-2. Although this is a legitimate solution, it was _not preferred_ to keep convention somewhat closer to the accustomed format of `MTw`-`PDw`-`T1w` and to collapse two varying parameters (flip angle and MT) into one variable, so that the file list becomes easier to read for the multi-echo derivatives of the `MTS` method (e.g. `MPM`).              

#### Principles for adding a new `_suffix` entry

Updates to the specification is REQUIRED to extend the list of available suffixes. A new entry request or any suggestion to modify existing entries can be made by opening a GitHub issue on [BEP001 repository](https://github.com/bids-standard/bep001). Following principles MUST be respected:  

* List of available suffixes is a list of unique entries. Thus, more than one description for a single `_suffix` is not allowed. 
* Every suffix MUST belong to one and only one of the three suffix classes of **i)** `grouping suffixes (A)`, **ii)** `designation suffixes for qMRI maps (M)` and **iii)** `suffixes for conventional MRI contrasts (W)`.
* Corresponding suffix class (`A`, `M` or `W`) must be denoted for every entry of the list of available suffixes. 

*** 

* `Grouping suffixes` MUST attain a clear description of the qMRI application that they relate to. If available, hyperlinks to example applications and/or more detailed descriptions are encouraged.
* If there exist an `grouping suffix` which relates to one or many `designation suffix for qMRI map`, all the relevances MUST be indicated in the description of the `grouping suffix` by the _"Associated output suffixes: "_ expression.
* Unless the pulse sequence is exclusively associated with a specific qMRI application (e.g. `MP2RAGE`), sequence names are NOT used as `grouping suffixes`.
* `Grouping suffixes` MUST be used in conjuction with at least one of the `<indexable_metadata>-<index>` and `acq-<label>` key-value pairs. If existing `<indexable_metadata>-<index>` and `acq-<label>` key-value pairs are not enough to describe a new qMRI method, additional request is needed to extend those lists.     

*** 

* `Designation suffixes for qMRI maps` MUST attain a clear description of the parameter that they contain, **including the units**. If available, hyperlinks to example applications and/or more detailed descriptions are highly encouraged.

* If there exist a `designation suffix for qMRI maps` that relates to one or many `grouping suffix`, all the relevances MUST be indicated in the description of the `designation suffix for qMRI map` by the _"Can be generated by: "_ expression.

*** 

* The list of available `suffixes for conventional MRI contrasts` is immutable: `T1w`,`T2w`,`PDw`,`T2starw`. 

***


## Indexable Metadata 

### Proposed Change 

### Justification 

## Sidecar JSON files of qMRI maps

Which metadata fields must be included. Some of these fields are to be inherited by the input files and some of them are to be populated by the estimation software. Right now: 

* BasedOn --> List of files gruoped by an grouping suffix + (optional) field maps. 
* MagneticFieldStrength
* Manufacturer
* ManufacturerModelName
* InstitutionName 
* PulseSequenceType
* PulseSequenceDetails
* EstimationPaper
* EstimationAlgorithm
* EstimationSoftwareName
* EstimationSoftwareVersion 
* EstimationSoftwareLanguage
* EstimationSoftwareEnv 

## Sidecar JSON files of grouping suffixes 

Mention about root JSON + repeating JSON file convention.
Which root JSON metadata fields are optional/required. 


## Legacy suffixes for anatomical data
### Proposed Change 
Moves some suffixes for `anatomical data` to a new table called "LEGACY suffixes". These suffixes are still valid BIDS format,
but we reccomend not to use them anymore. They are ambiguous and/or inconsistent.

### Justification 
### Justification 
The following suffixes are moved to the legacy table:
 * **`_T2`\***: what is meant is T2\*-_weighted_ data. To be consistent with the suffixes `T1w`, `T2w` and `PDw`, we want
to use the suffix `_T2starw` for this.
 * **`_FLAIR`**: This is a sequence name rather than a contrast or a way of grouping quantitative maps. We prefer to use
`_T2w`, `_PDw`, or `_MESE` depending on the use case.
 * **`_FLASH`**: This is a sequence name rather than a contrast or a way of grouping quantitative maps. We prefer to use
`_T2starw`, `_PDw`, or `_GREME` depending on the use case.
 * **`_PD`**: what is meant is PD*-_weighted_ data. To be consistent with the suffixes `T1w`, `T2w` and `T2starw`, we want
to use the suffix `_PDw` for this.
 * **`_PDT2`**: We recommend to use `PDT2map` for this.
 * **`_inplaneT1`**: this is not a proper MRI contrast or grouping variable. The fact that the data is acquired with the
same prescription as the functional data is already decsribed in the orientation matrix. Furthermore,
it could be communicated via the `acq-`-tag.
 * **`_inplaneT2`**: this is not a proper MRI contrast or grouping variable. The fact that the data is acquired with the
same prescription as the functional data is already decsribed in the orientation matrix. Furthermore,
it could be communicated via the `acq-`-tag.
