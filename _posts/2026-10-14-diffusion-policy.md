---
layout: post
title: "Diffusion Policy"
date: 2026-10-14 13:14:26 -0400
tags: robotics ML
categories: research
giscus_comments: true
---

<!--toc:start-->

- [Diffusion Models](#diffusion-models)
<!--toc:end-->

This is my first technical post and I think it will be a good open to later
discussions about how to implement decision-making and control algorithms [^1].
I will be heavily relying on the paper released by
[Cheng et al 2024](https://arxiv.org/abs/2303.04137v5) and a post by Lillian
Weng on
[Diffusion Models](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/#prog-distll).

[^1]: I've found there's not a clear distinction between decision-making and control algorithms, especially in the robotics space. I like the distinction found in Game AI where decision-making refers to high level search and planning algorithms, often operating at the semantic level, while control refers to low-level search and planning algorithms. <!-- rumdl-disable-line line-length -->

I will start off by explaining what a diffusion policy is. A diffusion policy is
a visuomotor policy for robotic manipulation based on the popular Diffusion
Model which is used for image generation (think Stable Diffusion, etc.) [^2].
Diffusion policies were popularized by Cheng et al who uses them for Imitation
Learning (IL) in comparison to popular alternatives like Iterative Behavior
Cloning (IBC) which use the ideas behind implicit/energy-based models.

This post will be focused on diffusion policies for manipulation because I'm
currently working on research to use diffusion policies for garment folding.
Okay, that's not the only reason, I also want to use this as an attempt to start
writing regularly again after I mentioned it in my last post.

My plan in this post is to outline an implementation strategy for testing and
getting a feel for a diffusion policy. I want this post to somewhat
comprehensive so it will require that I first start with Diffusion Models.

## Diffusion Models

Diffusion policies serve as a method for implementing visuomotor policies for
robotic manipulation [^2].

[^2]: A visuomotor policy is a way to formulate a state space where we can have images as states and then have movement/joint-commands/etc. as actions to transition between states (i.e. when I move I should see a new image and that new image is caused exclusively by my previous action.) This means that we should be able to model static environment fairly well since the only way to see something is by performing an action like moving the robot forward. <!-- rumdl-disable-line line-length -->
