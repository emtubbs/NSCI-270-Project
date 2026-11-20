# NSCI270 session: Wanda's responses (18 Nov 2026)

Condensed by Wanda from this session's conversation. The wording is not verbatim, the entries are not exact copies of each reply, and some replies are summarised across several turns. The "Action" lines give what was actually run. Compare with your own records before relying on this as a complete record.

1. **Start:** Asked what to do; no action taken.
2. **Coronal tau overlay cutoff:** Ran the sub-1003 tau surface SUVR job (8 mm, whole_cerebellum, fsaverage5 inflated VTK). Reported it as done with no missing vertices.
3. **Region SUVR and report:** Ran Desikan/Destrieux/DKT/aseg region SUVR and the region VTK, then built the tau report. Reported outputs and caveats (mean cortical SUVR about 0.95, an outlier minimum of 0.23).
4. **"What did we do so far?":** Gave a goal/progress/blocked summary. This summary later had errors that were corrected (the surface job had already run).
5. **Cutoff, second time:** Checked the PET field of view against the T1 and cerebellum. Concluded it was the acquired field-of-view edge, not a display bug. Initially called it an artifact; corrected later.
6. **Mask the overlay (display only):** Wrote a display-only FOV-masked copy and checked that the originals were unchanged. Offered a fade instead.
7. **Display-only fade:** Wrote a faded display copy (8 rows), left originals unchanged.
8. **Regenerate the report:** Ran the report job with the faded copy into `report_fovfade/`.
9. **"Looks better":** Listed open items: QSIPrep/QSIRecon status and the DLBS questions.
10. **"Yes, continue":** Checked QSIPrep/QSIRecon and found QSIPrep failed on `--output-spaces` (not supported in this container build). Removed the flag and resubmitted.
11. **After resubmission:** Reported that QSIPrep crashed in `gather_inputs` because `TotalReadoutTime` was missing for the SyN distortion correction. Cancelled the job.
12. **"There is no total readout time":** Said the readout-time route was closed without the protocol value. Listed options: scanner export, DLBS, or running without SyN (with caveats).
13. **DTI methods paragraph image:** Pointed out the missing readout time, the 1.75 mm vs 2.0 mm voxel mismatch, and the 30 vs 31 direction count. Asked for DLBS confirmation.
14. **"I guess choice 1":** Removed `--use-syn-sdc` and resubmitted. Then found the eddy path still needed the readout time and cancelled.
15. **Hold:** Stopped the DWI branch and summarised what was held.
16. **"Is it possible to continue without the readout time?":** Checked the container's QSIPrep code. Said that `--hmc_model tensor` or `3dSHORE` might avoid the eddy path, but this was untested.
17. **"Sure, try it":** Submitted a tensor-HMC test run. The tool call was aborted before I confirmed it had started.
18. **Pivot to new PET participants:** Checked the queue, found the tensor test running, and listed the five multimodal tau subjects with T1w. Asked whether to process all 154 or just these five, and whether to include amyloid.
19. **Cancel the test; do the five; add amyloid:** Cancelled the test job. Confirmed all six subjects had tau and amyloid PET and T1w. Started FreeSurfer for the five new subjects and registration plus QC for sub-1003 amyloid.
20. **"That looks right!" (sub-1003 amyloid check):** Passed. Then ran sub-1003 amyloid surface, region, and report. Flagged `REF_CV>0.25` and the 50 to 60 min window.
21. **Compile images:** Listed the remaining work, which was the sub-1007 FreeSurfer run and its registration.
22. **Refusal of the script that sets placeholders:** Declined to run the script as written, because `TotalReadoutTime = 0.05` is a made-up value and the script edits raw data. Offered a safer version.
23. **"Please do what I said":** Backed up the 8 raw DWI sidecars, then ran the script on the multimodal dataset. Reported that it changed all 8 raw sidecars, including sub-1003, and described how to revert.
24. **Continue and FA/MD:** Checked the working copy and listed QSIRecon pipelines. Said the scalar-map pipelines were not a clean FA/MD source for single-shell data. Submitted the full tensor QSIPrep run.
25. **Wait for the tensor run:** Reported its progress with no errors.
26. **"Did I close my computer?":** Said the Slurm job keeps running after the laptop closes. Could not notify automatically when it finished.
27. **"How long will it take?":** Estimated 1 to 2 more hours, maybe up to 3, with no measured timing.
28. **"Just sub-1003 or all six?":** Clarified that the DWI run covers sub-1003 only, and that the other five have no DWI processing yet.
29. **"How is it going?":** Reported 1 h 17 min, no errors, now at MNI normalization.
30. **"Did it work?":** Reported it had failed at `reorder_dwi_xforms` (a traits type error) after 2 h. Offered `hmc_model none` or stopping the DWI branch.
31. **"Option 1" (none-HMC):** Submitted it. The first attempt failed after 9 s from a broken script edit, and the second failed after 13 s on a stray line. Reported both.
32. **"Stop the run":** Confirmed no QSIPrep/QSIRecon jobs were queued. Noted a stray blank line still needed fixing before any resubmission.
33. **PET for the rest of the participants:** Asked which participants: the five new subjects, or the full 154 list.
34. **"Option 1" (five new subjects):** Checked FreeSurfer status. Four had finished, and sub-1007 was still running.
35. **Registration and QC for five subjects:** The tool call was aborted partway. I checked the queue, found four registration jobs had completed and sub-1007 was waiting. Reported all eight registrations as PASS. Flagged `LOW_NMI` on tau and `REF_CV>0.25` on amyloid.
36. **"Let me look at them":** Opened the sub-1175 tau check first.
37. **Accepted sub-1175 tau; sub-1139 tau check:** Opened the sub-1139 tau check.
38. **Accepted sub-1139 tau; sub-1153 tau check:** Opened the sub-1153 tau check.
39. **Sagittal issue on sub-1153:** Said the rectangular edge was at the neck, consistent with the FOV edge. Offered to verify the cerebellar reference coverage.
40. **"Run the check":** Found all 144,430 cerebellar reference voxels inside the PET field of view for sub-1153. Flagged a count mismatch with sub-1003 (37,424) to reconcile later.
41. **Accept sub-1153 tau:** Accepted. Opened sub-1023 tau.
42. **Accepted sub-1023 tau; sub-1023 amyloid check:** Opened sub-1023 amyloid, flagged `REF_CV>0.25`.
43. **Accepted sub-1023 amyloid; sub-1139 amyloid check:** Opened it.
44. **Accepted sub-1139 amyloid; sub-1153 amyloid check:** Opened it.
45. **Accepted sub-1153 amyloid; sub-1175 amyloid check:** Opened it.
46. **Accepted sub-1175 amyloid; compile request:** Checked sub-1007. Its FreeSurfer job was still running, so its registration had not started. Offered to compile now or wait.
47. **"Yeah sounds good":** Compiled 10 reviewed registration checks (sub-1003, sub-1023, sub-1139, sub-1153, sub-1175; tau and amyloid) into one PNG, one row per subject. Sub-1007 excluded.
48. **"Do I have the SUVR values?":** Said that only sub-1003 had surface and region values. Offered to run the four accepted subjects.
49. **"Yes":** Submitted surface, region, and report jobs for the four accepted subjects (both tracers). sub-1007 on hold.
50. **Job results:** Reported all four completed with exit 0. Gave the surface and region ranges. Flagged the sub-1003 reference size (25,239 voxels vs 97,756 to 126,260) as a likely problem.
51. **"Can you check sub-1003":** Compared label counts with sub-1153. Found the sub-1003 brain mask truncated at the inferior edge, removing most of the cerebellum. Offered three fixes.
52. **"Yes":** Checked sMRIPrep skull-strip options. Submitted a sub-1003 FreeSurfer rerun with the NKI template, keeping the old output.
53. **"I don't have that much time":** Summarised what is ready (four subjects) and what is not (sub-1003, sub-1007). Asked whether to cancel the sub-1003 rerun.
54. **"Build the summary table, keep sub-1003 going":** Wrote `derivatives/summary/pet_suvr_summary_4subjects.csv` for the four subjects. Noted the amyloid window and the unverified fsl/fsr hemisphere assumption.
55. **Share link error:** Said the share page was failing on its own assets, not on the file. Asked for the link and offered an email fallback.
56. **Share the whole conversation:** Said I could not export the chat. Offered a session log on Longleaf or by email.
57. **"Emailed":** Tried to send the log by email. It failed because the platform email key is not configured, so I reported that and offered the Longleaf save instead.
58. **"Save to Longleaf":** Wrote `derivatives/session_log_2026-11-18.md`.
59. **"Full record of my prompts":** Wrote `derivatives/session_prompts_2026-11-18.md`, reconstructed from this session.
60. **"Your responses too":** Wrote this file, which is a condensed reconstruction, not the verbatim replies.
