# Conditional GAN

A Conditional GAP is used to generate hand-written images of digits, conditioned on the digit to be generated (the class vector). As a result, it lets the user choose what digit they want to generate.

The main difference in this approach is that the generator no longer takes noise_dim as an argument, but in_dim instead, as it takes both the noise and class vectors. (Here, in_dim = noise_dim + n_classes)

## Class Input
To include the class information in the input vector for the generator, a [one-hot encoded vector](https://pytorch.org/docs/stable/nn.functional.html#torch.nn.functional.one_hot) is used, where its length is the number of classes and each index represents the class. In the next step, the one-hot class vector is concatenated with the noise vector and fed as the input to the generator. 

Similarly, the class information should also also be included when feeding the image as input to the discriminator. To combine them both, the one-hot encoded vector is transformed to be of same width and height as the input image through repeatition along the new dimension. Thus, resultiing input tensor has (image channels+ no. of classes, image width, image height) dimension.

By thus including the class information in the inputs to both the generator and the discriminator, a conditional GAN model is built for generating images of the chosen class.

## Training the model

A snapshot of the progression in image generation is shown below:
![Snapshot1](https://raw.githubusercontent.com/himasai97/GANs/main/ConditionalGAN/Result_CondGAN_1.PNG)
![Snapshot2](https://raw.githubusercontent.com/himasai97/GANs/main/ConditionalGAN/Result_CondGAN_2.PNG)

## Exploration

After putting the generator in eval mode, the conditional GAN model is used to create images, where interpolation has been introduced to better observe the changes in the images generated. Here, the model produces intermediate images that eventually morph from the starting image to the final image set.

### Interpolating the class vector

In this case, the interpolation vector is created for the class labels and is given as input to the model, along with the noise vector.

A set of pairwise class interpolations for a collection of different numbers is visualized below:

![Class_interpol](https://raw.githubusercontent.com/himasai97/GANs/main/ConditionalGAN/CondGAN_eval_class_interpol.PNG)

### Interpolating the noise vector

Here, the class vector is held constant while interpolating the noise vector. The images generated at each step for the target class=5 is shown below:

![Noise_interpol](https://raw.githubusercontent.com/himasai97/GANs/main/ConditionalGAN/CondGAN_eval_noise_interpol.PNG)

