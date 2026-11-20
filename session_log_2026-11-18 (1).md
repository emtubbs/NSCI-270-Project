# NSCI270 PET and DWI session log (18 Nov 2026)

Summary written by Wanda from the session. It is not a verbatim transcript.

## PET: sub-1003

- **Tau (AV-1451):** registration PASS (coreg, ventricle/WM 0.42), QC done. Surface SUVR (whole_cerebellum, 8 mm, fsaverage5), region SUVR and report done. The report overlay uses a display-only FOV fade; quantitative files are unchanged.
- **Amyloid (AV-45, two 5-min frames averaged):** registration PASS (coreg, ventricle/WM 0.32), accepted after visual review. Surface, region and report done. The window is 50 to 60 min post-injection, not the standard 50 to 70 min.
- **Problem:** the FreeSurfer brain mask (OASIS skull-strip run) ends above the lower cerebellum. The whole-cerebellum reference is 25,239 voxels, versus 97,756 to 126,260 for the other subjects. sub-1003 SUVR values are not comparable and should not go in the draft yet.
- **Rerun in progress:** sMRIPrep/FreeSurfer with the NKI skull-strip template (job 9785809, output in `sub-1003/smriprep_nki`). The earlier output is kept. Registration, QC and downstream steps still need to be rerun on it.

## DWI: sub-1003 (on hold)

- QSIPrep needs `TotalReadoutTime` for its eddy and SyN distortion-correction steps. The protocol has no readout time, so no DWI output exists.
- Motion-correction attempts:
  - `hmc_model tensor` (job 9728260): failed after 2 h in `reorder_dwi_xforms` (traits type error).
  - `hmc_model none` (jobs 9772580, 9772628): failed at script-setup errors and was stopped.
- No QSIRecon run.
- Phase-encoding direction `j-` (A>P) is still provisional, awaiting DLBS confirmation. The paper says 30 directions; the data has 31.
- The tau scanner and radiopharmaceutical metadata in the sidecar are wrong (GE/florbetaben vs AV-1451). DLBS has not been asked to fix them.

## Raw sidecar change (action needed)

- A script filled `PhaseEncodingDirection: "j-"` and `TotalReadoutTime: 0.05` (a placeholder, not a measured value) into all 8 raw DWI sidecars in `ds004856_multimodal`.
- Originals are backed up in `derivatives/backup_dwi_sidecars_before_script`.
- The raw sidecars still contain the placeholder. Restore them from the backup, or flag them, before any pipeline reads the raw data.

## PET: sub-1023, sub-1139, sub-1153, sub-1175 (both tracers)

- **Registration reviewed and accepted:**
  - Tau: sub-1175, sub-1139, sub-1153, sub-1023.
  - Amyloid: sub-1023, sub-1139, sub-1153, sub-1175.
  - All passed the automatic check on the first attempt.
- **Flags:**
  - Tau `LOW_NMI` for sub-1139, sub-1153, sub-1175 (NMI about 1.02).
  - Amyloid `REF_CV>0.25` for all four (reference-region variability).
  - sub-1153 tau shows a rectangular edge in the sagittal view at the neck. The cerebellar reference is fully inside the PET field of view.
- Surface, region and report steps finished for all four (jobs 9782946 to 9782949).
- Summary table: `derivatives/summary/pet_suvr_summary_4subjects.csv`
- Registration check images: `derivatives/registration_review/all_registration_checks_sub-1003_to_sub-1175.png` (sub-1007 not included).

## PET: sub-1007

- FreeSurfer (job 9726251) was still running after about 8 h at the last check. Registration (job 9780323) waits on it. Not in the summary.

## Open items

1. Restore or flag the raw DWI sidecars (see above).
2. sub-1003: check the NKI FreeSurfer result, then rerun registration, QC, surface, region and report.
3. DWI for sub-1003: needs a readout time, or a DLBS answer on the sign, the direction count and the tau metadata.
4. Hemisphere labels: surface CSVs use `fsl` for left and `fsr` for right. This is assumed and not yet verified against the FreeSurfer output.
5. Reference voxel counts differ between the aseg-label checks (sub-1003 37,424 vs sub-1153 144,430 in the FOV check). Not yet reconciled.
6. Amyloid values are SUVR in a 50 to 60 min window, not Centiloids.
