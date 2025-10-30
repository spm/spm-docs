# fMRI analysis assessment questions

## 1. Effects of spatial smoothing on GLM results

There is no universally agreed-upon amount of spatial smoothing to apply prior to a statistical analysis, so it is useful to see the effects in practice. Run a single-subject statistical analysis on the same subject, but with different degrees of isotropic smoothing (same full-width at half-maximum in all three directions). Suggested values:

- No smoothing
- 4 mm smoothing
- 8 mm smoothing
- 12 mm smoothing

For each smoothed dataset, using an identical statistical model, obtain the statistical results and note the `Height threshold` shown at the bottom of the results figure. Plot a simple graph showing how the height threshold varies with the amount of smoothing applied. Answer these questions:

- Why does the threshold change with smoothing?
- Is there a correct amount of smoothing to apply in general?
- What amount of smoothing seems best for these data and why?

---

## 2. Effects of the amount of data

For data from a single subject, run a statistical analysis using three different subsets of the data:

- All the data
- Only the first half of the data
- Only the second half of the data (adjust timings accordingly)

Compare the resulting statistical maps. Consider these questions:

- How do results differ when more or fewer scans are included?
- How might the results change if even more scans were acquired?
- If no statistically significant differences are found in a brain region, does that imply the region is not involved in the task? (Hint: consider power and sensitivity.)

---

## 3. Alternative processing pipelines

Multi-subject analyses usually generate a first-level contrast image per subject, followed by a second-level analysis across subjects. Compare two different pipelines for generating a first-level contrast image from a single individual:

1. Preprocess the fMRI dataset first: motion correction (realign & reslice), fMRI–T1 coregistration, spatial normalisation, then spatial smoothing of the fMRI dataset. After preprocessing, run the statistical analysis to generate the contrast image. (Do not use masking in the analysis.)

2. First do motion correction and fMRI–T1 coregistration, then run the statistical analysis to generate the contrast image (no masking). After the contrast image is produced, spatially normalise and smooth the contrast image.

Questions to address:

- Is there any difference between the resulting smoothed & spatially normalised contrast images?
- Use SPM's ImCalc (or similar) to compute the difference between the two images and inspect it.
- What processing differences might cause any observed discrepancies?
- Which strategy is easier to implement and why?

---

## 4. Effects of statistical thresholding on GLM results (fMRI)

Set up a GLM comparing responses to faces versus scrambled faces. Use the uncorrected ('none') thresholding option and systematically vary the following parameters:

- Cluster-forming threshold: test values in the range 0.001 to 0.1
- Cluster extent threshold: choose appropriate values based on observed cluster sizes

For each parameter set, rerun the analysis and document how the threshold choices affect the detection and size of significant clusters. Discuss:

- The implications of using liberal versus conservative thresholds
- The trade-off between sensitivity and false-positive risk



