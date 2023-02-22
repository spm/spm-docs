## Group Analysis

For fMRI or EEG data group analysis is also referred to as second level
analysis as the first level is fitting General Linear Models to data
from each subject. The second level then assesses the variability of the
effects over a group of subjects or between groups. SPM relies on the
summary statistic approach in which first level analyses produce
contrast images, summarising the effects for each subject. These images
then enter as data into second level models. For VBM there is no first
level. You simply enter your processed anatomical images directly.

It is recommended that group analysis be implemented in SPM using what
is called the *"partitioned error"* approach. This requires setting up a
second level design matrix to test for each effect of interest.

If you have more than one experimental factor the analysis involves many
steps as there are many effects to test for (due to the combinatorial
nature of factorial designs). We therefore now provide excerpts from
emails on the SPM list to guide your analysis. The answers have been
edited for consistency.

### Example 1: Two Factors

A 2-by-2 design with one within-subject and one between-subject factor.

**Question:**

My experimental set-up looks like this: 2 groups (between-subject
factor), with each 2 conditions (time, a pre and post scan). I have 8
subjects in the first group and 14 in the second. 

Ideally, I would set-up a 2x2 Mixed ANOVA to model the main effects and
interaction effects. I'm assuming it's using the Flexible Factorial?
Could you help on how to precisely specify this? And could you help with
the contrasts or refer to good literature on this? I find a lot of
things online, but none or very limited to a 2x2 mixed ANOVA. I assume I
can also add covariates? 

What would be the best way to do this? I'm doing VBM analysis in SPM
right now, but I guess it's the same approach when using PET or fMRI. 

**Answer:**

What you would do is create two contrast images per subject at the first
level:

- \[1 1\]}}  Overall/average effect  (post plus pre)

- \[1 -1\]}} Difference between condition (post minus pre)

To see where effect (where can be to ) is different between groups
create a two sample t-test design (at the \"second level\") and enter
the con images for group 1 (ie. 8 con images), the con images for group
2 (ie. 14 con images) and enter a F-contrast.

To test for an effect over all subjects, create a one sample t-test
design, enter the con images for both groups, and use a F-contrast.

So you need four different models at the second level and one contrast
in each. This gives you the four things you typically test for in a 2 by
2 design.

You may see a difference (eg. in structure for VBM data) between groups
irrespective of time ( contrast at 2nd level and contrast at first). And
you may see that this difference changes over time ( at 2nd level and at
first, the interaction).

You may wish to mask the latter by the former, and yes, you can enter
covariates in the 2nd level designs.

*Note on purely within-subject designs*

If this were a 2-by-2 design where both factors were within subject you
would create four contrast images per subject: \[1 1 1 1\]}}, \[1 1 -1
-1\]}}, \[1 -1 1 -1\]}}, \[1 -1 -1 1\]}} (overall effect, main effect 1,
main effect 2, interaction). You would then create four second level
designs, each being a one sample t-test, and enter the con images from
all subjects for that particular contrast (only 1 con image per
subject). Then enter a F-contrast to test for the corresponding effect
in your group of subjects.

More generally, if you have a -by- design you can use the
function \<code\>spm_make_contrasts\</code\> to tell you what first
level contrasts to enter (please read the help of that function as it
assumes a specific ordering of conditions in your design matrix):
\<syntaxhighlight lang=\"matlab\"\> Con = spm_make_contrasts(\[k1 k2\])
\</syntaxhighlight\> for a -by- design (eg. 2 by 3) . If the required
contrast has multiple rows you\'ll need to take up multiple con images
to the second level.

### Example 2: Three Factors

A 2-by-3-by-2 design. The first two factors are within-subject, the
second is between-subject.

**Question:**

I have a task design that involves emotional pictures (positive, neutral
and negative) each followed by one of two possible questions (1 and 2).
At the first level, I computed six contrasts per subject, with each
question type separately for each emotional valence, versus baseline.

I now would like to assess the role of the valence and question type
factors as well as a group factor (patient versus control) at the second
level. From what I understood, the easiest option would be a full
factorial model with 3 factors: group, question type and valence. My
questions are:

1\) Can I use a full-factorial model or the use of multiple contrast
from each of the subjects is equivalent to a repeated measures design,
which would mean I have to use a flexible factorial design and model a
"Subject" factor explicitly? 

2\) If I use a full factorial, from what I gather the independence of
the factors should be: group (Yes), question(No), valence(No). What
about the variance? Should it be unequal, equal, equal or just always
unequal? 

3\) Another, perhaps easier, possibility would be to just use two groups
t-tests and assess each of the 6 conditions. So I would have to run 6
t-tests. However if I wanted to, for example, assess the difference
across groups in question 1 regardless of valence, can I simply enter
all the contrasts of question one (that is for positive, neutral and
negative valence) in the "Group 1 scans" box? I assume this would
compute the average of all the scans and compare it across groups, which
is what I would be looking for, but is there any problem in entering 3
scans per subject in the t-test design? Also, suppose I enter age as a
covariate. I then would need to enter the same age 3 times per subject.
Would that be a problem?

**Answer:**

I would analyse this data as described below. Classically, there are 8
effects you might wish to test for (see below) - in what I\'ve described
you set up 4 sets of first level contrast images and two design matrices
for each.  That is, you create 8 new directories for SPM analyses (one
for each effect you are testing for). So, step (1) is to create the
necessary first level con images, step (2) create the second level
design matrices, assign con images and fit the models, step (3) is to
enter the 2nd level contrasts to test for the effects you are interested
in.

