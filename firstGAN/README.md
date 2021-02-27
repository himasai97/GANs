# First GAN

This Generative Adversarial Network is built and trained on the hand-written images of digits (0-9) using PyTorch framework.

The three main steps involved in this project are:

(1) building the generator and discriminator components of the GAN

(2) creating loss functions for both the generator and discriminator

(3) training the GAN model and visualizing the generated images

## MNIST Dataset

The training images for the model are obtained from the [MNIST](http://yann.lecun.com/exdb/mnist/). It contains 60,000 images of handwritten digits, from 0 to 9, of 28x28 size. A sample of these images taken from Wikipedia is shown below:

![MNIST data](https://raw.githubusercontent.com/himasai97/GANs/main/firstGAN/MNIST.png)

## Noise generation
When creating the noise vectors, it must be ensured that the images generated from the same class don't all look the same. For this purpose, PyTorch is used to randomly sample numbers from the normal distribution.

## Building Generator and Discriminator
Here, modular programming approach is used to build the generator and discriminator components. The functions created for this purpose are:

**get_gen_block**: returns a generator neural network layer, consisting of a [linear transformation](https://pytorch.org/docs/stable/generated/torch.nn.Linear.html) (also called Dense layer) followed by a [batch normalization](https://pytorch.org/docs/stable/generated/torch.nn.BatchNorm1d.html) for stabilization, and then a [ReLU](https://pytorch.org/docs/master/generated/torch.nn.ReLU.html) (a non-linear activation function).

**get_disc_block**: returns a discriminator neural network layer, consisting of a [linear transformation](https://pytorch.org/docs/stable/generated/torch.nn.Linear.html) (also called Dense layer) followed by a [leaky ReLU](https://pytorch.org/docs/stable/generated/torch.nn.LeakyReLU.html) (to prevent the _dying ReLU_ problem often observed in the discriminator).

### Components

**Generator** class: 

Initialized with features: noise vector dimension, output image dimension, initial hidden layer output dimension.

Generator model: using the above values, a neural network is built with 4 layers created using the block function and the final layer consisting only of a linear transformation followed by a [sigmoid function](https://pytorch.org/docs/master/generated/torch.nn.Sigmoid.html).

In the forward pass method of this class, the generator model takes a noise vector as input and applies non-linear transformations until the tensor is mapped to the size of the image to be outputted, which is then scaled with the sigmoid function, to produce a fake image.

**Discriminator** class:

Initialized with features: the input image dimension and the initial hidden layer output dimension

Discriminator model: using the above values, a neural network is built with 3 layers created using the block function and the final layer consisting simply of a linear transformation. A sigmoid function is not needed after the output layer as it is included in the loss function of the discriminator.

In the forward pass method of this class, the discriminator model takes an image tensor as the input and transforms it until the model returns a single number (1-D tensor) as the output. The binary result thus classifies whether the given image is fake or real.

## Loss functions

[BCEWithLogitsLoss](https://pytorch.org/docs/stable/generated/torch.nn.BCEWithLogitsLoss.html) is the loss function used in this project

**Get Discriminator's loss**:

Here, the noise vectors are first created and then fed to the generator to get a batch of fake images.

The discriminator is then called with these fake images as input and the loss is calculated from the discriminator's prediction of the fake images. Here, the ground truth is simply a tensor of all zeros.

Then the discriminator is called with a batch of real images and the loss is calculated with the ground truth being a tensor of all ones.

Finally the discriminator's loss is calculated by taking the average of the real and fake losses

**Get Generator's loss**

Similar to the discriminator's case, the noise vectors are created in the first step and fed to the generator to get a batch of fake images.

Then the discriminator's prediction of these fake images is obtained to calculate the generator's loss. In this case, the ground truth used in the loss function is a tensor of all ones. This is because the goal of the generator is to make the discriminator think that its fake images are real

## Training the model
Finally, everything is put together to train the model! For each epoch, the entire training set is processed in batches. For every batch, the discriminator and generator components are updated based on the losses calculated. 

Here, batches refer to a set of images that are processed per training step instead of training one image at a time. as a result, the generator will create an entire batch of images, receive the discriminator's feedback on each before updating the model. Similarly, the discriminator calculates its loss on the entire batch of generated images as well as on a batch of real images before the model is updated. This results in a more efficient and stable training of the GAN model.

In this project, I used [Adam optimization algorithm from PyTorch](https://pytorch.org/docs/stable/optim.html) to optimize both the generator and discriminator models.

Thus, my first GAN model is built with sufficient accuracy in generating fake images of hand-written digits. The progression in the images generated is shown below:

![Results](https://raw.githubusercontent.com/himasai97/GANs/main/firstGAN/Result.PNG)

**Observations**

- The discriminator often outperforms the generator as its job is easier. 
- Sometimes, the generator's loss is greater than 1. This is because for a sufficiently confident wrong guess, the binary cross entropy loss can be any positive number.
- While it's possible to get a near-perfect accuracy for one of the two models, it is important to ensure that neither one gets too good, as it would cause the entire model to stop learning. Balancing the two models is a difficult task in a standard GAN. As a result, in the next project I will work on DCGAN and explore its advantages over the standard model.




