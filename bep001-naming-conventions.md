Template:

```
sub-<participant_label>/[ses-<session_label>/]
    anat/
        sub-<participant_label>_<modality_label>.json
        sub-<participant_label>[_ses-<session_label>][_indexable_metadata-<index>][_acq-<label>][_part-<mag/phase>][_ce-<label>][_rec-<label>][_run-<index>]_<modality_label>.nii[.gz]
        sub-<participant_label>[_ses-<session_label>][_indexable_metadata-<index>][_acq-<label>][_part-<mag/phase>][_ce-<label>][_rec-<label>][_run-<index>]_<modality_label>.json
```

A naming convention that can logically group parametrically linked anatomy imaging datasets can be achieved by combining following key/value pairs:

1. Suffix (a.k.a modality_label)               (REQUIRED)
2. [\_indexable\_metadata-<index>]             (RECOMMENDED)
3. [\_acq-<label>]                             (RECOMMENDED)
4. [\_part-<mag/phase>]                        (OPTIONAL)


### Suffix

Quantitative MRI (qMRI) techniques can be referred by i) sequence names, ii) outputs they generate or iii) special acronyms. As a result, a list of qMRI methods (values) cannot be semantically linked to a single key. To free this field from a key tag, names of the qMRI methods are stored in the `suffix` (a.k.a `<modality_label>`). If several scans of the same modality are intended to create a quantitative map, they MUST be labeled by `suffix`.

| Suffix  | Method Name                              | Output Name                    |
|---------|------------------------------------------|--------------------------------|
| VFA     | Variable Flip Angle                      | T1Map                          |
| IRT1    | Inversion Recovery                       | T1Map                          |
| MP2RAGE | Magnetization Prepared 2 Gradient Echoes | T1Map, UNIT1                   |
| MESET2  | Multi-Echo Spin Echo                     | MWF, T2Map                     |
| MTR     | Magnetization Transfer Ratio             | MTRmap                         |
| MTS     | Magnetization Transfer Saturation        | MTsat, T1map                   |
| MPM     | Multi-Parameter Mapping (hMRI)           | R1map, R2starmap, MTsat, PDmap |
| SESL    | Spin Echo-Spin Lock                      | T1Rmap                         |

### `indexable_metadata-<index>` key-value pair

If the grouping logic of several scans of the same modality is (entirely or partially) bound up with a metadata field (which varies between scans), `indexable_metadata-<index>` SHOULD be included in the file name. This is applicable if the varying entries of the same metadata field are enumerable. Unlike other key-value pairs, key tag of the `indexable_metadata-<index>` is mutable depending on the metadata field that varies between several scans of the same modality intended to create quantitative map(s):

| Key tag | Value list | Associated metadata |
|---------|------------|---------------------|
| fa      | 1,2,... N  | FlipAngle           |
| inv     | 1,2,... N  | InversionTime       |
| echo    | 1,2,... N  | EchoTime            |
| tsl     | 1,2,... N  | SpinLockTime        |

### `acq-<label>` key-value pair

If the grouping logic of several scans of the same modality is (entirely or partially) bound up with a metadata field (which varies between scans), `acq-<label>` SHOULD be included in the file name. This is applicable if the varying entries of the same metadata field are categorical. Note that value of the `acq-<label>` is free form. However, to enable a unified naming convention while combining several scans of the same modality intended to create quantitative map(s), following labels SHOULD be included in the filename where applicable:

| Method Name | Labels           | Respective metadata fields   |
|-------------|------------------|------------------------------|
| MTR         | MTon, MToff      | MTC (Sequence variant attr.) |
| MTS         | MTon, MToff, T1w | MTC (Sequence variant attr.) |
| MPM         | MTon, MToff, T1w | MTC (Sequence variant attr.) |

### `part-<mag/phase>` key-value pair

See DICOM tag (0008,0008) Image Type. Allows for storing phase and magnitude images. In its absence the image is assumed to be magnitude. Phase images should be in radians and have a range of (0, 2 pi] (including 0, excluding 2 pi).

### Description of the quantitative maps 

| Name      | Description                                  | Units |
|-----------|----------------------------------------------|-------|
| T1map     | Longitudinal relaxation time map             | ms    |
| T2map     | True transverse relaxation time map          | ms    |
| T2starmap | Observed transverse relaxation time map      | ms    |
| R1map     | Longitudinal relaxation rate map             | 1/ms  |
| R2map     | Transverse relaxation rate map               | 1/ms  |
| R2starmap | Observed transverse relaxation rate map      | 1/ms  |
| MTRmap    | Magnetization transfer ratio map             | %     |
| MTsat     | Magnetization transfer saturation map        | %     |
| PDmap     | Proton density map                           | N/A   |
| T1Rmap    | T1-rho (T1 relaxation in rotating frame) map | ms    |
| UNIT1     | Homogeneous (flat) T1w image                 | N/A   |
