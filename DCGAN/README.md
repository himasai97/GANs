The highly successful DCGAN model was developed in 2015 and the source paper explaining its implementation can be found [here](https://arxiv.org/pdf/1511.06434v1.pdf)

# Generator

It has 4 layers (3 hidden layers + 1 output layer)

While most of the steps in building the generator block are similar to the steps implemented in the previous project, the main differences are noted below:

- Instead of passing in the image dimension, the number of image channels is passed as input to the generator.

  This is because the convolutions, used in DCGAN, don't depend on the number of pixels on an image. However, the size of filters is determined from the number of image channels.

- The linear transformation layer is replaced with a [transposed convolution layer](https://pytorch.org/docs/master/generated/torch.nn.ConvTranspose2d.html).

- In the final layer, a [Tanh activation](https://pytorch.org/docs/stable/generated/torch.nn.Tanh.html) is used.

# Discriminator

It has 3 layers (2 hidden + 1 output)

Each block in the Discriminator has a [convolution layer](https://pytorch.org/docs/master/generated/torch.nn.Conv2d.html) inplace of a linear transformation layer.
