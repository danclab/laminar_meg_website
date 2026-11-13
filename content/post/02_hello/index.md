---
title: How did laminar MEG become a thing?
description: A short, slightly personal history of head-casts, cortical depth, and why we started this community.
slug: 00_hello
date: 2026-11-13 00:00:00+0100
image: cover.png
author: James J. Bonaiuto
categories:
    - history
tags:
    - laminar MEG
    - head-casts
    - DANC
weight: 1
---

Welcome to the LaminarMEG blog! Solène and I have been putting this site together as a place to share papers, code, tutorials and, hopefully, some of the less polished parts of doing laminar MEG. I thought a good way to start would be with a little history of how we got here.

## It started with head-casts

When I tell people I work on laminar MEG, I still sometimes get some version of: *Wait, MEG can do that?*  This is a fair question. The cortex is only a few millimetres thick, and MEG is not exactly famous for resolving cortical layers. The important point is that we aren't directly imaging histological layers. We're testing whether magnetic field measurements are better explained by sources at different depths within the cortex, which is a much more constrained problem.

Back in 2014, Luzia Troebinger, José David López, Antoine Lutti, David Bradbury, Sven Bestmann, and Gareth Barnes were working on something that made this possible: high-precision MEG. A major source of error in conventional MEG is uncertainty about where the brain is relative to the sensors. Annoyingly, people move in the scanner, and co-registering the MEG recording with the MRI is not perfect. Their solution was to use individually moulded, 3D-printed head-casts to keep the head in a reproducible position. The first ones weren't particularly pretty or comfortable, but they brought movement and co-registration errors down into roughly the millimetre range ([Troebinger et al., 2014](paper:troebingerHighPrecisionAnatomy2014)).

This led to the obvious question: if we know the head position this precisely, can we distinguish sources closer to the pial surface from those closer to the white matter? In a second paper that year, they showed that under favourable conditions, the magnetic fields predicted by these two source models were distinguishable, including in auditory-evoked data ([Troebinger et al., 2014](paper:troebingerDiscriminationCorticalLaminae2014)). That was really the starting point for laminar MEG.

## It wasn't all straightforward

I joined the work at UCL while these methods were being developed. By 2016 I was trying to use them to look at alpha, beta, and gamma activity during visual and motor tasks. At about the same time, Sofie Meyer was working on more flexible head-casts, which made repeated recordings a lot more practical ([Meyer et al., 2017](paper:meyerFlexibleHeadcasts2017)).

Looking through some of my emails from that period is pretty entertaining. In July 2016 I was circulating an abstract on the laminar specificity of oscillations. A couple of months later I was excitedly telling Sven that I had some promising visual results. In between were a lot of less exciting messages about cortical meshes, co-registration, and source reconstructions that took forever. At one point I had a 1-mm inversion running for more than 52 hours without finishing its first run. Not ideal. We also had some pretty strange results. In one December 2016 exchange I was trying to understand why some inversion algorithms seemed to put almost everything on the pial surface, while another analysis was producing enormous subcortical t-values. These turned out to be different problems. The point is that getting a nice deep-versus-superficial figure was never the same thing as knowing it was right.

![The UCL high-precision MEG group wearing head-casts in June 2015. From left to right: Simon Little, Saskia Helbling, Holly Rossiter, Sheena Waters, Sofie Meyer, Gareth Barnes, Sven Bestmann, and Jimmy Bonaiuto (clearly Simon and I are wearing someone else's heast-casts!)](gareth_lab_headcasts.jpg)

This work eventually led to two papers published in 2018. In the methodological study, we tested how much the laminar result depended on the inversion algorithm, SNR, patch size, and other modelling choices ([Bonaiuto et al., 2018](paper:bonaiutoNoninvasiveLaminarInference2018)). In the empirical study, we found that visual alpha and sensorimotor beta were better explained by deeper source models, while gamma favoured more superficial ones ([Bonaiuto et al., 2018](paper:bonaiutoLaminaSpecificCorticalDynamics2018)). I was pretty pleased with those results, but they didn't tell us that alpha comes from layer V and gamma from layers II/III. We compared two cortical surfaces. That's useful, but it's not the same as identifying individual cytoarchitectonic layers.

## From oscillations to cortical dynamics

Since then, a lot of the work has been about figuring out where the method breaks. Source orientation is one example: the usual surface-normal assumption isn't always a good approximation of the orientation of cortical columns, and we found that more anatomically informed estimates could improve MEG source reconstruction ([Bonaiuto et al., 2020](paper:bonaiutoCorticalColumnOrientation2020)).

The beta burst work also changed what I wanted to do with laminar MEG. Rather than just asking whether beta is deep or superficial, we could ask how the inferred deep and superficial contributions change over the time course of a single burst. Combining MEG with biophysical modelling in collaboration with Stephanie Jones gave us a way to test that idea ([Bonaiuto et al., 2021](paper:bonaiutoLaminarBetaBursts2021)). I think using *depth and time together* is ultimately much more interesting than assigning a frequency band to a layer.

And the hardware is changing too. OPMs can get much closer to the scalp, which should help us measure some of the fine spatial structure in the magnetic field. Saskia Helbling's simulations showed promising results for laminar inference with OPMs, while also making it clear that SNR, co-registration, and model biases don't just disappear ([Helbling, 2025](paper:helblingLaminarOPMSimulation2025)).

## Where are we now?

Our recent multilayer work extends the old deep-versus-superficial comparison to a series of surfaces across the cortical ribbon ([Szul et al., 2026](paper:szulMultilayerMEG2026)). We can now ask more detailed questions about depth-resolved activity, but the answer still depends on anatomy, SNR, co-registration, source orientation, and how separable the lead fields actually are. There are places where the data contain useful depth information, and others where they contain much less. We should be careful not to confuse the number of surfaces in a model with the number of cortical layers we can actually resolve.

Some of those original UCL datasets are still ones I look at regularly, almost ten years later. I, and now Solène, probably know a few people's cortical folds better than they do by now.

As for this website, there are now people working on laminar SQUID-MEG, OPM-MEG, source reconstruction, simulations, biophysical modelling, and invasive validation in several different groups. We thought it would be useful to have somewhere to talk about work in progress, not just finished papers. In particular, I'd like people to feel comfortable sharing weird results, failed simulations, depth biases, code, and things they aren't sure about yet. These are often the most useful discussions.

We still need much stronger invasive validation and better ways of quantifying uncertainty. But I think we've moved beyond *can MEG tell us anything about cortical depth?*, to a more interesting question: *can we use depth and millisecond timing to test circuit-level models in humans?*

That's what I'm hoping this community will help us figure out.

{{< bibliography >}}

*Cover drawing by Solène Gailhard.*
