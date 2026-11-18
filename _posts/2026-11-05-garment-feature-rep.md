---
layout: post
title: "Garment Folding Feature Representation"
date: 2026-10-14 13:14:26 -0400
tags: WIP garment-folding
categories: research
giscus_comments: true
---

This post is a bit of a rush job so I hope to come back and polish it up, but
its more important that I just get a post out if I want to keep this habit up.

I spent some time today writing notes about the different ways that researchers
have used to represent the feature space for garment folding tasks. I am still
working on going through the different types of methods but the main ones that I
have noticed are image based (RBG-D), mesh-based, keypoints, semantic keypoints,
implicit representation, learned latent space, and a new deformation aware
reprentation using a temporal constrastive pretraining approach. The last one is
latest and the authors compare it with a similar point-based approach used by
the authors of UniGarmentManip.

The main thing that I learned from taking these notes is the feature
representation can have a major impact on the training stability, performance,
and generalizability of a model. In order to create fast, high performance
models, it's important to get the feature representation right and this design
choice must work with the model architecture to create a good performance. One
of the other considerations that I noticed is that some features are easier to
obtain in simulation than in real-life so its important to select features that
can be easily obtained.

<!-- TODO: Need to fix the sentence below for clarity -->

If the proposed data pipeline and model require that
real-world data be used then its important to account for the fact that some
features may have error that can cause the model training to experience drift
because the features are temporally related and so they may make it harder to
model the features and deploy for autonomous operation in the real-world.

Instant-Fold is one of the latest papers that uses a masked RBG-D observation
which is passed into a frozen DINO model to obtain an image embedding.
Simulataneously, the depth image is converted into a point cloud and sampled.
The samples of 3D points are projected on the image embedding using a bilinear
interpolation. Once the 3D points are projected onto the image embedding, both
the 3D position data and the image embedding from the DINO model are passed into
a MLP to produce a 3D token representation. The process is how the pretraining
is completed.

Another approach is to directly use the RBG-D images and pass them into Spatial
Action Maps. This approach is used by Speedfolding and APS-Net (Zhou et al
2025). Speedfolding uses the Spatial Action Maps to classify an action primitive
and predict parameterizations of the action primitives (ie we select whether to
pick & place, fling, or drag and then specify two planar gripper locations that
the robot arms should be in to perform the task). APS-Net is similar but they
also collect keypoints on the garment to determine if the garment is
sufficiently smooth. These approaches work fairly well with success rates around
the 80% range but it seems the lack the kind of generalization and robustness
that a model should have in order to be deployed in the real-world.

Diffusion Policies are being chosen over Spatial Action Maps due to the fact
that they can better represent multi-modal action distributions compared to
explicit policy representations like Spatial Action Maps. Diffusion Policies
also have better training stability and robustness and they have nice properties
like the assumption that we are sampling from an action distribution created
from the training data.
