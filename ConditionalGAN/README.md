# Conditional GAN

A Conditional GAP is used to generate hand-written images of digits, conditioned on the digit to be generated (the class vector). As a result, it lets the user choose what digit they want to generate.

The main difference in this approach is that the generator no longer takes noise_dim as an argument, but in_dim instead, as it takes both the noise and class vectors. (Here, in_dim = noise_dim + n_classes)

## Class Input
To include the class information in the input vector for the generator, a [one-hot encoded vector](https://pytorch.org/docs/stable/nn.functional.html#torch.nn.functional.one_hot) is used, where its length is the number of classes and each index represents the class. In the next step, the one-hot class vector is concatenated with the noise vector and fed as the input to the generator. 

Similarly, the class information should also also be included when feeding the image as input to the discriminator. To combine them both, the one-hot encoded vector is transformed to be of same width and height as the input image through repeatition along the new dimension. Thus, resultiing input tensor has (image channels+ no. of classes, image width, image height) dimension.

By thus including the class information in the inputs to both the generator and the discriminator, a conditional GAN model is built that generates images of only the given set of numbers.

