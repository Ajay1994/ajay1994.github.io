---
layout: post
title: "Important Notes"
date: 2021-9-10
---

#### Paper Summary
##### Written By: Ajay Jaiswal
------------------

**Stride in Colvolution:** The convolution calculation does not need to be perfomed for every point of input, for example, we can skip every nth input point in either direction. This concept is called stride. A convolution without any striding applied is said to have a stride of 1. Stride is an important concept, as it essentially allows us to downsample input resolution through convulution quickly.

**Upsampling:** Upsampling on the other hand can be implemented by transposed convolution, where
the input matrix is spread out by zero padding, producing output resolution higher than
the input resolution (can be thought of as having stride lower than 1).

**Dilated convolution:** More complex schemes of convolution have be devised, such as the recently introduced
concept of dilated convolutions (also known as ”atrous” convolution). Instead of using
contiguous neighboring inputs to calculate the output value of a convolution operation,
we insert gaps of size d ∈ N0 between the inputs. This hyperparameter is called the
dilation of convolution. Normal convolutions have dilation of 0 (some sources define
regular convolution dilation to be 1). In Figure 2.2 we can see a visual explanation of the
operation. Dilation allows convolution layers to have larger effective receptive field while
retaining a lower amount of parameters. Using this technique has been shown to help
alleviate overfitting as well as improve performance.

![image](https://user-images.githubusercontent.com/6660499/132966596-deb6297d-e927-4461-be7e-8aae899c6217.png)

#### Effective receptive field
A single convolutional layer with a kernel of size k can only encode relations between its
inputs in k × k patches. The inputs are connected in a local region, in case of a kernel size of 3 on an image, we can only hope to represent connections between pixels not further apart than 2 pixels. As this would be rather limiting, convolutional layers are put in a
sequence.

When we stack convultional layers on top of one another, the effective reach of connections increases, in the example of 3×3 kernel, two stacked layers would yield a function of a 5 × 5 patch of the original input as the final output, since the second set of convolutions operates on inputs that are the outputs of the previous convolution. This concept is called the **effective receptive field** of the network and can be viewed as the size of
the field of initial inputs that can influence a final output. Some sources also refer to this
concept as field-of-view.


It is important to realize that not every input, even if it falls into the receptive field
of an output, will be able to influence the output in the same way. Intuitively, the input units (pixels or voxels) spatially closer to the output units
can influence it more significantly, as they are used in more convolution calculations when
computing the output’s value. When the effective receptive field is mentioned in regards
to a network, it is understood as the size of the field of an arbitrary output in relation to
the network’s inputs.

Effective receptive field is a critical concept when considering CNN architectures, as it
provides a way of reasoning about individual outputs.

It follows naturally that the receptive field of a CNN can be increased by employing
different methods. Every method tends to have advantages and disadvantages, some
more severe than others. The **simplest means of increasing the receptive field is to stack
more layers**. This comes with the huge downside of significantly impacting training times
and increasing memory requirements (though this is framework dependent), as well as,
generally, increasing the number of epochs needed for training to converge. However, on
the flip side, this method increases the number of spatially independent parameters in the
network as a side-effect, which can be beneficial to increasing accuracy if overfitting is
accounted for.

Another technique is using **subsampling, either through increasing the stride of some
convolutional layers or by pooling**. Through reducing the size of one layer’s output, we are effectively ”packing” or ”compressing” those pixels into a smaller spatial area, which means that successive layer’s receptive field will increase to a larger patch of the original
input. Subsampling has the advantage of spatially decreasing the size of inputs, which
translates into improved training times, and can help with overfitting, as it is more likely to
force the network to generalize, even though it does not necessarily reduce the parameter
space. On the other hand, subsampling can make certain features harder to extract due
to lowered resolution - typically subtle details in input images and smaller objects become
lost in the process.

A relatively recent and popular method of **enlarging the receptive fiend is using dilated
convolutions**. Most commonly the dilation is set to a small number, as the beneficial effect
seems to diminish quickly with increased dilation. The biggest advantage to using dilated
convolutions is the ability to expand the receptive field without losing resolution, unlike
increasing stride or pooling. Dilated convolutional layers allow us to quickly grow the
effective receptive field, much more aggressively than non-dilated convolutions.

Note that the receptive field of a single layer is equal to the size of its filters, as that
is the extent of the inputs that contribute to individual output’s value.
