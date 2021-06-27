---
layout: post
title: "Good Students Play Big Lottery Better"
date: 2021-06-27
---

#### Paper Summary
##### Written By: Ajay Jaiswal
------------------
[Paper URL](https://arxiv.org/pdf/2101.03255.pdf)

**Lottery Ticket Hypothesis (LTH)** points to a brand-new opportunity to identify an extremely sparse and self-trainable sub-network from a dense model. Such sub-network can be found via the following three steps: 

(1) training a dense network from scratch; 

(2) pruning unnecessary weights to form a sparse sub-network; and 

(3) re-training the sub-network from the same random initial weights used in the dense model (winning ticket). 

Such sparse sub-network that can match the test accuracy of the original dense networks when trained in isolation from (the same) random initialization. However, the hypothesis fails to immediately generalize to larger dense networks such as ResNet-50. Later research efforts found that, while it was generally hard to achieve a comparable performance of the sub-network from totally (even the same) random initialization, some “auxiliary information" could be obtained from the **early training phase** of the dense network (first step) to aid the re-training (third step).

**Weight rewinding** proposed (Frankle et. al. 2019) “roll back" the dense model weights to some early training iteration (but not the very beginning), and to use the rewound weight as the re-training initialization. That was found to help find lottery tickets in large networks and/or at extremely high pruning ratios. A simplified version which is called **learning rate rewinding**, i.e., rolling back the learning rate schedule rather than the weight status, for the retraining to start
with. Those efforts suggest the promise of “recycling" useful knowledge from the original training for finding large-scale lottery tickets.

