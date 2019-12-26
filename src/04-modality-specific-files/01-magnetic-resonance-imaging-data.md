# Magnetic Resonance Imaging data

## Common metadata fields

MR Data described in sections 8.3.x share the following RECOMMENDED metadata
fields (stored in sidecar JSON files). MRI acquisition parameters are divided
into several categories based on
["A checklist for fMRI acquisition methods reporting in the literature"](https://thewinnower.com/papers/977-a-checklist-for-fmri-acquisition-methods-reporting-in-the-literature)
by Ben Inglis:

### Scanner Hardware

| Field name                    | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| :---------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Manufacturer                  | RECOMMENDED. Manufacturer of the equipment that produced the composite instances. Corresponds to DICOM Tag 0008, 0070 `Manufacturer`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| ManufacturersModelName        | RECOMMENDED. Manufacturer's model name of the equipment that produced the composite instances. Corresponds to DICOM Tag 0008, 1090 `Manufacturers Model Name`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| DeviceSerialNumber            | RECOMMENDED. The serial number of the equipment that produced the composite instances. Corresponds to DICOM Tag 0018, 1000 `DeviceSerialNumber`. A pseudonym can also be used to prevent the equipment from being identifiable, so long as each pseudonym is unique within the dataset                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| StationName                   | RECOMMENDED. Institution defined name of the machine that produced the composite instances. Corresponds to DICOM Tag 0008, 1010 `Station Name`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| SoftwareVersions              | RECOMMENDED. Manufacturer’s designation of software version of the equipment that produced the composite instances. Corresponds to DICOM Tag 0018, 1020 `Software Versions`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| HardcopyDeviceSoftwareVersion | (Deprecated) Manufacturer’s designation of the software of the device that created this Hardcopy Image (the printer). Corresponds to DICOM Tag 0018, 101A `Hardcopy Device Software Version`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| MagneticFieldStrength         | RECOMMENDED. Nominal field strength of MR magnet in Tesla. Corresponds to DICOM Tag 0018,0087 `Magnetic Field Strength`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ReceiveCoilName               | RECOMMENDED. Information describing the receiver coil. Corresponds to DICOM Tag 0018, 1250 `Receive Coil Name`, although not all vendors populate that DICOM Tag, in which case this field can be derived from an appropriate private DICOM field                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| ReceiveCoilActiveElements     | RECOMMENDED. Information describing the active/selected elements of the receiver coil. This doesn’t correspond to a tag in the DICOM ontology. The vendor-defined terminology for active coil elements can go in this field. As an example, for Siemens, coil channels are typically not activated/selected individually, but rather in pre-defined selectable "groups" of individual channels, and the list of the "groups" of elements that are active/selected in any given scan populates the `Coil String` entry in Siemens’ private DICOM fields (e.g., `HEA;HEP` for the Siemens standard 32 ch coil when both the anterior and posterior groups are activated). This is a flexible field that can be used as most appropriate for a given vendor and coil to define the "active" coil elements. Since individual scans can sometimes not have the intended coil elements selected, it is preferable for this field to be populated directly from the DICOM for each individual scan, so that it can be used as a mechanism for checking that a given scan was collected with the intended coil elements selected |
| GradientSetType               | RECOMMENDED. It should be possible to infer the gradient coil from the scanner model. If not, e.g. because of a custom upgrade or use of a gradient insert set, then the specifications of the actual gradient coil should be reported independently                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| MRTransmitCoilSequence        | RECOMMENDED. This is a relevant field if a non-standard transmit coil is used. Corresponds to DICOM Tag 0018, 9049 `MR Transmit Coil Sequence`                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| MatrixCoilMode                | RECOMMENDED. (If used) A method for reducing the number of independent channels by combining in analog the signals from multiple coil elements. There are typically different default modes when using un-accelerated or accelerated (e.g. GRAPPA, SENSE) imaging                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| CoilCombinationMethod         | RECOMMENDED. Almost all fMRI studies using phased-array coils use root-sum-of-squares (rSOS) combination, but other methods exist. The image reconstruction is changed by the coil combination method (as for the matrix coil mode above), so anything non-standard should be reported                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

#### Sequence Specifics

| Field name                  | Definition                                                                                                                                                                                                                                                                     |
| :-------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| PulseSequenceType           | RECOMMENDED. A general description of the pulse sequence used for the scan (i.e. MPRAGE, Gradient Echo EPI, Spin Echo EPI, Multiband gradient echo EPI).                                                                                                                       |
| ScanningSequence            | RECOMMENDED. Description of the type of data acquired. Corresponds to DICOM Tag 0018, 0020 `Scanning Sequence`.                                                                                                                                                                |
| SequenceVariant             | RECOMMENDED. Variant of the ScanningSequence. Corresponds to DICOM Tag 0018, 0021 `Sequence Variant`.                                                                                                                                                                          |
| ScanOptions                 | RECOMMENDED. Parameters of ScanningSequence. Corresponds to DICOM Tag 0018, 0022 `Scan Options`.                                                                                                                                                                               |
| SequenceName                | RECOMMENDED. Manufacturer’s designation of the sequence name. Corresponds to DICOM Tag 0018, 0024 `Sequence Name`.                                                                                                                                                             |
| PulseSequenceDetails        | RECOMMENDED. Information beyond pulse sequence type that identifies the specific pulse sequence used (i.e. "Standard Siemens Sequence distributed with the VB17 software," "Siemens WIP ### version #.##," or "Sequence written by X using a version compiled on MM/DD/YYYY"). |
| NonlinearGradientCorrection | RECOMMENDED. Boolean stating if the image saved has been corrected for gradient nonlinearities by the scanner sequence.                                                                                                                                                        |
| MTState                     | RECOMMENDED. Boolean value (`true` or `false`), specifying whether the magnetization transfer pulse is applied. This parameter is REQUIRED by all the anatomical images grouped by `MTR`, `MTS` and `MPM` suffixes. This field originally corresponds to DICOM tag 0018, 9020 `Magnetization Transfer`. |
| MTOffsetFrequency           | RECOMMENDED. The frequency offset of the magnetization transfer pulse with respect to the central H1 Larmor frequency in Hertz (Hz).                                                                                                                                                                    |
| MTPulseBandwidth            | RECOMMENDED. The excitation bandwidth of the magnetization transfer pulse in Hertz (Hz).                                                                                                                                                                                                                |
| MTNumberOfPulses            | RECOMMENDED. Number of magnetization transfer RF pulses applied before the readout.                                                                                                                                                                                                                     |
| MTPulseShape                | RECOMMENDED. Shape of the magnetization transfer RF pulse waveform. Accepted values: `HARD`, `GAUSSIAN`, `GAUSSHANN` (gaussian pulse with Hanning window), `SINC`, `SINCHANN` (sinc pulse with Hanning window), `SINCGAUSS` (sinc pulse with Gaussian window), `FERMI`.                                 |
| MTPulseDuration             | RECOMMENDED. Duration of the magnetization transfer RF pulse in seconds.                                                                                                                                                                                                                                |
| SpoilingState               | RECOMMENDED. Boolean value (`true` or `false`), specifying whether the pulse sequence uses any type of spoiling stratey to suppress transverse magnetization remaining after the readout. |
| SpoilingType                | RECOMMENDED. Specifies which spoiling method(s) are used by a spoiled sequence. Accepted values: `RF`, `GRADIENT` or `COMBINED`.                                                          |
| SpoilingRFPhaseIncrement    | RECOMMENDED. The amount of incrementation described in degrees, which is applied to the phase of the excitation pulse at each TR period for achieving RF spoiling.                        |
| SpoilingGradientMoment      | RECOMMENDED. Zeroth moment of the spoiler gradient lobe in militesla times second per meter (mT.s/m).                                                                                     |
| SpoilingGradientDuration    | RECOMMENDED. The duration of the spoiler gradient lobe in seconds. The duration of a trapezoidal lobe is defined as the summation of ramp-up and plateu times.                            |

#### In-Plane Spatial Encoding

| Field name                     | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| :----------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NumberShots                    | RECOMMENDED. The number of RF excitations need to reconstruct a slice or volume. Please mind that this is not the same as Echo Train Length which denotes the number of lines of k-space collected after an excitation.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ParallelReductionFactorInPlane | RECOMMENDED. The parallel imaging (e.g, GRAPPA) factor. Use the denominator of the fraction of k-space encoded for each slice. For example, 2 means half of k-space is encoded. Corresponds to DICOM Tag 0018, 9069 `Parallel Reduction Factor In-plane`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ParallelAcquisitionTechnique   | RECOMMENDED. The type of parallel imaging used (e.g. GRAPPA, SENSE). Corresponds to DICOM Tag 0018, 9078 `Parallel Acquisition Technique`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| PartialFourier                 | RECOMMENDED. The fraction of partial Fourier information collected. Corresponds to DICOM Tag 0018, 9081 `Partial Fourier`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| PartialFourierDirection        | RECOMMENDED. The direction where only partial Fourier information was collected. Corresponds to DICOM Tag 0018, 9036 `Partial Fourier Direction`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| PhaseEncodingDirection         | RECOMMENDED. Possible values: `i`, `j`, `k`, `i-`, `j-`, `k-`. The letters `i`, `j`, `k` correspond to the first, second and third axis of the data in the NIFTI file. The polarity of the phase encoding is assumed to go from zero index to maximum index unless `-` sign is present (then the order is reversed - starting from the highest index instead of zero). `PhaseEncodingDirection` is defined as the direction along which phase is was modulated which may result in visible distortions. Note that this is not the same as the DICOM term `InPlanePhaseEncodingDirection` which can have `ROW` or `COL` values. This parameter is REQUIRED if corresponding fieldmap data is present or when using multiple runs with different phase encoding directions (which can be later used for field inhomogeneity correction). |
| EffectiveEchoSpacing           | RECOMMENDED. The "effective" sampling interval, specified in seconds, between lines in the phase-encoding direction, defined based on the size of the reconstructed image in the phase direction. It is frequently, but incorrectly, referred to as "dwell time" (see `DwellTime` parameter below for actual dwell time). It is required for unwarping distortions using field maps. Note that beyond just in-plane acceleration, a variety of other manipulations to the phase encoding need to be accounted for properly, including partial fourier, phase oversampling, phase resolution, phase field-of-view and interpolation.<sup>2</sup> This parameter is REQUIRED if corresponding fieldmap data is present.                                                                                                                  |
| TotalReadoutTime               | RECOMMENDED. This is actually the "effective" total readout time , defined as the readout duration, specified in seconds, that would have generated data with the given level of distortion. It is NOT the actual, physical duration of the readout train. If `EffectiveEchoSpacing` has been properly computed, it is just `EffectiveEchoSpacing * (ReconMatrixPE - 1)`.<sup>3</sup> . This parameter is REQUIRED if corresponding "field/distortion" maps acquired with opposing phase encoding directions are present (see 8.9.4).                                                                                                                                                                                                                                                                                                  |

<sup>2</sup>Conveniently, for Siemens’ data, this value is easily obtained as
1/\[`BWPPPE` \* `ReconMatrixPE`\], where BWPPPE is the
"`BandwidthPerPixelPhaseEncode` in DICOM tag (0019,1028) and ReconMatrixPE is
the size of the actual reconstructed data in the phase direction (which is NOT
reflected in a single DICOM tag for all possible aforementioned scan
manipulations). See [here](https://lcni.uoregon.edu/kb-articles/kb-0003) and
[here](https://github.com/neurolabusc/dcm_qa/tree/master/In/TotalReadoutTime)

<sup>3</sup>We use the "FSL definition", i.e, the time between the center of the
first "effective" echo and the center of the last "effective" echo.

#### Timing Parameters

| Field name             | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| :--------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| EchoTime               | RECOMMENDED. The echo time (TE) for the acquisition, specified in seconds. This parameter is REQUIRED if corresponding fieldmap data is present or the data comes from a multi echo sequence. Corresponds to DICOM Tag 0018, 0081 `Echo Time` (please note that the DICOM term is in milliseconds not seconds).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| InversionTime          | RECOMMENDED. The inversion time (TI) for the acquisition, specified in seconds. Inversion time is the time after the middle of inverting RF pulse to middle of excitation pulse to detect the amount of longitudinal magnetization. Corresponds to DICOM Tag 0018, 0082 `Inversion Time` (please note that the DICOM term is in milliseconds not seconds).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| SliceTiming            | RECOMMENDED. The time at which each slice was acquired within each volume (frame) of the acquisition. Slice timing is not slice order -- rather, it is a list of times (in JSON format) containing the time (in seconds) of each slice acquisition in relation to the beginning of volume acquisition. The list goes through the slices along the slice axis in the slice encoding dimension (see below). Note that to ensure the proper interpretation of the `SliceTiming` field, it is important to check if the OPTIONAL `SliceEncodingDirection` exists. In particular, if `SliceEncodingDirection` is negative, the entries in `SliceTiming` are defined in reverse order with respect to the slice axis (i.e., the final entry in the `SliceTiming` list is the time of acquisition of slice 0). This parameter is REQUIRED for sparse sequences that do not have the `DelayTime` field set. In addition without this parameter slice time correction will not be possible.   |
| SliceEncodingDirection | RECOMMENDED. Possible values: `i`, `j`, `k`, `i-`, `j-`, `k-` (the axis of the NIfTI data along which slices were acquired, and the direction in which `SliceTiming` is defined with respect to). `i`, `j`, `k` identifiers correspond to the first, second and third axis of the data in the NIfTI file. A `-` sign indicates that the contents of `SliceTiming` are defined in reverse order - that is, the first entry corresponds to the slice with the largest index, and the final entry corresponds to slice index zero. When present, the axis defined by `SliceEncodingDirection` needs to be consistent with the ‘slice_dim’ field in the NIfTI header. When absent, the entries in `SliceTiming` must be in the order of increasing slice index as defined by the NIfTI header.                                                                                                                                                                                           |
| DwellTime              | RECOMMENDED. Actual dwell time (in seconds) of the receiver per point in the readout direction, including any oversampling. For Siemens, this corresponds to DICOM field (0019,1018) (in ns). This value is necessary for the optional readout distortion correction of anatomicals in the HCP Pipelines. It also usefully provides a handle on the readout bandwidth, which isn’t captured in the other metadata tags. Not to be confused with `EffectiveEchoSpacing`, and the frequent mislabeling of echo spacing (which is spacing in the phase encoding direction) as "dwell time" (which is spacing in the readout direction).                                                                                                                                                                                                                                                                                                                                                 |

#### RF & Contrast

| Field name                  | Definition                                                                                                                                                                                                                                                                                                                                     |
| :-------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| FlipAngle                   | RECOMMENDED. Flip angle for the acquisition, specified in degrees. Corresponds to: DICOM Tag 0018, 1314 `Flip Angle`.                                                                                                                                                                                                                          |
| MultibandAccelerationFactor | RECOMMENDED. The multiband factor, for multiband acquisitions.                                                                                                                                                                                                                                                                                 |
| NegativeContrast            | OPTIONAL. Boolean (`true` or `false`) value specifying whether increasing voxel intensity (within sample voxels) denotes a decreased value with respect to the contrast suffix. This is commonly the case when Cerebral Blood Volume is estimated via usage of a contrast agent in conjunction with a T2\* weighted acquisition protocol.      |

#### Slice Acceleration

| Field name                  | Definition                                                     |
| :-------------------------- | :------------------------------------------------------------- |
| MultibandAccelerationFactor | RECOMMENDED. The multiband factor, for multiband acquisitions. |

#### Anatomical landmarks
Useful for multimodal co-registration with MEG, (S)EEG, TMS, etc.

| Field name                    | Definition                                                                                                                                                                                                                                                                                                                                     |
| :---------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| AnatomicalLandmarkCoordinates | RECOMMENDED. Key:value pairs of any number of additional anatomical landmarks and their coordinates in voxel units (where first voxel has index 0,0,0) relative to the associated anatomical MRI, (e.g. `{"AC": [127,119,149], "PC": [128,93,141], "IH": [131,114,206]}, or {"NAS": [127,213,139], "LPA": [52,113,96], "RPA": [202,113,91]}`). |

#### Institution information

| Field name                  | Definition                                                                                                                                                                            |
| :-------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| InstitutionName             | RECOMMENDED. The name of the institution in charge of the equipment that produced the composite instances. Corresponds to DICOM Tag 0008, 0080 `InstitutionName`.                     |
| InstitutionAddress          | RECOMMENDED. The address of the institution in charge of the equipment that produced the composite instances. Corresponds to DICOM Tag 0008, 0081 `InstitutionAddress`.               |
| InstitutionalDepartmentName | RECOMMENDED. The department in the institution in charge of the equipment that produced the composite instances. Corresponds to DICOM Tag 0008, 1040 `Institutional Department Name`. |

When adding additional metadata please use the camelcase version of
[DICOM ontology terms](https://scicrunch.org/scicrunch/interlex/dashboard)
whenever possible.

### Anatomical imaging data

Template:

```Text
sub-<label>/[ses-<label>/]
    anat/
        sub-<label>[_ses-<label>][_echo-<index>][_fa-<index>][_inv-<index>][_tsl-<index>][_acq-<label>][_part-<mag/phase>][_ce-<label>][_rec-<label>][_run-<index>]_<suffix>.nii[.gz]
        sub-<label>[_ses-<label>][_echo-<index>][_fa-<index>][_inv-<index>][_tsl-<index>][_acq-<label>][_part-<mag/phase>][_ce-<label>][_rec-<label>][_run-<index>][_mod-<suffix>]_defacemask.nii[.gz]
```

The term anatomical imaging data spans a broad range of MRI applications.
These applications share in common the ability to convey structural information
about the imaged anatomy, but differ by the nature of the data they create. Anatomical (structural) data for a participant may refer to i) a single standalone
data set (e.g. a 3D high resolution T1 weighted image), ii) a group of images acquired for the purpose of calculating quantitative maps (`quantitative MRI`, or `qMRI`)
(e.g. four 3D volumes provided as an input to a MP2RAGE calculation), and iii) the quantitative parameter maps themselves (e.g. `T1map` etc.).

#### Conventional structural acquisitions

<NOTE: We should probably lead with some information and examples about a standard T1w conventional acquisition!>

#### Supported modalities, quantitative maps and grouped scan collections

Quantitative MRI maps are calculated from multiple anatomical images with varying MRI acquisition parameters. These `grouped scan collections`
are then processed to derive `qMRI maps`.
Alternatively, grouped scans may be collected to enhance certain contrast
features, such as to calculate a weighted average of multi-echo gradient echo (`MEGRE`) images to improve segmentation algorithm outputs.

The contrast characteristics change with varying acquisition parameters across `grouped scan collections`. Therefore, despite being desired for naming, it is not tenable to label all the members of a `grouped scan collection` with one conventional MRI suffix label (eg: `T1w` or `T2w`). For example, increasing the flip angle (FA) of a spoiled gradient echo acquisition (while keeping all other parameters constant) increases T1-weightning. When a group of images are acquired in this fashion with varying FA, those with low FA are more predominated by `PDw` and `T2starw` effects than those with higher FA (more `T1w` effects). 

To circumvent this problem, the `_<suffix>`
entity is adaptively used according to the intended application of anatomical imaging data.
For conventional MRI applications, the suffixes will correspond to common anatomical contrasts and imaging modalities such as `T1w`, `T2w`, `T2starw` etc.
For groups of scans acquired with the purpose of quantifying parametric maps or generating images with improved structural contrast, the `_<suffix>` indicates the collection that the scans belong to.

#### The `_<suffix>` entity

To ensure an comprehensible, human readable directory that contains anatomical imaging data,
the `_<suffix>` entity can be used in one of three ways:

1. Conventional MRI suffixes
2. Grouping suffixes
3. Quantitative MRI (qMRI) map suffixes

This distinction was added to the specification on acceptance of the BEP001 proposal in version `1.x.x`.
As a result there are some additional suffixes that are no longer recommended but remain part of the specification in order to maintain backwards compatability with previous versions of the specification.
These can be found in the [legacy suffixes](#legacy-suffixes) section below.

##### Conventional MRI suffixes

**Function:**

Denotes the type of the predominant contrast conveyed by an individual file of
a conventional anatomical image.
One of the `_<suffix>` entries listed in the table below
is REQUIRED (with additional entities where applicable) to provide a self
explanatory file name.

A change to the specification is REQUIRED to expand or to modify the following table.

| Name                                       | _suffix | _suffix type | Description                                                                                                                                                                                                                     |
|--------------------------------------------|---------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| T1 weighted images                         | T1w     | Conventional | Denotes images with predominant T1 contribution.                                                                                                                                                                                |
| T2 weighted images                         | T2w     | Conventional | Denotes images with predominant T2 contribution.                                                                                                                                                                                |
| Proton density weighted images             | PDw     | Conventional | Denotes images with predominant proton density (PD) contribution.                                                                                                                                                               |
| T2 star weighted images                    | T2starw | Conventional | Denotes images with predominant T2* contribution, typically images acquired using a GRE sequence with low flip angle, long echo time and long repetition time. Please note that this suffix is not a surrogate for `T2starmap`. |
| Fluid Attenuated Inversion Recovery Images | FLAIR   | Conventional | Denotes images with predominant T2 contribution (a.k.a T2-FLAIR), in which signal from fluids (e.g. CSF) is nulled out by adjusting inversion time, coupled with notably long repetition and echo times.              |

Example use for conventional **T1 weighted images**:<>

```Text
sub-01_run-1_T1w.nii.gz
sub-01_run-1_T1w.json
sub-01_run-2_T1w.nii.gz
sub-01_run-2_T1w.json
```

The `run` entity in the example above denotes the index of the acquisition
repeated with the identical scan parameters (e.g. to achieve a higher SNR by averaging the scans together).
Note that changing parameters between multiple acquisitions of the same sequence
creates a different use case: **grouped scan collections**. For more reading on
this topic, please see the [Grouping suffixes](#grouping-suffixes) subsection.

**Important:**

If an anatomical image is defaced for anonymization, one MAY provide
the binary mask that was used to remove facial features. In the specific case of
naming this binary mask, the `_<suffix>` entity is replaced by `_<defacemask>` entry.
Therefore, to contain the original `_<suffix>` entry, the OPTIONAL `mod-<suffix>`
entity is used. For example, deface mask image belonging to a **T1 weighted image**
is named as follows:

```
sub-01_mod-T1w_defacemask.nii.gz
sub-01_mod-T1w_defacemask.json
```

##### Grouping suffixes

**Function:**

Files that belong to a **grouped scan collection** are intended to be used together within a qMRI analysis framework.
The quantitative maps that are output from these calculations (e.g., `T1map`, `T2map` etc) are described in the [qMRI map suffixes](#qmri-map-suffixes) section below.
A change to the specification is REQUIRED to expand or to modify the following table.

| Name                                       | _suffix | _suffix type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | REQUIRED entities                                                                 | OPTIONAL entities                              |
|--------------------------------------------|---------|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|------------------------------------------------|
| Variable flip angle                        | VFA     | Grouping     | Groups together parametrically linked anatomical images (primarily) for relaxometry mapping. The VFA method involves at least two spoiled gradient echo (SPGR) of steady-state free precession (SSFP) images acquired at different flip angles. Depending on the provided metadata fields and the sequence type, data may be eligible for DESPOT1, DESPOT2 and their variants. Please visit the [method-specific list of priority levels for qMRI metadata](#prioritylevels) for details. _Associated output suffixes_: T1map, T2map, R1map, R2map | i) `indexable_metadata` of `fa`.                                                  | i) `indexable_metadata` of `rfphase`.          |
| Inversion recovery (for T1 mapping)        | IRT1    | Grouping     | Groups together parametrically linked anatomical images for T1 mapping. The IRT1 method involves multiple inversion recovery spin-echo images acquired at different inversion times. _Associated output suffixes_: T1map, R1map                                                                                                                                                                                                                                                                                                 | i) `indexable_metadata` of `inv`.                                                 | N/A                                            |
| Magnetization prepared two gradient echoes | MP2RAGE | Grouping     | Groups together parametrically linked anatomical images (primarily) for T1 mapping. The MP2RAGE method is a special protocol that collects several images at different flip angles and inversion times to create a parametric T1map by combining the magnitude and phase images. _Associated output suffixes_: T1map, R1map, UNIT1                                                                                                                                                                                              | i) `indexable_metadata` of `inv` and `fa`, ii) `part`.                            | i) `indexable_metadata` of `echo`.             |
| Multi-echo spin echo                       | MESE    | Grouping     | Groups together parametrically linked anatomical images (primarily) for T2 mapping.The MESE method involves multiple spin echo images acquired at different echo times. _Associated output suffixes_: T2map, R2map, MWFmap                                                                                                                                                                                                                                                                                                      | i) `indexable_metadata` of `echo`.                                                | N/A                                            |
| Multi-echo gradient echo                   | MEGRE   | Grouping     | Groups together parametrically linked multiple anatomical gradient echo images acquired at different echo times. _Associated output suffixes_: T2starmap, R2starmap, when used for T2* mapping.                                                                                                                                                                                                                                                                                                                                 |i) `indexable_metadata` of `echo`.                                                | N/A                                            |
| Magnetization transfer ratio               | MTR     | Grouping     | Groups together parametrically linked anatomical images for calculating a semi-quantitative magnetization transfer ratio map. _Associated output suffixes_: MTRmap                                                                                                                                                                                                                                                                                                                                                              |i) `acq` with `MTon` and `MToff` values.                                          | N/A                                            |
| Magnetization transfer saturation          | MTS     | Grouping     | Groups together parametrically linked anatomical images for calculating a semi-quantitative magnetization transfer saturation index map. The MTS method involves three sets of anatomical images that differ in terms of application of a magnetization transfer RF pulse (MTon or MToff) and flip angle. _Associated output suffixes_: T1map, MTsat                                                                                                                                                                            |i) `acq` with `MTon`, `MToff` and `T1w` values. | N/A                                            |
| Multi-parametric mapping                   | MPM     | Grouping     | Groups together parametrically linked anatomical images for multiparametric mapping (a.k.a hMRI). The MPM method involves anatomical images differing in terms of application of a magnetization transfer RF pulse (MTon or MToff), flip angle and (optionally) echo time and magnitue/phase parts. Please visit [here](https://owncloud.gwdg.de/index.php/s/iv2TOQwGy4FGDDZ) for suggested MPM acquisition protocols.  _Associated output suffixes_:R1map, R2starmap, MTsat, PDmap, T1map, T2starmap                                                                                                                                                                        | i) `acq` with `MTon`, `MToff` and `T1w` values. | i) `indexable_metadata` of `echo`, ii) `part`. |
| Double-angle B1 mapping                    | B1DAM   | Grouping     | Groups together parametrically linked anatomical images for RF transmit field (B1 plus) mapping. Double angle method is based on the calculation of the actual angles from signal ratios, collected by two acquisitions at different nominal excitation angles. Common sequence types for this application include spin echo and echo planar imaging. _Associated output suffixes_: B1plusmap                                                                                                                                   | i) `indexable_metadata` of `fa`.                                                  | N/A                                            |
| Gradient and spin echo                     | GRASE   | Grouping     | Groups together parametrically linked anatomical images acquired using gradient and spin echo (GRASE) sequence (primarily) for T2 mapping. _Associated output suffixes_: T2map, R2map, MWFmap                                                                                                                                                                                                                                                                                                                                   | i) `indexable_metadata` of `echo`.                                                | N/A                                            |

