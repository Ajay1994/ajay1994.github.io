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


### Hypercolunm Concept 
Paper : https://arxiv.org/pdf/1411.5752v2.pdf

Many algorithms using features from CNNs (Convolutional Neural Networks) usually use the last FC (fully-connected) layer features in order to extract information about certain input. However, the information in the last FC layer may be too coarse spatially to allow precise localization (due to sequences of maxpooling, etc.), on the other side, the first layers may be spatially precise but will lack semantic information. To get the best of both worlds, the authors of the hypercolumn paper define the hypercolumn of a pixel as the vector of activations of all CNN units “above” that pixel.

![image](https://user-images.githubusercontent.com/6660499/132967096-024775f4-3760-4d78-b860-9b87a05acce9.png)

The first step on the extraction of the hypercolumns is to feed the image into the CNN (Convolutional Neural Network) and extract the feature map activations for each location of the image. The tricky part is when the feature maps are smaller than the input image, for instance after a pooling operation, the authors of the paper then do a bilinear upsampling of the feature map in order to keep the feature maps on the same size of the input. There are also the issue with the FC (fully-connected) layers, because you can’t isolate units semantically tied only to one pixel of the image, so the FC activations are seen as 1×1 feature maps, which means that all locations shares the same information regarding the FC part of the hypercolumn. All these activations are then concatenated to create the hypercolumn. For instance, if we take the VGG-16 architecture to use only the first 2 convolutional layers after the max pooling operations, we will have a hypercolumn with the size of:


**64 filters (first conv layer before pooling)  +  128 filters (second conv layer before pooling ) = 192 features**

This means that each pixel of the image will have a 192-dimension hypercolumn vector. This hypercolumn is really interesting because it will contain information about the first layers (where we have a lot of spatial information but little semantic) and also information about the final layers (with little spatial information and lots of semantics). Thus this hypercolumn will certainly help in a lot of pixel classification tasks such as the one mentioned earlier of automatic colorization, because each location hypercolumn carries the information about what this pixel semantically and spatially represents. This is also very helpful on segmentation tasks (you can see more about that on the original paper introducing the hypercolumn concept).

#### Extracting arbitrary feature maps

Now, to extract the feature map activations, we’ll have to being able to extract feature maps from arbitrary convolutional layers of the network. We can do that by compiling a Theano function using the get_output() method of Keras, like in the example below:

![image](https://user-images.githubusercontent.com/6660499/132967171-77f856d5-1104-48b1-81fe-b697b6a59f95.png) 

![image](https://user-images.githubusercontent.com/6660499/132967229-aa7eb1f0-c30a-44c4-be1d-12f6e70b798c.png)

**Feature Map**

In the example above, I’m compiling a Theano function to get the 3 layer (a convolutional layer) feature map and then showing only the 3rd feature map. Here we can see the intensity of the activations. If we get feature maps of the activations from the final layers, we can see that the extracted features are more abstract, like eyes, etc. Look at this example below from the 15th convolutional layer:

![image](https://user-images.githubusercontent.com/6660499/132967238-be6ee1c8-4ff6-445c-8b1f-109398c0954e.png)

As you can see, this second feature map is extracting more abstract features. And you can also note that the image seems to be more stretched when compared with the feature we saw earlier, that is because the the first feature maps has 224×224 size and this one has 56×56 due to the downscaling operations of the layers before the convolutional layer, and that is why we lose a lot of spatial information.

**Extracting hypercolumns**


Now finally let’s extract the hypercolumns of arbitrary set of layers. To do that, we will define a function to extract these hypercolumns.  Let’s now test the hypercolumn extraction for the first 2 convolutional layers:

![image](https://user-images.githubusercontent.com/6660499/132967363-85750ad2-2395-4050-a5c6-11b8ca959dd2.png)

That’s it, we extracted the hypercolumn vectors for each pixel. The shape of this “hc” variable is: (192L, 224L, 224L), which means that we have a 192-dimensional hypercolumn for each one of the 224×224 pixel (a total of 50176 pixels with 192 hypercolumn feature each).

layers_extract = [3, 8]
hc = extract_hypercolumn(model, layers_extract, im)

Let’s plot the average of the hypercolumns activations for each pixel:

![image](https://user-images.githubusercontent.com/6660499/132967892-8fcd6172-7a0d-4aa9-b3f4-0e27329270f9.png)

As we can see now, the features are really more abstract and semantically interesting but with spatial information a little fuzzy.

Remember that you can extract the hypercolumns using all the initial layers and also the final layers, including the FC layers. 
