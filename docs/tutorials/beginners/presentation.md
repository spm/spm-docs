# Tips on preparing your presentation

Once you have submitted your preregistration document and received the mock score values, you should do a second-level analysis using the images in the
`S:\FBS_CLNE0068_students\Faces 2nd level` folder and the statistical model described in your preregistration.

You should look at the effects associated with your assigned score values in 3 ways:

1. **FWE whole brain-corrected results** - use the FWE option with the appropriate threshold. Report any significant peaks.
2. **Uncorrected results** - use an uncorrected threshold of p < 0.05 ('none' option). Report any clusters/peaks that would be significant at this threshold. If you have too many clusters/peaks, you can use a more stringent uncorrected threshold (e.g. p < 0.01) to reduce the number of results reported. If you have too few you can use a more lenient threshold (e.g. p < 0.1). 
3. **Small volume correction (SVC)** - look at the areas listed in your preregistration. You could use the atlas functionality you saw in one of the tutorials or the 'small volume' button in SPM results GUI where you'll be able to enter the coordinates you pre-registered and look at a small sphere around them (e.g. 8 mm radius). Report any significant peaks.

Use web search or AI tools as you did for the preregistration to find relevant references for the peaks you found (if they are outside the pre-registered areas). Make sure to check and cite the original papers. You can use the [MNI ←→ Talairach Converter with Brodmann Areas](https://bioimagesuiteweb.github.io/webapp/mni2tal.html) to figure out the anatomical labels from the MNI coordinates of your peaks. If you don't find any relevant papers, this is not a problem - just report that.

Summarise the results in your presentation. Explain and motivate any methods choices you had to make. Discuss what you learned from this whole exercise.

Make sure you report the following:

1. The threshold you used for whole-brain FWE correction and how many distinct blobs you found.
2. The uncorrected threshold you used and how many clusters/peaks you found at that threshold.
3. For how many of these peaks/clusters you found relevant literature references.
4. What threshold you used for small volume correction and how many significant peaks you found in your pre-registered areas.


> **Tip:** Remember that the association with the score could be either positive or negative - check both directions. (F test or two separate t tests could be used).

> **Tip:** For each variant of the analysis described above, how many different tests did you run? What should the statistical threshold be to take that into account (no need to correct between the three variants)?

> **Tip:** Think about the dead salmon paper and how it is relevant.

> **Bonus question:** Some of you might get the visual areas in your results. Why could that be? Does the fact that an actvation pattern 'makes sense' necessarily mean that it's real?




