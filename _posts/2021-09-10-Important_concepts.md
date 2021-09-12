---
layout: post
title: "Important Notes"
date: 2021-9-10
---

#### Paper Summary
##### Written By: Ajay Jaiswal
------------------

Stride in Colvolution: The convolution calculation does not need to be perfomed for every point of input, for example, we can skip every nth input point in either direction. This concept is called stride - in this example we are computing the output with stride of n + 1. A convolution without any striding applied is said to have a stride of 1. Stride is an important concept, as it essentially allows us to downsample input resolution through convulution quickly.
