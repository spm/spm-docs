# Semantic matching fMRI task

## What is second-level/group-level analysis?

Second-level fMRI analysis (also known as group-level analysis) is a critical step in neuroimaging research that aggregates data from multiple participants to draw broader conclusions about brain function. Unlike first-level analysis, which focuses on each participant's brain activity in isolation, group-level analysis examines patterns of brain activity that are consistent across a group of participants. By pooling data from multiple individuals, researchers can identify common neural responses associated with specific tasks, conditions, or populations. 

In this section, we will go through different second-level models:

- **One-sample t-test**: what is the average brain response across all participants?
- **Two-sample t-test**: are there differences between the two groups studies? 
- **Factorial**: are there any interactions driving the findings? 

## About the data

To demonstrate these analyses, we will use a semantic matching task from [Seghier et al. (2010)](https://doi.org/10.1523/JNEUROSCI.3377-10.2010) where both left and right handed participants were instructed to respond either with their left or right hand. Consequently, the dataset consists of the following groups: (1) right handed responding with their right hand, (2) right handed responding with their left hand, (3) left handed responding with their right hand, and (4) left handed responding with their left hand. In the examples described here we will only focus on the effects of response hand and handedness, but if you are interested in other analyses that can be performed with this data, please refer to [the accompanying publication](https://doi.org/10.1523/JNEUROSCI.3377-10.2010). 

The data archive for this tutorial can be (downloaded from here)[]. The data has already been preprocessed and first-level models have been specified. The data structure of the archive is as follows:

```
derivatives
    first_level         <- first-level models
    preprocessed        <- preprocessed data
    second_level        <- space for your second-level models
```

Within the first-level directory, you will find estimated first-level models for each participant. Below you can see the design matrix that has been specified for each of the participants. 

![](../../../assets/figures/tutorials/fmri/group/semantic_first_level_design_matrix.png)

This design matrix includes two runs of the task with the following regressors defined:

    1.      Semantic task condition (pictures) - Run 1
    2.      Semantic task condition (words) - Run 1
    3.      Control task condition (non-words) - Run 1
    4.      Control task condition (pictures) - Run 1
    5.      Instructions - Run 1
    6-11.   Motion regressors - Run 1

    12.     Semantic task condition (pictures) - Run 2
    13.     Semantic task condition (words) - Run 2
    14.     Control task condition (non-words) - Run 2
    15.     Control task condition (pictures) - Run 2
    16.     Instructions - Run 2
    17-22.  Motion regressors - Run 2

    23.     Error term - Run 1
    24.     Error term - Run 2

Based on these regressors the following task contrasts have been specified:

1. Effects of interest (F-test)

    ```
    1 0 0 0
    0 1 0 0
    0 0 1 0
    0 0 0 1
    ```

2. Semantic - control pictures

    ```
    1 0 0 -1
    ```

3. Semantic - control words

    ```
    0 1 -1 0
    ```

4. Interaction

    ```
    1 -1 1 -1
    ```

5. Semantic pictures 

    ```
    1 0 0 0
    ```

6. Semantic words

    ```
    0 1 0 0
    ```

7. Control words

    ```
    0 0 1 0
    ```

8. Control pictures

    ```
    0 0 0 1
    ```

9. Task 

    ```
    1 1 1 1
    ```

In the following sections of this tutorial, we will explore different second-level models that we can apply to this dataset. 

!!! info "Reference"
    [Seghier, M. L., Fagan, E., & Price, C. J. (2010). Functional subdivisions in the left angular gyrus where the semantic system meets and diverges from the default network. Journal of Neuroscience, 30(50), 16809-16817.](https://doi.org/10.1523/JNEUROSCI.3377-10.2010)

--8<-- "addons/abbreviations.md"