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

The progression in the images generated is shown below

![Result_DCGAN](https://raw.githubusercontent.com/himasai97/GANs/main/DCGAN/Result_DCGAN.PNG)

# Observation

While there is a better balance between the discriminator and the generator in a DCGAN when compared with the standard GAN model, it is to be observed that generator is disproportionately producing certain digits (like 1, 8, 0 and 3) when compared to others. Because the discriminator didn't learn to detect this imbalance quickly enough, the generator continued producing similar outputs. As a result, it ended up tricking the discriminator so well that there was no improvement to be found in the model, also known as a mode collapse.

To mitigate unstable learning and mode collapse, I have implemented an andvanced technique called WGANs in the next project.
