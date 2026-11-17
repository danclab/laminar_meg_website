---
title: "laMEG: getting started with laminar MEG"
description: "A practical introduction to laMEG: cortical surfaces, simulations, source reconstruction, and inference across cortical depth."
slug: lameg
date: 2026-11-17T00:00:00+01:00
draft: false
image: cover.png
author: James J. Bonaiuto
categories:
    - software
    - how_to
tags:
    - laMEG
    - source reconstruction
    - tutorials
    - DANC
links:
    - id: lameg
      title: laMEG on GitHub
      description: Source code, installation instructions, and issue tracker.
      website: https://github.com/danclab/laMEG
    - id: lameg-docs
      title: laMEG documentation
      description: Installation guidance and API reference.
      website: https://danclab.github.io/laMEG/index.html
    - id: lameg-tutorials
      title: laMEG tutorials
      description: Worked examples of surface processing, simulations, and laminar inference.
      website: https://github.com/danclab/laMEG/tree/main/tutorials
    - id: lameg-wiki
      title: laMEG acquisition and preparation guides
      description: MRI sequences, head-casts, co-registration, and other practical considerations.
      website: https://github.com/danclab/laMEG/wiki
    - id: lameg-releases
      title: laMEG releases
      description: Versioned releases and notes on changes.
      website: https://github.com/danclab/laMEG/releases
---

In the previous post, I wrote about [how laminar MEG became a thing]({{< relref "post/02_hello/index.md" >}}). This time, I want to get to a more practical question: *how do you actually try it?*

This is what [laMEG](link:lameg) is for. It is an open-source Python toolbox built around FreeSurfer and SPM, bringing together cortical surface preparation, simulations, source reconstruction, and laminar inference. The aim is to make these methods easier to use, inspect, and extend.

There are already [tutorials](link:lameg-tutorials) and [documentation](link:lameg-docs), so this post is a guide to what is there and where I would start.

## Start with the anatomy

Before asking where within the cortex a signal comes from, we need a model of that cortex.

laMEG starts from FreeSurfer's pial and white-matter surfaces and prepares a set of surfaces spanning the cortical ribbon. It handles the less glamorous but important steps: removing non-cortical vertices, combining hemispheres, downsampling the meshes, preserving correspondence between depths, and estimating dipole orientations. The `LayerSurfaceSet` interface keeps these surfaces and their processing stages together.

The correspondence between vertices across layers matters. A vertex on one surface needs to refer to the same cortical location on the other surfaces. Otherwise, comparing depths also means comparing different places. Dipole orientation matters too: changing the assumed direction of a current changes its predicted magnetic field. We explored anatomically informed estimates of cortical column orientation in [Bonaiuto et al. (2020)](paper:bonaiutoCorticalColumnOrientation2020).

