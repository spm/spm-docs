# MEG/EEG group analysis

By this point in the course, you are already quite seasoned neuroimaging data analysts and you have generated several types of within-subject results. The final step is to perform group-level statistics on these results, to make inferences that generalise beyond the specific subjects you have tested.

You are already familiar with the concept of second-level (group) analyses from the fMRI part of the course. The same principles apply to M/EEG data, whether at the sensor level (e.g., scalp-time images) or at the source level (e.g. source images). 

You can now pool the results per condition from the following modalities

- Sensor-level evoked responses (scalp-time images) for EEG
- Single-channel time-frequency images for MEG
- Source images (MEG+EEG) for IID.
- Source images (MEG+EEG) for MSP(GS)

Copy the files for your subject for the three original conditions (Familiar, Unfamiliar and Scrambled) and the two contrasts (Faces vs Scrambled and Familiar vs Unfamiliar) to the corresponding group analysis folders in `S:\FBS_CLNE0068_students\`.

You can now use the 2nd-level designs you are already familiar with to answer different research questions about the datasets.

Examples of possible research questions include:

1. Are there significant differences between responses to Familiar and Unfamiliar faces? Where are they localised in space, time and frequency?
2. How are responses to visual stimuli or differential responses to Faces vs Scrambled or differential responses to Familiar vs Unfamiliar faces affected by age and gender?
3. What aspects of the responses are correlated with various psychological and clinical scores such as the ones you used for your pre-registered presentation? Although we know that these are only mock scores you could set up a pipeline to answer this question and see what would be the effect of your statistical choices on the amount of false positive results you get.

Feel free to come up with your own questions and design an SPM pipeline to answer them. You now have sufficient knowledge to analyse open datasets that you can find on the web or data you might get access to as part of your lab rottations or research projects.

> ## Assessment question
>
> ### Effects of Statistical Thresholding on GLM Results (M/EEG)
><br>
>Set up a General Linear Model (GLM) comparing responses to faces versus scrambled faces. Choose one analysis domain (e.g. single channel evoked response, time-frequency images, scalp-time maps or source reconstructions).<br>
>
>Use the uncorrected (‘none’) thresholding option and systematically vary:
> - Cluster-forming threshold: test values between 0.001 and 0.1 <br>
> - Cluster extent threshold: select appropriate values based on observed cluster sizes<br>
>
>For each parameter set, rerun the analysis and document how threshold choices affect the detection and size of significant clusters. Discuss the implications of liberal versus conservative thresholds and highlight the balance between sensitivity and false-positive risk.

