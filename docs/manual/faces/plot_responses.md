## Masking F-contrasts

To assess further the effects of stimulus repetition

- Press 'Results', select the SPM.mat file, and select contrast 3, 'Main
  effect of Rep'. This contrast has been created automatically. Then
  press 'Done'.

- *Mask with other contrast ? \[Yes/No\]*

- Specify Yes.

- Select contrast number 5; `Positive effect of condition_1`, press
  'Done'

- Accept the 0.05 default uncorrected mask p-value

- Select 'inclusive' for nature of mask

- *Title for comparison ?*

- Accept what is offered

- *p value adjustment to control: \[FWE/FDR/none\]*

- Select none

- *Threshold F or p value*

- Accept the default value, 0.001

- *Extent threshold {voxels} \[0\]*

- Accept the default value, 0

Specify 'mask with another contrast' (yes), select the 'Canonical HRF:
Faces $>$ Baseline' t-contrast, specify 'threshold for mask' (accept
default of p=0.05 uncorrected), specify 'nature of mask' (inclusive),
specify 'title for comparison' (accept default), specify 'corrected
height threshold' (no), specify threshold (0.001). Again, when the MIP
appears, press 'Volume'.

The MIP shows all clusters where the difference between the parameter
estimates for first and second presentations for the canonical hrf OR
its temporal derivative is significantly different from zero AND the
canonical hrf for all four conditions is significantly larger than zero.
Note that although the mask does not alter the threshold for the target
contrast, the combined probability for a voxel to appear in the masked
contrast is in the order of 0.05 x 0.001 uncorrected due to the
orthogonality of the contrasts.

## Plotting event-related responses

To plot repetition suppression effects for the R fusiform region, select
the third cluster from the top (by clicking 48 60 -15; slightly more
posterior to that identified above) and press 'plot'. Select
'event-related responses'.

- *Which trials or conditions (1 to 6)*

- Specify '1:4'

- *Plot in terms of \...*

- Select fitted response and PSTH

To adjust the scale of the x-axis (time), press 'attrib' from the plot
controls panel. Select Xlim, and change the default (-4 32) to '0 10'.

Note the signal decrease between first and second presentations of both
famous and non-famous faces. Other options from the plot controls panel:

- Hold: allows overlaying of plots

- Grid: toggles between grid on/off

- Box: toggles between axes/box

- Text: allows editing of title and/or axis labels.