I\'m assuming you have two within-subject factors here:

- \<code\>(A) Task\</code\>, with two levels: question 1 or question 2,
- \<code\>(B) Emotional valence\</code\>, with three levels: (1)
  positive, (2) neutral and (3) negative. 

To make this concrete lets say you have 18 people in the patient group
and 17 in the controls, making a total of 35.

Let\'s also say you have set up a first level design matrix for each
subject with columns arranged in the order \<code\>A1B1\</code\>,
\<code\>A1B2\</code\>, \<code\>A1B3\</code\>, \<code\>A2B1\</code\>,
\<code\>A2B2\</code\> and \<code\>A2B3\</code\>.

To test *(1) the overall effect* I would use a \<code\>\[1 1 1 1 1
1\]\</code\> contrast for each subject and take the resulting 35 con
images into a one-sample t-test at the second level. Then you would
specify a \<code\>\[1\]\</code\> F-contrast (at the second level) to
test for significantly non-zero BOLD responses related to your paradigm.
Using the same first level contrast images in a two-sample t-test design
at the second level, split into the 18 patients and 17 controls, will
let you test for group effects (using a second-level F-contrast
\<code\>\[1 -1\]\</code\> to test for differences). This is *(2) the
main effect of group*.

To test for *(3) the main effect of Factor A* (the one with two levels)
I would use a \<code\>\[1 1 1 -1 -1 -1\]\</code\> contrast for each
subject and take the resulting 35 con images into a one-sample t-test at
the second level. Similarly, using the same first level contrast images
in a two-sample t-test design at the second level, split into the 18
patients and 17 controls, will let you test for group effects (using a
second-level F-contrast \<code\>\[1 -1\]\</code\> to test for
differences). This will test for *(4) the group x task interaction*. 

To test for *(5) the main effect of Factor B* I would use two contrasts
per subject \<code\>\[1 -1 0 1 -1 0\]\</code\> and \<code\>\[0 1 -1 0 1
-1\]\</code\> and take the resulting 70 con images (two per subject)
into a two-sample t-test design at the second level. I would then use a
\<code\>\[1 0; 0 1\]\</code\> F-contrast to test for this main
effect. Similarly, using the same first level contrast images in a
one-way ANOVA design at the second level (with 4 \"levels\"; first two
for patients, second two for controls), split into the 18 patients and
17 controls, will let you test for group effects (using a second-level
F-contrast \<code\>\[1 0 -1 0; 0 1 0 -1\]\</code\> to test for
differences). This will test for *(6) the group x valence interaction*. 

To test for *(7) the interaction between Factors A and B* I would use
two contrasts per subject \<code\>\[1 -1 0 -1 1 0\]\</code\> and
\<code\>\[0 1 -1 0 -1 1\]\</code\> and take the resulting 70 con images
(two per subject) into a two-sample t-test design at the second level. I
would then use a \<code\>\[1 0; 0 1\]\</code\> F-contrast to test for
this interaction effect. Similarly, using the same contrasts in a
one-way ANOVA design at the second level (with 4 \"levels\"; first two
for patients, second two for controls), split into the 18 patients and
17 controls, will let you test for group effects (using a second-level
F-contrast \<code\>\[1 0 -1 0; 0 1 0 -1\]\</code\> to test for
differences). This will test for *(8) the group x valence x task
interaction*. 

That\'s it in terms of the factorial nature of your design: for a
factorial design with 3 factors there are 8 effects to test for: an
overall effect, 3 main effects, 3 two-way interactions and one 3-way
interaction - and you can test for them using the approaches numbered
(1) to (8) above. 

As an aside, SPM can help you with the contrast you need to test for
effects in factorial designs. Use the function  \<code\>Con =
spm_make_contrasts (\[k1 k2\])\</code\> for a -by- design (eg. 2 by 3) .
If the required contrast has multiple rows you\'ll need to take up
multiple con images to the second level.

For any of the second level designs you can also enter covariates, such
as age. If you have a one-sample t-test design at the second level then
this is straightforward, as long as your age variable is centred - i.e.
zero mean (otherwise it will be collinear with the other effect you are
testing, and make it disappear). A \<code\>\[0 1\]\</code\> F-contrast
will then test for regions with BOLD responses dependent on age. For
more complex second level designs e.g. two-sample t-tests, you can
include an interaction term - this will create two new columns e.g. age
of patients, age of controls. A \<code\>\[0 0 1 0; 0 0 0 1\]\</code\>
F-contrast then tests for any effect of age, a \<code\>\[0 0 1
-1\]\</code\> F-contrast then tests for regions where the BOLD vs age
effect is different for patients versus controls.

## Further reading

- [ANOVA and
  SPM](https://www.fil.ion.ucl.ac.uk/~wpenny/publications/rik_anova.pdf), R.
  Henson and W. Penny
- [Analysis of Variance
  (ANOVA)](http://www.mrc-cbu.cam.ac.uk/wp-content/uploads/2015/03/Henson_EN_15_ANOVA.pdf), R.
  Henson
- [Modeling Group-Level Repeated Measurements of Neuroimaging Data Using
  the Univariate General Linear
  Model](https://doi.org/10.3389/fnins.2019.00352), M. McFarquhar