The [surface-processing tutorial](https://github.com/danclab/laMEG/blob/main/tutorials/tutorial_01_surface_processing.ipynb) is a good place to start, even before running an inversion. Look at the surfaces, check the correspondence, and make sure the anatomy makes sense.

One thing is worth keeping in mind: these layer surfaces sample **cortical depth**. They are not individually segmented histological laminae. Eleven layer surfaces do not mean eleven biological laminae (of some possible extraterrestrial's brain), or even eleven independently resolvable signals. Our multilayer work distinguishes these depth samples from cytoarchitectonic laminae and uses anatomical information to relate the two ([Szul et al., 2026](paper:szulMultilayerMEG2026)).

## What question are you asking?

laMEG is not a single inversion algorithm. It supports several approaches to laminar inference, which answer related but different questions.

**Model comparison** asks which candidate source model best explains the measurements. For example, we can reconstruct the same data using a pial-surface model and a white-matter-surface model, then compare their free energy or cross-validation error. The tutorials introduce this with simulated sources, so we know which answer we should get. These approaches were evaluated in [Bonaiuto et al. (2018)](paper:bonaiutoNoninvasiveLaminarInference2018).

**Source-space power analysis** instead reconstructs activity using a combined source space with multiple layer surfaces, and compares task-related power changes at corresponding depths within a region of interest. This is useful for questions about the depth bias of oscillatory activity. It is not the same measurement as a difference in model evidence; the [ROI power tutorial](https://github.com/danclab/laMEG/blob/main/tutorials/tutorial_05_roi_power_analysis.ipynb) walks through that approach.

**Sliding-window model comparison** asks how the preferred depth changes over the course of a response within a cortical location. The [sliding-window tutorial](https://github.com/danclab/laMEG/blob/main/tutorials/tutorial_04_sliding_win_model_comparison.ipynb) uses an initial source reconstruction to select spatial priors, then compares depth models in successive time windows. This is the approach we used to investigate changing deep and superficial contributions during beta bursts ([Bonaiuto et al., 2021](paper:bonaiutoLaminarBetaBursts2021)).

The multilayer framework extends the comparison beyond two surfaces. Its result is evidence for the dominant depth contribution within a cortical location and time window rather than a simultaneous recording from every cortical layer ([Szul et al., 2026](paper:szulMultilayerMEG2026)). That matters when deciding what an analysis can tell you.

## Simulate something you already know the answer to

My recommendation is to start with a simulation, not your most interesting empirical result.

The [free-energy tutorial](https://github.com/danclab/laMEG/blob/main/tutorials/tutorial_02_model_comparison_free_energy.ipynb) simulates an oscillation at a known cortical location and depth, projects it to the sensors, adds noise, and asks whether model comparison recovers the correct depth. The [cross-validation tutorial](https://github.com/danclab/laMEG/blob/main/tutorials/tutorial_03_model_comparison_cv_error.ipynb) provides another way to compare the candidate models.

Once that works, change one thing at a time. Move the source. Reduce the SNR. Change the simulated patch size or the patch size assumed during reconstruction. The most useful question to ask is not whether the method can recover a source under favourable conditions, but how it behaves when those conditions deteriorate.

There is also a [tutorial on anatomical predictors of inference accuracy](https://github.com/danclab/laMEG/blob/main/tutorials/tutorial_07_anatomy_inference_accuracy.ipynb). It examines cortical thickness, orientation, distance to the scalp, and differences between lead fields across depth. This is particularly useful when deciding where to test a laminar hypothesis because not every cortical location provides equally informative measurements.

## Moving to your own recordings

The worked examples start from preprocessed MEG data in SPM format, anatomical surfaces, and the information needed to co-register the MEG recording with the MRI. For acquisition and preparation, the [wiki](link:lameg-wiki) has guidance on MRI sequences, head-casts, and co-registration.

Our simulations show that depth inference depends on SNR, co-registration accuracy, source orientation, and how distinguishable the predicted fields are at the location being analysed ([Szul et al., 2026](paper:szulMultilayerMEG2026)). I would use those dependencies to plan checks for a particular dataset - a successful simulation doesn't guarantee a successful experiment.

For installation, follow the repository instructions. The documented setup uses a dedicated conda environment, FreeSurfer, and an SPM standalone installation configured through `lameg-postinstall`. The README also links to example data for the tutorials.

At the time of writing, the latest [release](link:lameg-releases) is **v0.2.1**. Check which software version your examples require, particularly when working from the development branch, and record the version used for an analysis.

## A toolbox, not the community's only pipeline

At our [first LaminarMEG meeting](/meetings/), Solène presented work in progress on EBBlayer, including the problem of reconstructing correlated activity at different depths. That deserves its own post.

More generally, laMEG is one route to laminar MEG, not a requirement for participating in this community. Comparisons with other software and modelling approaches are part of the point.

For reproducible software problems, please open an [issue on GitHub](https://github.com/danclab/laMEG/issues), including the version, error message, and a minimal example where possible. For questions about interpretation, validation, or a result that looks strange, bring them to a meeting or the [mailing list](https://listes.services.cnrs.fr/wws/subscribe/laminarmeg).

{{< bibliography >}}