For example:

```Text
sub-01_fa-1_VFA.nii.gz
sub-01_fa-1_VFA.json
sub-01_fa-2_VFA.nii.gz
sub-01_fa-2_VFA.json
```

Note that every image in a **grouped scan collection** has the same `_<suffix>` as they are likely to be used together.
Although the acquisitions will have many identical acquisition parameters, only one `.json`-sidecar file is permitted per image within a single folder (following the [inheritance principle](../02-common-principles#the-inheritance-principle)).
This means that parameter values that are identical across a set of grouped scans will still have to be stored separately in each `.json`-sidecar file.

Individual images within a grouped scan collection may be used for any purpose.
For example, the acquisition that is most similar to a `T1w` image may be used in analysis pipelines needing a conventional T1-weighted image.

<NOTE: The real world data sets probably need more documentation than a link here 😬>
[Example real-world datasets can be downloaded here.](https://osf.io/k4bs5/)

**Method-specific priority levels for qMRI metadata:**

Although there is not an upper limit to the amount of metadata
for images collected by a `grouping suffix`, some of the metadata entries become
REQUIRED when considered within the context of a specific qMRI
application.

<a name="prioritylevels">Table of method-specific priority levels for qMRI metadata</a>

| Grouping suffix | REQUIRED metadata                                                                                    | OPTIONAL metadata          |
|-----------------|------------------------------------------------------------------------------------------------------|----------------------------|
| VFA             | `FlipAngle`, `PulseSequenceType`, `RepetitionTimeExcitation`                                         | `SpoilingRFPhaseIncrement` |
| IRT1            | `InversionTime`                                                                                      | -                          |
| MP2RAGE         | `FlipAngle`, `InversionTime`, `RepetitionTimeExcitation`, `RepetitionTimePreperation`, `NumberShots` | `EchoTime`                 |
| MESE            | `EchoTime`                                                                                           | -                          |
| MEGRE           | `EchoTime`                                                                                           | -                          |
| MTR             | `MTState`                                                                                            | -                          |
| MTS             | `FlipAngle`, `MTState`, `RepetitionTimeExcitation`                                                   | -                          |
| MPM             | `FlipAngle`, `MTState`, `RepetitionTimeExcitation`                                                   | `EchoTime`                 |
| B1DAM           | `FlipAngle`                                                                                          | -                          |
| GRASE           | `EchoTime`                                                                                           | -                          |

Explanation of the table:

* The metadata fields listed in the REQUIRED columns are needed to perform a
minimum viable qMRI application for the corresponding `grouping suffix`.
* Note that some of the metadata fields may be unaltered across different members
of a given `grouped scan collection`, yet still needed as an input to a qMRI
model for parameter fitting (e.g. `RepetitionTimeExcitation` of `VFA`). 
* The metadata fields listed in the OPTIONAL columns can be used to derive
different flavors of the minimum viable qMRI application for the respective
`grouping suffix`. The following section expands on the set of rules governing
the derivation of qMRI applications from an existing `grouping suffix`.

**qMRI applications that can be derived from an existing `grouping suffix`:**

Certain grouping suffixes may refer to a generic data collection regime such as
variable flip angle (VFA), rather than a more specific acquisition, e.g.,
magnetization prepared two gradient echoes (MP2RAGE). Such generic acquisitions
can serve as a basis to derive various qMRI applications by changes to
the acquisition sequence (e.g. readout) type or varying additional scan parameters.

If such inheritance relationship is applicable between an already existing
`grouping suffix` and a new qMRI application to be included in the specification,
the inheritor qMRI method MUST be listed in the table below instead of
introducing a new `grouping suffix`. This approach:

* prevents the list of available suffixes from over-proliferation
* provides qMRI-focused BIDS applications with a set of meta-data driven rules
to infer possible fitting options
* keep an inheritance track of the qMRI methods described within the
specification.

<a name="varianttable">Table of qMRI applications that can be derived from an existing grouping suffix</a>

| Grouping suffix | If REQUIRED metadata == Value | OPTIONAL metadata [`var`/`fix`]<sup>[*](#footnotederive)</sup>      | Derived application |
|-----------------|-------------------------------|----------------------------------|---------------------|
| VFA             | `PulseSequenceType` == `SPGR` | -                                | DESPOT1             |
| VFA             | `PulseSequenceType` == `SSFP` | `SpoilingRFPhaseIncrement` [`fix`] | DESPOT2             |
| VFA             | `PulseSequenceType` == `SSFP` | `SpoilingRFPhaseIncrement` [`var`] | DESPOT2-FM          |
| MP2RAGE         | -                             | `EchoTime` [`var`]                 | MP2RAGE-ME          |
| MPM             | -                             | `EchoTime` [`var`]                 | MPM-ME              |

<a name="footnotederive">*</a> `var` denotes that the listed OPTIONAL metadata value changes across
 constituent images of the respective `grouping suffix`, fixed otherwise (`fix`). 
 If the OPTIONAL metadata type is `var`, respective naming entities can be found in the 
 OPTIONAL entities column of [`grouping suffix` table](#grouping-suffix). 
*** 

A derived qMRI application becomes avaiable if all the OPTIONAL metadata fields
listed for a `grouping suffix` is provided in the data. In addition, conditional
rules based on the value of a given REQUIRED metada field can be set
for the description of a derived qMRI application. Note that the value of this
REQUIRED metadata is fixed across constituent images of a `grouping suffix`. 

For example, if the REQUIRED metadata field of `PulseSequenceType` is SPGR
for a collection of anatomical images listed by the `VFA` suffix, the data
qualifies for `DESPOT1` T1 fitting. For the same suffix, if the `PulseSequenceType`
metadata field has the value of `SSFP`, and the `SpoilingRFPhaseIncrement` is 
provided as a metadata field, then the dataset becomes eligible for `DESPOT2`
T2 fitting application. Finally, if the `DESPOT2` data has more than one
`SpoilingRFPhaseIncrement` field as a metadata field, then the dataset is valid
for `DESPOT2-FM`.

Please note that OPTIONAL metadata fields listed in the [qMRI applications that can be derived from an existing suffix table](#varianttable) MUST be also included in the [method sprecific priority levels for qMRI metadata table](#prioritylevels)  for the sake of completeness.

Please also note that the rules concerning the presence/value of certain metadata
fields within the context of `grouping suffix` is not a part of the BIDS
validation process. Such rules rather constitute a centralized guideline for
creating interoperable qMRI datasets.

For a dataset with a `grouping suffix`, the BIDS validation is successful if:

* provided NIfTI and JSON file names respect the anatomy imaging dataset template
* provided suffixes are present in the list of available suffixes
* sidecar JSON files follow the hierarchy defined for `grouping suffix`.

#### qMRI map suffixes

**Function:**

Denotes the parameter contained within an individual file of a
quantitative parametric image.

Changes to the specification is REQUIRED to
expand or to modify the following table.

| Name                                                   | _suffix   | _suffix type | Description                                                                                                                                                                                                                                                                          |
|--------------------------------------------------------|-----------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Longitudinal relaxation time map                       | T1map     | Parametric   | In seconds (s). T1 maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `VFA`, `IRT1`, `MP2RAGE`, `MTS`,`MPM`. You can visit [this interactive book for T1 mapping](https://qmrlab.org/t1_book/intro) for further reading.  |
| True transverse relaxation time map                    | T2map     | Parametric   | In seconds (s). T2 maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `MESE`, `MPM`, `GRASE`                                                                                                                                       |
| Observed transverse relaxation time map                | T2starmap | Parametric   | In seconds (s). T2* maps are REQUIRED to use this suffix irrespective of the method they are related to._Can be generated from:_ `MEGRE`, `MPM`                                                                                                                                      |
| Longitudinal relaxation rate map                       | R1map     | Parametric   | In seconds-1 (1/s). R1 maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `VFA`, `IRT1`, `MP2RAGE`, `MTS`, `MPM`                                                                                                           |
| True transverse relaxation rate map                    | R2map     | Parametric   | In seconds-1 (1/s). R2 maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `MESE`, `MPM`, `GRASE`                                                                                                                                  |
| Observed transverse relaxation rate map                | R2starmap | Parametric   | In seconds-1 (1/s). R2* maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_`MEGRE`, `MPM`                                                                                                                                 |
| Proton density map                                     | PDmap     | Parametric   | In arbitrary units (a.u.). PD maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `MPM`                                                                                                                                    |
| Magnetization transfer ratio map                       | MTRmap    | Parametric   | In percentage (%). MTR maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `MTR`                                                                                                                                           |
| Magnetization transfer saturation index map            | MTSat     | Parametric   | In arbitrary units (a.u.). MTsat maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `MTS`, `MPM`                                                                                                                          |
| Homogeneous (flat) T1 image by MP2RAGE                 | UNIT1     | Parametric   | In arbitrary units (a.u.). UNIT1 maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `MP2RAGE`                                                                                                                             |
| Longutidunal relaxation in rotating frame (T1 rho) map | T1rho     | Parametric   | In seconds (s). T1-rho maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ N/A                                                                                                                                             |
| Myelin water fraction map                              | MWFmap    | Parametric   | In percentage (%). MWF maps are REQUIRED to use this suffix irrespective of the method they are related to. _Can be generated from:_ `MESE`, `GRASE`                                                                                                                                          |
| Combined PD/T2 map                                     | PDT2map   | Parametric   | In arbitrary units (a.u.). Combined PD/T2 maps are REQUIRED to use this suffix irrespective of the method they are related to. N/A                                                                                                                                                   |
| RF transmit field map                                  | B1plusmap | Parametric   | In arbitrary units (a.u.). Radio frequency (RF) transmit field maps are REQUIRED to use this suffix irrespective of the method they are related to. For further details please see the fieldmap data section.  _Can be generated from_: `B1DAM`                                                                      |

Quantitative maps can be obtained right off the scanner or by processing files
belonging to a `grouped scan collection`. Regardless of the method they are
obtained by, one of the `_suffix` entries listed in the table above is REQUIRED
for a proper naming. For example:

```Text
sub-01_T1map.nii.gz
sub-01_T1map.json
```
**Important:**

Quantitative unit of the parameter contained by a quantitative map MUST comply
with the unit description provided for its respective `_suffix` entry.
For example, a T1 map in milliseconds unit (ms) is not valid, given that the
description of the `T1map` suffix requires the parameter to be in seconds (s).

**Content of the JSON files accompanying qMRI maps:**

Metadata fields listed in the sidecar JSON of a qMRI map depend on the method by
which it is obtained:
* If a qMRI map is generated at the scanner site through non-transparent vendor
implementation, JSON content is confined to the available metadata.
* If a qMRI map is generated using an open-source software, its sidecar JSON
file MUST inherit content from the JSON files of the constituent images of a
`grouped scan collection` by adhering to the following rules:
     * All the acquisition parameters that are unchanged across constituents of
     a `grouped scan collection` are added to the JSON file of the resultant
     qMRI map.
     * Relevant acquisition parameters that vary across constituents of a
     `grouped scan collection` are added to the JSON file of the resultant
     qMRI map **in array form**. To find out which varying scan parameters are
     relevant to a given `grouped scan collection`, please see the
    [method-specific priority levels for qMRI metadata](#prioritylevels) table in the [grouping suffixes](#grouping-suffixes) subsection.
     * The JSON file accompanying a qMRI map which is obtained by
     using an open-source software MUST include all the metadata fields listed
     in the following table for the sake of provenance.

| Field name                  | Definition                                                     |
| :-------------------------- | :------------------------------------------------------------- |
| BasedOn | List of files grouped by an `grouping suffix` to generate the map. The fieldmaps are also listed, if involved in the processing. |
| EstimationReference | Reference to the study/studies on which the implementation is based.|
| EstimationAlgorithm | Type of algoritm used to perform fitting (e.g. linear, non-linear, LM etc.)|
| EstimationSoftwareName | The name of the open-source tool used for fitting (e.g. qMRLab, QUIT, hMRI-Toolbox etc.)|
| EstimationSoftwareVer | Version of the open-source tool used for fitting (e.g. v2.3.0 etc.)|
| EstimationSoftwareLang | Language in which the software is natively developed (e.g. MATLAB R2018b, C++17, Python 3.6 etc.)|
| EstimationSoftwareEnv | Operation system on which the application was run (e.g. OSX 10.14.3, Ubuntu 18.04, Win10 etc.)|

Example:

```Text
sub-01_T1map.nii.gz
sub-01_T1map.json
```

```Text
sub-01_T1map.json
{

<<Parameter injected by the software for provenance recording>>

"BasedOn":["anat/sub-01_fa-1_VFA.nii.gz",
           "anat/sub-01_fa-2_VFA.nii.gz",
           "anat/sub-01_fa-3_VFA.nii.gz",
           "anat/sub-01_fa-4_VFA.nii.gz",
           "fmap/sub-01_B1plusmap.nii.gz"],

<<Parameters that are constant across members of VFA grouping suffix>>

“MagneticFieldStrength”: “3”,
“Manufacturer”: “Siemens”,
“ManufacturerModelName”: “TrioTim”,
“InstitutionName”: “xxx”,
“PulseSequenceType”: “SPGR”,
“PulseSequenceDetails”: “Information beyond the sequence type that identifies
 specific pulse sequence used (VB version, if not standard, Siemens WIP XXX
 ersion ### sequence written by xx using a version compiled on mm/dd/yyyy/)”,
"RepetitionTimeExcitation": "35",
"EchoTime": "2.86",
"SliceThickness": "5",

<<Relevant parameters that vary across members of VFA grouping suffix>>

"FlipAngle": ["5","10","15","20"],

<<Additional parameters injected by the software for provenance recording>>

"EstimationPaper":"Deoni et. al.MRM, 2015",
"EstimationAlgorithm":"Linear",
“EstimationSoftwareName”: “qMRLab”,
“EstimationSoftwareLanguage”: “Octave”,
“EstimationSoftwareVersion”: “2.3.0”,
“EstimationSoftwareEnv”: “Ubuntu 16.04”
}
```

##### Legacy suffixes (to be deprecated)

Some suffixes were defined in the original BIDS 1.0.0 - 1.2.0 specification,
but it is recommended not to use them any more, because they are inconsistent
and/or ambiguous. They will most likely be deprecated in later main versions of
BIDS.

However, they are still part of the BIDS 1.x specification for legacy reasons.
Thus, the following suffixes are VALID, but NOT RECOMENDED:

| Name           | Suffix    | Description                                                    | Reason to deprecate                                                                                                                                                                                                                                                                                                                                                                                                |
|----------------|-----------|----------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| T2\*           | T2star    | Not provided.                                                  | Ambiguous, may refer to a parametric image or to a conventional image. **Change:** Replaced by `T2starw` or `T2starmap`.                                                                                                                                                                                                                                                                                           |
| FLASH          | FLASH     | Not provided.                                                  | FLASH (Fast-Low-Angle-Shot) is a vendor specific implementation for spoiled  gradient echo acquisition. It is commonly used for rapid anatomical imaging  and also for many different qMRI applications. Given the versatility and the popularity of the FLASH sequence, it can easily obfuscate purpose of a  `grouped scan collection` when used as a `grouping suffix`. **Change:** Removed from suffixes.     |
| Proton Density | PD        | Not provided.                                                  | Ambiguous, may refer to a parametric image or to a conventional image. **Change:** Replaced by `PDw` or `PDmap`.                                                                                                                                                                                                                                                                                                   |
| Inplane T1     | inplaneT1 | T1-weighted anatomical image matched to functional acquisition | Explanation goes here. **Change:** Removed from the suffixes.                                                                                                                                                                                                                                                                                                                                                                             |
| Inplane T2     | inplaneT2 | T2-weighted anatomical image matched to functional acquisition | Explanation goes here. **Change:** Removed from the suffixes.                                                                                                                                                                                                                                                                                                                                                                             |


#### The `indexable_metadata` entity

If multiple anatomical images are bundled together by a `grouping suffix`, there
is at least one metadata field that varies across constituent images. If the
varying metadata field values are enumerable and the metadata field is listed
in the table of allowed key tags (see below), `indexable_metadata` entity SHOULD
be included in the file name to distinguish constituent images from each other.

<a name="allowedkeys">Table of allowed key tags for the indexable_metadata entity</a>

| Allowed key tags | Value list | Associated metadata field |
|---------|------------|---------------------|
| echo    | 1,2,... N  | EchoTime            |
| fa      | 1,2,... N  | FlipAngle           |
| inv     | 1,2,... N  | InversionTime       |
| rfphase | 1,2,... N  | SpoilingRFPhaseIncrement      |
| tsl     | 1,2,... N  | SpinLockTime        |

For example (for a multi-echo gradient echo dataset):

```Text
sub-01_echo-1_MEGRE.nii.gz
sub-01_echo-1_MEGRE.json
sub-01_echo-2_MEGRE.nii.gz
sub-01_echo-2_MEGRE.json
sub-01_echo-3_MEGRE.nii.gz
sub-01_echo-3_MEGRE.json
```
**Some important considerations regarding the `indexable_metadata` entity:**

* Unlike other entities, key tag of the `indexable_metadata` entity is
mutable depending on the metadata field that varies across the constituents of a
`grouping scan collection`. Therefore, this entity can appear more than once in
the filename with different keys.

* Please note that the order of the `index` and the value of the associated
metadata field do NOT have to be coherent (i.e. `fa-1`,`fa-2` and `fa-3` can
correspond to the `FlipAngle` of `35`, `10` and `25` degrees), and the actual
values need to be stored in the corresponding metadata field of the accompanying
JSON files.

* Please note that `<indexable_metadata>-<index>` is not free form. Updates to the
specification is REQUIRED to extend [the allowed key tags](#allowedkeys).

**Important:**

If a file name contains more than one `indexable_metadata` entity, existent key
tags MUST appear in alphabetical order in the file name. For example:

    ```
    sub-01_echo-1_inv-1_MP2RAGE.nii.gz
    sub-01_echo-1_inv-1_MP2RAGE.json
    ```

#### The `acq` entity

The OPTIONAL `acq-<label>` key/value pair corresponds to a custom label the user
MAY use to distinguish a different set of parameters used for acquiring the same
modality. For example this should be used when a study includes two T1w images -
one full brain low resolution and and one restricted field of view but high
resolution. In such case two files could have the following names:
`sub-01_acq-highres_T1w.nii.gz` and `sub-01_acq-lowres_T1w.nii.gz`, however the
user is free to choose any other label than `highres` and `lowres` as long as
they are consistent across subjects and sessions. In case different sequences
are used to record the same modality (e.g. RARE and FLASH for T1w) this field
can also be used to make that distinction. At what level of detail to make the
distinction (e.g. just between RARE and FLASH, or between RARE, FLASH, and
FLASHsubsampled) remains at the discretion of the researcher.

If multiple anatomical images are bundled together by a `grouping suffix`, there
is at least one metadata field that varies across constituent images. If the
varying metadata field values are categorical and the label is listed in the
table of available `acq` entity labels for a given `grouping suffix` (see below),
`acq-<label>` SHOULD be included in the file name.

Note that value of the `acq-<label>` is free form. However, to enable a unified
naming convention while combining several scans of the same modality intended to
create quantitative maps, following labels SHOULD be included in the filename
where applicable:

<a name="allowedacq">Table of allowed `<acq>` entity labels for `grouping suffixes`</a>

| Grouping suffix | Labels           | Respective metadata fields and values   |
|-------------|------------------|------------------------------|
| MTR         | `MTon`, `MToff`      | MTState [`On`, `Off`] |
| MTS         | `MTon`, `MToff`, `T1w` | MTstate [`On`,`Off`,`Off`], FlipAngle [`lower`,`lower`,`higher`] |
| MPM         | `MTon`, `MToff`, `T1w` | MTstate [`On`,`Off`,`Off`], FlipAngle [`lower`,`lower`,`higher`] |

For example (for an `MPM` dataset):

```Text
sub-01_echo-1_acq-MTon_MPM.nii.gz
sub-01_echo-1_acq-MTon_MPM.json
sub-01_echo-1_acq-MToff_MPM.nii.gz
sub-01_echo-1_acq-MToff_MPM.json
sub-01_echo-1_acq-T1w_MPM.nii.gz
sub-01_echo-1_acq-T1w_MPM.json
```
**Some important considerations regarding the `acq` entity:**

* Note that value of the `acq-<label>` is free form by default, as indicated
by the main specification. However, to enable a unified naming convention for
this specific use case, only the labels listed in [the table of allowed `acq` entity labels](#allowedacq) SHOULD be used with the corresponding `grouping suffixes`.
* Changes to the specification is REQUIRED to extend [the table of allowed `acq`
entity labels](#allowedacq).


#### `part-<mag/phase>` key/value pair

Some parametrically linked anatomical images involve both magnitude and phase
reconstructed images in the calculation of a parameter map. In that case, the
filename MUST make use of this key/value pair to distinguish between them.

Phase images SHOULD be in radians and have a range of (0, 2 pi]
(including 0, excluding 2 pi).

The `part-<mag/phase>` key/value pair is associated with the DICOM tag 0008,0008
`Image Type`.

For example (for an `MP2RAGE` dataset):

```Text
sub-01_inv-1_part-mag_MP2RAGE.nii.gz
sub-01_inv-1_part-mag_MP2RAGE.json

sub-01_inv-1_part-phase_MP2RAGE.nii.gz
sub-01_inv-1_part-phase_MP2RAGE.json

sub-01_inv-2_part-mag_MP2RAGE.nii.gz
sub-01_inv-2_part-mag_MP2RAGE.json

sub-01_inv-2_part-phase_MP2RAGE.nii.gz
sub-01_inv-2_part-phase_MP2RAGE.json

```

#### The `ce` entity

The OPTIONAL `ce-<label>` key/value can be used to distinguish
sequences using different contrast enhanced images. The label is the name of the
contrast agent. The key `ContrastBolusIngredient` MAY be also be added in the
JSON file, with the same label.

#### The `rec` entity

Similarly the OPTIONAL `rec-<label>` key/value can be used to distinguish
different reconstruction algorithms (for example ones using motion correction).

If the structural images included in the dataset were defaced (to protect
identity of participants) one CAN provide the binary mask that was used to
remove facial features in the form of `_defacemask` files. In such cases the
OPTIONAL `mod-<label>` key/value pair corresponds to the modality (as stored in
the `suffix`) of the acquisition eg: T1w, inplaneT1, referenced by a
defacemask image. E.g., `sub-01_mod-T1w_defacemask.nii.gz` where `T1w` is the
scan suffix which becomes the value for the `mod-` entity.

Some meta information about the acquisition MAY be provided in an additional
JSON file. See [Common metadata fields](#common-metadata-fields) for a
list of terms and their definitions. There are also some OPTIONAL JSON
fields specific to anatomical scans:

| Field name              | Definition                                                                                                                                         |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| ContrastBolusIngredient | OPTIONAL. Active ingredient of agent. Values MUST be one of: IODINE, GADOLINIUM, CARBON DIOXIDE, BARIUM, XENON Corresponds to DICOM Tag 0018,1048. |
| RepetitionTimeExcitation | OPTIONAL. The time in seconds between successive excitation pulses that excite the same tissue. The DICOM tag that best refers to this parameter is [(0018, 0080)](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)). This field may be superseded by `RepetitionTimePreparation` for certain use cases, such as [MP2RAGE](https://infoscience.epfl.ch/record/172927/files/mp2rage.pdf). Use `RepetitionTimeExcitation` (in combination with `RepetitionTimePreparation` if needed) for anatomy imaging data rather than `RepetitionTime` as it is already defined as the amount of time that it takes to acquire a single volume in section 4.1.x. |
| RepetitionTimePreparation | OPTIONAL. The period of time in seconds that it takes a preparation pulse block to re-appear at the beginning of the succeeding (essentially identical) pulse sequence. |

#### The `run` entity

If the same acquisition for a given `_suffix` is repeated **without any parameter**
**changes**, they must be indexed with the key/value pair of `run-<index>`:
`_run-1`, `_run-2`, `_run-3` etc. (only integers are allowed as run numbers).
When there is only one scan of a given type, the run key MAY be omitted.

#### Other OPTIONAL metadata

Some meta information about the acquisition MAY be provided in an additional
JSON file. See Common MR metadata fields for a list of terms and their
definitions. There are also some OPTIONAL JSON fields specific to anatomical
scans:

| Field name              | Definition                                                                                                                                         |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- |
| ContrastBolusIngredient | OPTIONAL. Active ingredient of agent. Values MUST be one of: IODINE, GADOLINIUM, CARBON DIOXIDE, BARIUM, XENON Corresponds to DICOM Tag 0018,1048. |
| RepetitionTimeExcitation | OPTIONAL. The time in seconds between successive excitation pulses that excite the same tissue. The DICOM tag that best refers to this parameter is [(0018, 0080)](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)). This field may be superseded by `RepetitionTimePreparation` for certain use cases, such as [MP2RAGE](https://infoscience.epfl.ch/record/172927/files/mp2rage.pdf). Use `RepetitionTimeExcitation` (in combination with `RepetitionTimePreparation` if needed) for anatomy imaging data rather than `RepetitionTime` as it is already defined as the amount of time that it takes to acquire a single volume in section 4.1.x. |
| RepetitionTimePreparation | OPTIONAL. The period of time in seconds that it takes a preparation pulse block to re-appear at the beginning of the succeeding (essentially identical) pulse sequence. |



### Task (including resting state) imaging data

Currently supported image contrasts include:

| Name               | `contrast_label` | Description                                                                                                             |
| :----------------- | :--------------- | :-----------------------------------------------------------------------------------------------------------------------|
| BOLD               | bold             | Blood-Oxygen-Level Dependent contrast (specialized T2\* weighting)                                                      |
| CBV                | cbv              | Cerebral Blood Volume contrast (specialized T2\* weighting or difference between T1 weighted images)                    |
| Phase              | phase            | Phase information associated with magnitude information stored in BOLD contrast                                         |

Template:

```Text
sub-<label>/[ses-<label>/]
    func/
        sub-<label>[_ses-<label>]_task-<label>[_acq-<label>][_ce-<label>][_dir-<label>][_rec-<label>][_run-<index>][_echo-<index>]_<contrast_label>.nii[.gz]
        sub-<label>[_ses-<label>]_task-<label>[_acq-<label>][_ce-<label>][_dir-<label>][_rec-<label>][_run-<index>][_echo-<index>]_sbref.nii[.gz]
```

Imaging data acquired during functional imaging (i.e. imaging which supports
rapid temporal repetition). This includes but is not limited to task based fMRI
as well as resting state fMRI (i.e. rest is treated as another task). For task
based fMRI a corresponding task events file (see below) MUST be provided
(please note that this file is not necessary for resting state scans). For
multiband acquisitions, one MAY also save the single-band reference image as
type `sbref` (e.g. `sub-control01_task-nback_sbref.nii.gz`).

Each task has a unique label MUST only include of letters and/or numbers (other
characters including spaces and underscores are not allowed). Those labels MUST
be consistent across subjects and sessions.

If more than one run of the same task has been acquired a key/value pair:
`_run-1`, `_run-2`, `_run-3` etc. MUST be used. If only one run was acquired the
`run-<index>` can be omitted. In the context of functional imaging a run is
defined as the same task, but in some cases it can mean different set of stimuli
(for example randomized order) and participant responses.

The OPTIONAL `acq-<label>` key/value pair corresponds to a custom label one may
use to distinguish different set of parameters used for acquiring the same task.
For example this should be used when a study includes two resting state images -
one single band and one multiband. In such case two files could have the
following names: `sub-01_task-rest_acq-singleband_bold.nii.gz` and
`sub-01_task-rest_acq-multiband_bold.nii.gz`, however the user is MAY choose any
other label than `singleband` and `multiband` as long as they are consistent
across subjects and sessions and consist only of the legal label characters.

Similarly the OPTIONAL `ce-<label>` key/value can be used to distinguish
sequences using different contrast enhanced images. The label is the name of the
contrast agent. The key ContrastBolusIngredient MAY be also be added in the JSON
file, with the same label.

Similarly the OPTIONAL `rec-<label>` key/value can be used to distinguish
different reconstruction algorithms (for example ones using motion correction).

Similarly the OPTIONAL `dir-<label>` and `rec-<label>` key/values
can be used to distinguish different phase-encoding directions and
reconstruction algorithms (for example ones using motion correction).
See [`fmap` Case 4](01-magnetic-resonance-imaging-data.md#case-4-multiple-phase-encoded-directions-pepolar)
for more information on `dir` field specification.

Multi-echo data MUST be split into one file per echo. Each file shares the same
name with the exception of the `_echo-<index>` key/value. For example:

```Text
sub-01/
   func/
      sub-01_task-cuedSGT_run-1_echo-1_bold.nii.gz
      sub-01_task-cuedSGT_run-1_echo-1_bold.json
      sub-01_task-cuedSGT_run-1_echo-2_bold.nii.gz
      sub-01_task-cuedSGT_run-1_echo-2_bold.json
      sub-01_task-cuedSGT_run-1_echo-3_bold.nii.gz
      sub-01_task-cuedSGT_run-1_echo-3_bold.json
```

Please note that the `<index>` denotes the number/index (in a form of an
integer) of the echo not the echo time value which needs to be stored in the
field EchoTime of the separate JSON file. This is an example of the
`indexable_metadata` entity as described
[here](src/04-modality-specific-files/01-magnetic-resonance-imaging-data.md#indexable_metadata-index-key-value-pair)).

Some meta information about the acquisition MUST be provided in an additional
JSON file.

#### Required fields

| Field name     | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| :------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| RepetitionTime | REQUIRED. The time in seconds between the beginning of an acquisition of one volume and the beginning of acquisition of the volume following it (TR). When used in the context of functional acquisitions this parameter best corresponds to [DICOM Tag 0020,0110](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0020,0110)): the "time delta between images in a dynamic of functional set of images" but may be found in [DICOM Tag 0018, 0080](http://dicomlookup.com/lookup.asp?sw=Tnumber&q=(0018,0080)): "the period of time in msec between the beginning of a pulse sequence and the beginning of the succeeding (essentially identical) pulse sequence". This definition includes time between scans (when no data has been acquired) in case of sparse acquisition schemes. This value MUST be consistent with the '`pixdim[4]`' field (after accounting for units stored in '`xyzt_units`' field) in the NIfTI header. This field is mutually exclusive with `VolumeTiming`. |
| VolumeTiming   | REQUIRED. The time at which each volume was acquired during the acquisition. It is described using a list of times (in JSON format) referring to the onset of each volume in the BOLD series. The list must have the same length as the BOLD series, and the values must be non-negative and monotonically increasing. This field is mutually exclusive with `RepetitionTime` and `DelayTime`. If defined, this requires acquisition time (TA) be defined via either `SliceTiming` or `AcquisitionDuration` be defined.                                        |
| TaskName       | REQUIRED. Name of the task. No two tasks should have the same name. Task label (`task-`) included in the file name is derived from this field by removing all non alphanumeric (`[a-zA-Z0-9]`) characters. For example task name `faces n-back` will corresponds to task label `facesnback`. A RECOMMENDED convention is to name resting state task using labels beginning with `rest`.                                                                                                                                                                        |

For the fields described above and in the following section, the term "Volume"
refers to a reconstruction of the object being imaged (e.g., brain or part of a
brain). In case of multiple channels in a coil, the term "Volume" refers to a
combined image rather than an image from each coil.

#### Other RECOMMENDED metadata

##### Timing Parameters

| Field name                        | Definition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| :-------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| NumberOfVolumesDiscardedByScanner | RECOMMENDED. Number of volumes ("dummy scans") discarded by the scanner (as opposed to those discarded by the user post hoc) before saving the imaging file. For example, a sequence that automatically discards the first 4 volumes before saving would have this field as 4. A sequence that doesn't discard dummy scans would have this set to 0. Please note that the onsets recorded in the \_event.tsv file should always refer to the beginning of the acquisition of the first volume in the corresponding imaging file - independent of the value of `NumberOfVolumesDiscardedByScanner` field. |
| NumberOfVolumesDiscardedByUser    | RECOMMENDED. Number of volumes ("dummy scans") discarded by the user before including the file in the dataset. If possible, including all of the volumes is strongly recommended. Please note that the onsets recorded in the \_event.tsv file should always refer to the beginning of the acquisition of the first volume in the corresponding imaging file - independent of the value of `NumberOfVolumesDiscardedByUser` field.                                                                                                                                                                       |
| DelayTime                         | RECOMMENDED. User specified time (in seconds) to delay the acquisition of data for the following volume. If the field is not present it is assumed to be set to zero. Corresponds to Siemens CSA header field `lDelayTimeInTR`. This field is REQUIRED for sparse sequences using the `RepetitionTime` field that do not have the `SliceTiming` field set to allowed for accurate calculation of "acquisition time". This field is mutually exclusive with `VolumeTiming`.                                                                                                                               |
| AcquisitionDuration               | RECOMMENDED. Duration (in seconds) of volume acquisition. Corresponds to DICOM Tag 0018,9073 `Acquisition Duration`. This field is REQUIRED for sequences that are described with the `VolumeTiming` field and that not have the `SliceTiming` field set to allowed for accurate calculation of "acquisition time". This field is mutually exclusive with `RepetitionTime`.                                                                                                                                                                                                                              |
| DelayAfterTrigger                 | RECOMMENDED. Duration (in seconds) from trigger delivery to scan onset. This delay is commonly caused by adjustments and loading times. This specification is entirely independent of `NumberOfVolumesDiscardedByScanner` or `NumberOfVolumesDiscardedByUser`, as the delay precedes the acquisition.                                                                                                                                                                                                                                                                                                    |

The following table recapitulates the different ways that specific fields have
to be populated for functional sequences. Note that all those options can used
for non sparse sequences but that only options B, D and E are valid for sparse
sequences.

|          | RepetitionTime  | SliceTiming  | AcquisitionDuration | DelayTime | VolumeTiming |
|----------|:---------------:|:------------:|:-------------------:|:---------:|:------------:|
| option A |       \[ X ]    |              |         \[ ]        |           |      \[ ]    |
| option B |       \[ ]      |     \[ X ]   |                     |    \[ ]   |      \[ X ]  |
| option C |       \[ ]      |              |         \[ X ]      |    \[ ]   |      \[ X ]  |
| option D |       \[ X ]    |     \[ X ]   |         \[ ]        |           |      \[ ]    |
| option E |       \[ X ]    |              |         \[ ]        |    \[ X ] |      \[ ]    |

**Legend**
-   \[ X ] --> has to be filled
-   \[   \] --> has to be left empty
-   empty cell --> can be specified but not required

##### fMRI task information

| Field name      | Definition                                                                                                                                                                                                  |
| :-------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Instructions    | RECOMMENDED. Text of the instructions given to participants before the scan. This is especially important in context of resting state fMRI and distinguishing between eyes open and eyes closed paradigms.  |
| TaskDescription | RECOMMENDED. Longer description of the task.                                                                                                                                                                |
| CogAtlasID      | RECOMMENDED. URL of the corresponding [Cognitive Atlas](https://www.cognitiveatlas.org/) Task term.                                                                                                         |
| CogPOID         | RECOMMENDED. URL of the corresponding [CogPO](http://www.cogpo.org/) term.                                                                                                                                  |

See [Common metadata fields](#common-metadata-fields) for a list of
additional terms and their definitions.

Example:

```Text
sub-control01/
    func/
        sub-control01_task-nback_bold.json
```

```JSON
{
   "TaskName": "N Back",
   "RepetitionTime": 0.8,
   "EchoTime": 0.03,
   "FlipAngle": 78,
   "SliceTiming": [0.0, 0.2, 0.4, 0.6, 0.0, 0.2, 0.4, 0.6, 0.0, 0.2, 0.4, 0.6, 0.0, 0.2, 0.4, 0.6],
   "MultibandAccelerationFactor": 4,
   "ParallelReductionFactorInPlane": 2,
   "PhaseEncodingDirection": "j",
   "InstitutionName": "Stanford University",
   "InstitutionAddress": "450 Serra Mall, Stanford, CA 94305-2004, USA",
   "DeviceSerialNumber": "11035"
}
```

If this information is the same for all participants, sessions and runs it can
be provided in `task-<label>_bold.json` (in the root directory of the
dataset). However, if the information differs between subjects/runs it can be
specified in the
`sub-<label>/func/sub-<label>_task-<label>[_acq-<label>][_run-<index>]_bold.json` file.
If both files are specified fields from the file corresponding to a particular
participant, task and run takes precedence.

### Diffusion imaging data

Template:

```Text
sub-<label>/[ses-<label>/]
    dwi/
       sub-<label>[_ses-<label>][_acq-<label>][_dir-<label>][_run-<index>]_dwi.nii[.gz]
       sub-<label>[_ses-<label>][_acq-<label>][_dir-<label>][_run-<index>]_dwi.bval
       sub-<label>[_ses-<label>][_acq-<label>][_dir-<label>][_run-<index>]_dwi.bvec
       sub-<label>[_ses-<label>][_acq-<label>][_dir-<label>][_run-<index>]_dwi.json
       sub-<label>[_ses-<label>][_acq-<label>][_dir-<label>][_run-<index>]_sbref.nii[.gz]
       sub-<label>[_ses-<label>][_acq-<label>][_dir-<label>][_run-<index>]_sbref.json
```

Diffusion-weighted imaging data acquired for that participant. The OPTIONAL
`acq-<label>` key/value pair corresponds to a custom label the user may use to
distinguish different set of parameters. For example this should be used when a
study includes two diffusion images - one single band and one multiband. In such
case two files could have the following names:
`sub-01_acq-singleband_dwi.nii.gz` and `sub-01_acq-multiband_dwi.nii.gz`,
however the user is free to choose any other label than `singleband` and
`multiband` as long as they are consistent across subjects and sessions. For
multiband acquisitions, one can also save the single-band reference image as
type `sbref` (e.g. `dwi/sub-control01_sbref.nii[.gz]`) The bvec and bval files
are in the [FSL format](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FDT/UserGuide#DTIFIT):
The bvec files contain 3 rows with n space-delimited floating-point numbers
(corresponding to the n volumes in the relevant NIfTI file). The first row
contains the x elements, the second row contains the y elements and third row
contains the z elements of a unit vector in the direction of the applied
diffusion gradient, where the i-th elements in each row correspond together to
the i-th volume with `[0,0,0]` for non-diffusion-weighted volumes. Inherent to
the FSL format for bvec specification is the fact that the coordinate system of
the bvecs is with respect to the participant (i.e., defined by the axes of the
corresponding dwi.nii file) and not the magnet’s coordinate system, which means
that any rotations applied to dwi.nii also need to be applied to the
corresponding bvec file.

bvec example:

```Text
0 0 0.021828 -0.015425 -0.70918 -0.2465
0 0 0.80242 0.22098 -0.00063106 0.1043
0 0 -0.59636 0.97516 -0.70503 -0.96351
```

The bval file contains the b-values (in s/mm<sup>2</sup>) corresponding to the
volumes in the relevant NIfTI file), with 0 designating non-diffusion-weighted
volumes, space-delimited.

bval example:

```Text
0 0 2000 2000 1000 1000
```

`.bval` and `.bvec` files can be saved on any level of the directory structure
and thus define those values for all sessions and/or subjects in one place (see
Inheritance principle).

See [Common metadata fields](#common-metadata-fields) for a list of
additional terms that can be included in the corresponding JSON file.

JSON example:

```JSON
{
  "PhaseEncodingDirection": "j-",
  "TotalReadoutTime": 0.095
}
```

### Fieldmap data

Data acquired to correct for B0 inhomogeneities can come in different forms.
B0 (static magnetic field strength pattern), B1+ (transmit field pattern),
and B1- (receive field pattern; not yet supported) maps can be useful in
post-processing both raw functional and anatomical data.

B0 maps are primarily used to correct for spatial distortions in functional
data acquired with EPI sequences.

B1+ and B1- maps are mostly used in anatomical imaging, especially when
applying quantitative MRI (qMRI) techniques.

The current version of this standard considers four different scenarios. Please
note that in all cases fieldmap data can be linked to a specific scan(s) it was
acquired for by filling the IntendedFor field in the corresponding JSON file.


#### B0 fieldmaps

Data acquired to correct spatial distortions due to B0 inhomogeneities can come in
different forms. The current version of this standard considers four different
scenarios. Please note that in all cases fieldmap data can be linked to a specific
scan(s) it was acquired for by filling the IntendedFor field in the corresponding
JSON file.

For example:

```JSON
{
   "IntendedFor": "func/sub-01_task-motor_bold.nii.gz"
}
```

The IntendedFor field may contain one or more filenames with paths relative to
the subject subfolder. The path needs to use forward slashes instead of backward
slashes. Here’s an example with multiple target scans:

```JSON
{
   "IntendedFor": ["ses-pre/func/sub-01_ses-pre_task-motor_run-1_bold.nii.gz",
                   "ses-post/func/sub-01_ses-post_task-motor_run-1_bold.nii.gz"]
}
```

The IntendedFor field is OPTIONAL and in case the fieldmaps do not correspond to
any particular scans it does not have to be filled.

Multiple fieldmaps can be stored. In such case the `_run-1`, `_run-2` should be
used. The OPTIONAL `acq-<label>` key/value pair corresponds to a custom label
the user may use to distinguish different set of parameters.

#### Case 1: Phase difference image and at least one magnitude image

Template:

```Text
sub-<label>/[ses-<label>/]
    fmap/
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_phasediff.nii[.gz]
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_phasediff.json
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_magnitude1.nii[.gz]
```

OPTIONAL

```Text
sub-<label>/[ses-<label>/]
    fmap/
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_magnitude2.nii[.gz]
```

This is a common output for build in fieldmap sequence on Siemens scanners. In
this particular case the sidecar JSON file has to define the Echo Times of the
two phase images used to create the difference image. `EchoTime1` corresponds to
the shorter echo time and `EchoTime2` to the longer echo time. Similarly
`_magnitude1` image corresponds to the shorter echo time and the OPTIONAL
`_magnitude2` image to the longer echo time. For example:

```JSON
{
   "EchoTime1": 0.00600,
   "EchoTime2": 0.00746,
   "IntendedFor": "func/sub-01_task-motor_bold.nii.gz"
}
```

#### Case 2: Two phase images and two magnitude images

Template:

```Text
sub-<label>/[ses-<label>/]
    fmap/
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_phase1.nii[.gz]
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_phase1.json
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_phase2.nii[.gz]
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_phase2.json
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_magnitude1.nii[.gz]
        sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_magnitude2.nii[.gz]
```

Similar to the case above, but instead of a precomputed phase difference map two
separate phase images are presented. The two sidecar JSON file need to specify
corresponding `EchoTime` values. For example:

```JSON
{
   "EchoTime": 0.00746,
   "IntendedFor": "func/sub-01_task-motor_bold.nii.gz"
}
```

#### Case 3: A real fieldmap image

Template:

```Text
sub-<label>/[ses-<label>/]
    fmap/
       sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_magnitude.nii[.gz]
       sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_fieldmap.nii[.gz]
       sub-<label>[_ses-<label>][_acq-<label>][_run-<index>]_fieldmap.json
```

In some cases (for example GE) the scanner software will output a precomputed
fieldmap denoting the B0 inhomogeneities along with a magnitude image used for
coregistration. In this case the sidecar JSON file needs to include the units of
the fieldmap. The possible options are: `Hz`, `rad/s`, or `Tesla`. For example:

```JSON
{
   "Units": "rad/s",
   "IntendedFor": "func/sub-01_task-motor_bold.nii.gz"
}
```

#### Case 4: Multiple phase encoded directions ("pepolar")

Template:

```Text
sub-<label>/[ses-<label>/]
    fmap/
        sub-<label>[_ses-<label>][_acq-<label>][_ce-<label>]_dir-<label>[_run-<index>]_epi.nii[.gz]
        sub-<label>[_ses-<label>][_acq-<label>][_ce-<label>]_dir-<label>[_run-<index>]_epi.json
```

The phase-encoding polarity (PEpolar) technique combines two or more Spin Echo
EPI scans with different phase encoding directions to estimate the underlying
inhomogeneity/deformation map. Examples of tools using this kind of images are
FSL TOPUP, AFNI 3dqwarp and SPM. In such a case, the phase encoding direction is
specified in the corresponding JSON file as one of: `i`, `j`, `k`, `i-`, `j-`,
`k-`. For these differentially phase encoded sequences, one also needs to
specify the Total Readout Time defined as the time (in seconds) from the center
of the first echo to the center of the last echo (aka "FSL definition" - see
[here](https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/topup/Faq#How_do_I_know_what_phase-encode_vectors_to_put_into_my_--datain_text_file.3F) and
[here](https://lcni.uoregon.edu/kb-articles/kb-0003) how to calculate it). For
example

```JSON
{
   "PhaseEncodingDirection": "j-",
   "TotalReadoutTime": 0.095,
   "IntendedFor": "func/sub-01_task-motor_bold.nii.gz"
}
```

`label` value of `_dir-` can be set to arbitrary alphanumeric label (`[a-zA-Z0-9]+` for
example `LR` or `AP`) that can help users to distinguish between different
files, but should not be used to infer any scanning parameters (such as phase
encoding directions) of the corresponding sequence. Please rely only on the JSON
file to obtain scanning parameters. \_epi files can be a 3D or 4D - in the
latter case all timepoints share the same scanning parameters. To indicate which
run is intended to be used with which functional or diffusion scan the
IntendedFor field in the JSON file should be used.

#### B1+ fieldmaps

Template:

```Text
sub-<participant_label>/[ses-<session_label>/]
    fmap/
        sub-<participant-label>[_ses-<session_label>][_acq-<acq-label>][_run-<run_index>]_B1plusmap.nii[.gz]
        sub-<participant-label>[_ses-<session_label>][_acq-<acq-label>][_run-<run_index>]_B1plusmap.json
```

```JSON
{
   "PulseSequenceType":"DREAM",
   "IntendedFor": ["anat/sub-01_inv-1_part-mag_MP2RAGE.nii.gz",
                   "anat/sub-01_inv-1_part-phase_MP2RAGE.nii.gz",
                   "anat/sub-01_inv-2_part-mag_MP2RAGE.nii.gz",
                   "anat/sub-01_inv-2_part-phase_MP2RAGE.nii.gz"]
}
```

B1+ fieldmaps quantify the ratio between the _actual_ flip angle and the _intended_
flip angle that is transmited across the different locations in the image.
The image is thus 3D and its values should mostly be close to 1.  B1+
fieldmaps are stored in the `fmap`-folder and use the suffix `_B1plusmap`.
The `IntendedFor`-field in the JSON file can be used to indicate which
images it is intended to be be used with.
