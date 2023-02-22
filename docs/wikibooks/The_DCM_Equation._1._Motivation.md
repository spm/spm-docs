<figure>
<img src="8-tournament-transitive.svg"
title="8-tournament-transitive.svg" />
<figcaption>8-tournament-transitive.svg</figcaption>
</figure>

## Networks in the Brain

Statistical Parametric Mapping (SPM) enables researchers to label which
brain regions are significantly activated by an experimental task,
relative to some baseline. The coloured blobs produced by SPM tell us
which brain regions are involved in a particular task, but they say
nothing about the *network* of brain regions used by the subject. In the
simplest terms, we want to draw arrows between the coloured blobs,
showing how information flows through the brain.

Dynamic Causal Modelling (DCM) enables us to ask questions about brain
connectivity. Is brain region A responsible for the change in activation
of brain region B? Where does sensory stimulus enter a network of brain
regions? Which connections are modulated by factors such as attention?

## A DCM Experiment

The basic plan for a DCM experiment is as follows:

1.  Select the set of brain regions involved in a particular task. This
    will likely be guided by the results of a standard SPM analysis.
2.  Decide on a set of hypotheses for how these regions may be
    connected. Each hypothesis may have a different set of connections
    between regions, different places where external input enters the
    network, or different connections modulated by the experimental
    tasks or internal factors such as attention.
3.  Embody each hypothesis in a Dynamic Causal Model (DCM).
4.  Test each DCM for how well it describes the observed fMRI data (when
    combined with a model of blood-oxygen flow).
5.  Compare the models to select the best, or compare the strengths of
    individual connections between models.

Before we get on to the practical guide of how to implement a DCM
experiment, we will start with the theory. At the heart of DCM is a
*state equation*, and the first part of the tutorial describes what it
is and how it works.
