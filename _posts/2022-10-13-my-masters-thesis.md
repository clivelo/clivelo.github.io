---
title: My master's thesis
tags: [Psychology, Academic]
style: fill
color: danger
description: At last, I have graduated with my research master's degree. This was definitely one of the most challenging things I have ever done. Since I haven't posted much here, I thought "hey, let's write about my thesis again on my website after I have just gruelingly finished writing my thesis."
---

At last, I have graduated with my research master's degree. This was definitely one of the most challenging things I have ever done. Since I haven't posted much here, I thought "hey, let's write about my thesis again on my website after I have just gruelingly finished writing my thesis."

Anyway, my thesis title was **Neural Manifestation of Individual Variation in Touch Pleasantness**.

## Introduction
I would like you to first think about this. How would you like to be touched by a significant other or a friend? Say if they are affectionately stroking your forearm, how fast of a stroke would you consider pleasant?

In the field of touch, it is widely believed that our sense of touch is divided into two distinct systems—the **discriminative system** and the **affective system**.

The discriminative system deals with the haptic properties of an object in contact with our skin. For example, sandpaper feels rougher and grittier than just a normal piece of A4 paper, or a feather feels lighter than a 10 lb dumbbell in terms of the pressure being pushed against our skin. It is said that the fast-conducting **Aβ afferents** (afferents are sensory neurons) are responsible for transducing this information to our brain.

On the other hand, the affective system deals with the affective quality of touch. It is said that this information is instead transduced through slow-conducting **C-tactile (CT) afferents**.

I'm assuming by now you have thought about my question at the beginning of this introduction. Actually, research tells us that, in terms of stroking speed, people deemed a **1-10 cm/s** speed to be pleasant, any slower or any faster was considered less pleasant. In fact, when measuring the Aβ and CT afferents, Aβ fired more as stroking velocity increased whereas CT fired more at that same 1-10 cm/s velocity. As such, CT is said to associate with touch pleasantness, and this high firing rate/pleasantness at intermediate velocities was named the **inverted U-shaped effect**.

<img style="padding-right:2rem" src="/assets/post/2022-10-13-my-masters-thesis/002.png" title="Aβ" alt="Aβ" width="200" height="200"><img style="padding-right:2rem" src="/assets/post/2022-10-13-my-masters-thesis/003.png" title="CT" alt="CT" width="200" height="200">

Some researchers have proposed that this inverted U-shaped effect can be used as a diagnostic marker of certain developmental issues. However, there is a huge problem with this generalized claim. Does this effect exist on both the group and individual levels, or does it only exist on the group level? In other words, how much variability is there between people's preference to touch velocities? Also, how does that manifest in our brain processes?

## Method
To answer this research question, we used a robotic touch device that controls a soft cosmetic brush. The device stroked people at five distinct velocities (0.5 cm/s, 1 cm/s, 3 cm/s, 10 cm/s, and 20 cm/s) and they indicated the pleasantness of each stroke. Simultaneously, we measured their brain activity in each trial using the Electroencephalogram (EEG).

<img src="/assets/post/2022-10-13-my-masters-thesis/001.jpg" title="Robotic Touch Device" alt="Robotic Touch Device" width="400" height="400">

## Results
#### Subjective Pleasantness Ratings
First, we wanted to look at how much variability there was between individuals. Do all healthy people prefer 1-10 cm/s speed of touch? Well actually, no. K-means clustering showed that there were **two distinct types of people**. One group prefers the good ol' 1-10 cm/s, but the other group prefers faster speed (given the range of our tested velocities)! We speculated that maybe the 1-10 cm/s group prefers CT activation whereas the fast group prefers Aβ activation.

Interestingly, with the fast velocity preference group, intermediate velocities became less and less pleasant as the experiment went on. This implies that this group affectively habituated to intermediate velocities specifically and therefore likely reduces their exposure to such touch in real life.

<img src="/assets/post/2022-10-13-my-masters-thesis/004.png" title="Cluster" alt="Cluster" width="640" height="400">

#### Neural Process
We divided the EEG data into six areas and three frequencies (beta, alpha, theta). The data were subjected to a support vector machine (SVM) to explore what features can be used to predict a person’s stroking speed preference. We found that the **right-middle brain area theta frequency band** was a good predictor of speed preference (10-fold cross-validation accuracy of 0.67 > 0.5 chance)

## Postface
Hope you enjoyed this short summary of my master’s thesis, feel free to contact me by any method shown on my website if you want to know more. It’s time to get back to my job hunt.
