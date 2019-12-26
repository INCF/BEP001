# Proposed changes to BIDS specification

This document captures all of the changes that the BEP001 team are proposing to the BIDS specification.

Table of contents:

* [Acquisition metadata field](#acquisition-metadata-field)
* [Part metadata field](#part-metadata-field)
* [Repetition time](#repetition-time)
* [Symbolic links](#symbolic-links)
* [Suffix](#suffix)
* [B1plus fieldmaps](#b1plus-fieldmaps)
* [Repetition Time](#repetition-time)
* [B1plus fieldmaps](#b1plus-fieldmaps)
* [S0map](#s0map)

## Acquisition metadata field

The `acq-<label>` key-value pair was added to capture a group of parametrically linked anatomical images.
This was updated in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#acq-label-key-value-pair`.

## Part metadata field

### Proposed change

Change in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#part-magphase-keyvalue-pair`.

The `part-<mag/phase>` key-value pair was added to distinguish the magnitude and phase parts of an acquisition.

We additionally recommend (but do not require) that phase images should be in radians and have a range of 0 (inclusive) to 2pi (not included).

## Repetition Time

### Proposed Change

Adjust the definition of `RepetitionTime` in section [4.1.x Task (including resting state) imaging data](/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) and add two new fields to section [4.1.y Anatomy imaging data](/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data).

### Justification

`RepetitionTime` is currently defined very specifically as relating to functional imaging data.
However there are structural scans that collect multiple volumes during an acquisition.
Here we adjust the definition of `RepetitionTime` in section 4.1.x and add `RepetitionTimeExcitation` and `RepetitionTimePreparation` as two additional terms for structural acquisitions that include multiple contrasts in 4.1.y.

## Symbolic links

### Proposed change

Files in the derived folder can be symbolically linked to the main BIDS folder.
For example, a derived quantitative map `derivatives/sub-01/anat/sub-01_R1.nii.gz` may be linked as `sub-01/anat/sub-01_T1w.nii.gz` to be used as a standard structural image for functional MRI processing pipelines.

This change was made in `/src/02-common-principles.md#symbolic-links`.

### Justification

Whether a file is "derived" or "raw" depends on where you're starting from.
To give a specific example, some researchers will be working on _creating_ quantitative maps, while others will want to _use_ the map as an input file to a structural processing pipeline.
There is a possible natural cut off by saying that files in the BIDS specification are "raw" as they come off the scanner, and "derived" if offline processing has happened.
Unfortunately, it is not possible to harmonize datasets across different scanners when following this rule.
For example MP2RAGE images are sometimes used to construct `_T1uni`-images and `_T1map`s on the scanner system, but on some systems, this only happens post-hoc/"offline".

## Suffix

The original specification uses `modality_label` at the end of each file name.
We have updated this to `suffix` as there are multiple modalities that are not captured in the proposed options, and because the boundaries between "modalities" differ by domain specification.
This change was made in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data`

## B1plus fieldmaps

### Proposed Change

Extend the part on `Fieldmaps` in [4.1.x Task (including resting state) imaging data](/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#task-including-resting-state-imaging-data) by also allowing
for storing B1+ fieldmaps.

### Justification

For some anatomical MRI acquisitions, especially when doing quantiative MRI (qMRI), B1+ fieldmaps can be useful to get better estimates of the underlying physical parameters (e.g., T1 in T1 maps obtained with MP2RAGE-sequence, see Marques et al., 2013).

## Suffix

The original specification uses `modality_label` at the end of each file name.
We have updated this to `suffix` as there are multiple modalities that are not captured in the proposed options, and because the boundaries between "modalities" differ by domain specification.
This change was made in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#anatomy-imaging-data`

### Proposed Change

Add a new subsection to detail different use cases of `suffix` based on three
`suffix` classes:  i) `suffixes for conventional MRI contrasts`, ii) `grouping suffixes` and iii) `designation suffixes for qMRI maps`.

### Justification

Unlike other key labels that exist in the main specification (e.g. `task`), a designated key label (e.g. `modality_name` or `sequence_name`) can not semantically hold true for all the values that it may list for multiple quantitative MRI (qMRI) techniques. This is because qMRI methods can be addressed with respect to the following:
 * sequence names (MP2RAGE)
 * generated outputs (MTR, MTsat)
 * developer-defined acronyms (DESPOT1, hMRI,etc).

As a result, semantic relevance of any specific key label can get easily overloaded. We therefore suggest the use of the `_suffix`, as it is free from a designated key label. Nevertheless, this approach calls for a systematically created definition list of the `_suffix` entries. Otherwise, the approach becomes prone to the violation of one-to-one referral principle of the `_suffix` field. On the other hand, unorganized profileration of `_suffix` entries would eventually yield a noisy and unadaptable list of entries.

Based on the reasons given above, to maintain a viable and well-balanced list of suffixes, BEP001 classify suffixes in three categories:

1. `Suffixes for conventional MRI contrasts` (`W`)
     * **Function:** Denotes the type of the predominant contrast conveyed by a
      single file of a conventional anatomical image.

2. `Grouping suffixes` (`G`)
     * **Function:** Groups together files that belong to parametrically linked
     multiple scans, which are intended for a well-defined qMRI application.

3. `Designation suffixes for qMRI maps` (`M`)
     * **Function:** Denotes the parameter contained within a single file of a
     quantitative map.

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

* JSON file that contains non-varying metedata fields betweeen different `MTS` files,a.k.a. `top` JSON file.
** JSON file that contains all the varying metadata fields, corresponding to the companion nifti file, a.k.a. `lower` JSON file.
```

In conclusion, weight tags denote the type of predominant contrast conveyed by a single file of a conventional anatomical image ONLY when used as a suffix. In all other cases,  the `acq-<label>` key-value pair can be still used to accodomate legacy naming conventions for the sake of accustomed familiarity, yet without interfering with the interchangeability principle. Nevertheless, minimizing the use of weight tags outside their original purpose is highly encouraged to avoid confusion.

>  Answers to the plausible questions:

Q1 - For the `MTS` example given above, why two `<indexable_metadata>-<index>` key-value pairs of `fa` and (a new key tag of) `mt` are not used to distinguish `MTS` images from each other?
* A-1. `<indexable_metadata>-<index>` is applicable when the variable entries are _enumerable_. Whereas `mt` would attain  _categorical_ entries. Therefore, `mt` cannot be defined as a new key tag in the list of allowed key tags.

Q2 - For the `MTS` example given above, the list of `acq-<label>` key-value pairs would only define `MTon` and `MToff` instead of including `T1w`, which then could be used in conjunction with the `<indexable_metadata>-<index>` key-value pair of `fa`. Why deviate from a more weight tag agnostic convention?

* A-2. Although this is a legitimate solution, it was _not preferred_. The main motive
was to keep convention somewhat closer to the accustomed format of `MTw`-`PDw`-`T1w`,
collapsing two varying parameters (flip angle and MT) into one variable. We acknowledge that this somewhat constitutes a trade-off between strictly respecting the principles of the each key-value pair and creating a more readable/familiar
file list for the multi-echo derivatives of the `MTS` method (e.g. `MPM`).

#### Principles for adding a new `_suffix` entry

Updates to the specification is REQUIRED to extend the list of available suffixes. A new entry request or any suggestion to modify existing entries can be made by opening a GitHub issue on [BEP001 repository](https://github.com/bids-standard/bep001). Following principles MUST be respected:

* List of available suffixes is a list of unique entries. Thus, more than one description for a single `_suffix` is not allowed.
* Every suffix MUST belong to one and only one of the three suffix classes of **i)** `grouping suffixes (G)`, **ii)** `designation suffixes for qMRI maps (M)` and **iii)** `suffixes for conventional MRI contrasts (W)`.
* Corresponding suffix class (`G`, `M` or `W`) must be denoted for every entry of the list of available suffixes.

***

* `Grouping suffixes` MUST attain a clear description of the qMRI application that they relate to. If available, hyperlinks to example applications and/or more detailed descriptions are encouraged.
* If there exist an `grouping suffix` which relates to one or many `designation suffix for qMRI map`, all the relevances MUST be indicated in the description of the `grouping suffix` by the _"Associated output suffixes: "_ expression.
* Unless the pulse sequence is exclusively associated with a specific qMRI application (e.g. `MP2RAGE`), sequence names are NOT used as `grouping suffixes`.
* `Grouping suffixes` MUST be used in conjuction with at least one of the `<indexable_metadata>-<index>` and `acq-<label>` key-value pairs. If existing `<indexable_metadata>-<index>` and `acq-<label>` key-value pairs are not enough to describe a new qMRI method, additional request is needed to extend those lists.
* If it is possible to derive a qMRI application from an already existing `grouping suffix` by
means of defining a set of logical conditions over the metadata fields, the _table of method-specific priority levels_ and the _table of qMRI applications that can be derived from an existing `grouping suffix`_ MUST be expanded instead of creating a new `grouping suffix`. Please visit the _JSON content for `grouping suffixes`_ for further details.

***

* `Designation suffixes for qMRI maps` MUST attain a clear description of the parameter that they contain, **including the units**. If available, hyperlinks to example applications and/or more detailed descriptions are highly encouraged.

* If there exist a `designation suffix for qMRI maps` that relates to one or many `grouping suffix`, all the relevances MUST be indicated in the description of the `designation suffix for qMRI map` by the _"Can be generated by: "_ expression.

***

* The list of available `suffixes for conventional MRI contrasts` is immutable: `T1w`,`T2w`,`PDw`,`T2starw`.

***

## Indexable Metadata

Change in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md`.

There are a lot of different additional metadata fields that may be needed for structural brain imaging.
In this change we added a field called `indexable_metadata` to capture those changes.
The use is described in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#indexable_metadata-index-key-value-pair`.

The keys can be pulled from the "Sequence specifics" table (`/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#sequence-specifics`).
This table was updated to include metadata fields for MT sequences and to include metadata fields for any sequence with spoiling gradients.

### Proposed Change

Introduce a new key-value pair that can be used to distinguish images collected
under a `grouping suffix` by naming them through enumerable indexing of metadata
fields that vary between different runs of the same sequence.

### Justification

If the value type of a given metadata field is not categorical (e.g. `MTstate`)
but numeric (e.g. `FlipAngle`), indexing an abbreviated version of the metadata
field name (i.e. key) by enumeration (i.e. value) creates an easily scalable and
human readable key-value pair.

The `<indexable_metadata>` is a placeholder key tag, which is to be substituted
by one of the allowed key tags for `<indexable_metadata>-<index>` key-value pair.
This approach removes the need of introducing a new key-value pair to the anatomy
imaging data template for every metadata field that may create a distinguishing
condition among images collected by a `grouping suffix `.

Different instances of the `<indexable_metadata>-<index>` key-value pair can
co-exist in the same filename. However, instances MUST appear in alphabetical
order and MUST sit next to each other within the designated key-value pair slot
for `<indexable_metadata>-<index>` (please see the template of the anatomy
imaging data). Although the `<indexable_metadata>-<index>` brings some extra conditional checks to the validation and query processes, these requirements
can easily help mitigate such disadvantage.

The order of the enumerated `<indexable_metadata>-<index>` indexes is independent
from the order of the metadata values stored in the `lower` JSON files.

>  Answers to the plausible questions:

## Creating pre-defined labels for the free form `_<acq>-<label>` key-value pair

### Proposed Change

Distinguish images collected under a `grouping suffix` by naming them through
categorical indexing of metadata fields that vary between different runs of the same sequence, using `_<acq>-<label>`.

### Justification

Thanks to being free-form and not being included in the BIDS validation,
the `_<acq>-<label>` key-value pair makes an attractive tool for creating
work-around solutions to virtually any kind of naming conflicts. However,
exploiting this advantage without defining a set of case-specific
boundaries can simply defeat the overall logic of standardization. To
provide a flexible yet controlled solution to the categorical indexing
of the metadata fields included in the filename, we created a table of
pre-defined labels for `_<acq>-<label>` key-value pair:

* Filenames attaining one of the `grouping suffixes` listed in the table
can only use corresponding labels.

>  Answers to the plausible questions:

## Sidecar JSON files of grouping suffixes

### Proposed Change

### Justification

>  Answers to the plausible questions:

## Sidecar JSON files of qMRI maps

### Proposed Change

List and describe metadata fields that can facilitate the provenance
recording of a qMRI map under the _Suffix Class-3: Designation_
_suffixes for qMRI maps_ subsection.

### Justification

Metadata fields listed in the sidecar JSON of the quantitative maps depend on the
way they are generated:

* If a quantitative map is generated at the scanner site through non-transparent vendor implementations, the content is confined to the available metadata. Given
that such maps are the products of proprietary pipelines, metadata provided by
vendors often does not contribute to the reproducibility of the outputs. Nonetheless,
storing them along with the maps provides supporting information.

* If a quantitative map is generated using an open-source software, the sidecar JSON file MUST inherit all the fields from the `top` JSON file of the input data (grouped by a `grouping suffix`) and MUST include some additional metadata fields, which are
to be populated by a BIDS application.

## S0map

### Proposed Change

Add a suffix `_S0map` to store the intercept-parameter for when T2\*-decay curves are fit, for example using a multi-echo Gradien-Recalled Echo (GRE)-sequence.

Change in `/src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#suffix`.

### Justification

Both structural and functional multi-echo sequences are becoming more and more common. By fitting an exponential T2\*-model to such data, a `S0-map` remains that contains contrast for proton density (PD) and B1+ and B1--effects. This can be useful for for example skull-stripping.