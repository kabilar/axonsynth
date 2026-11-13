# Paper Analysis

The fixed analysis compares four methods on two calibration and ten held-out
LSM patches:

- three-class v2;
- original binary ablation;
- calibrated normalized-intensity threshold;
- bright-only Frangi.

## Data

The 12 raw patches and ground-truth masks are published in the LSM Dandiset
under BIDS names, e.g.
`derivatives/micr/sub-PH1/sub-PH1_stain-NEFH_seg-GM_desc-raw_SPIM.nii.gz`
and `..._desc-gt_SPIM.nii.gz`. Download the Dandiset and set `DATA_ROOT` to
its root (the directory that contains `derivatives/`); no renaming is needed.
Each patch in `config/lsm_12patch.json` lists the `subject`, `stain`, and
`seg` entities of its files, so `human_NEFH/Human_NEFH_GM` reads
`sub-PH1_stain-NEFH_seg-GM`. Patch IDs, output paths, and reported tables keep
the paper's patch names.

## Running

Copy the exported path template in `config/site.example.env` to the ignored
`config/site.env`, source that file, and submit the learned
inference and baseline arrays. After both arrays complete, run the evaluation,
depth, overlay, and packet jobs in `run/paper/`.

Raw metrics are reported alongside
`target_informed_neighbor_corrected_*`. The latter uses the target to
apply a two-round face-neighbor evaluation tolerance and is not deployable
prediction postprocessing.

Raw metrics are the primary reported comparison. The accepted learned-model
and intensity operating points were historically selected by target-informed
corrected Dice on each domain's calibration patch; Frangi was selected by raw
Dice. The criterion is retained and labeled rather than retrospectively
changing the accepted thresholds.

Inference and baseline sidecars record the input, checkpoint, output paths, and
fixed method settings used for each case. Evaluation also checks dimensions,
affines, finite score ranges, and agreement between each score and saved mask.
