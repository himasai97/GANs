# Wasserstein GAN with Gradient Penalty (WGAN-GP)

The Wasserstein Generative Adversarial Network, or WGAN, is an extension to the generative adversarial network that improves the stability of the model. It also uses a specific loss function, known as the W-loss, and gradient penalties that prevent mode collapse.

Instead of a discriminator that classifies an image as fake/real, a critic model is used in this approach, where it scores the images with real numbers.

While the generator and critic classes are built in the same way as in the previous project, some additional parameters are calculated to compute the model losses.

## Gradient penalty
- The first step in calculating gradient penalty is to create a batch of mixed images by weighing the fake and real images using epsilon and then adding them together. 
- The gradient of the critic's score on the mixed images (output) with respect to the pixels of the mixed images (output) is then calculated. 
- Finally, the gradient penalty is calculated as the mean squared distance between the magnitude of the gradient, called the norm, and the ideal norm of 1.

## Losses
For the generator, the loss is calculated to maximize the critic's prediction on the generator's fake images. For this purpose, it is defined to be the negative of the mean of the critic's scores.

For the critic, the loss is calculated to maximize the distance between the critic's prediction on the real images and the predictions on the fake images while also adding a gradient penalty that is weighed by a given lambda value.

## Snapshot of Outputs
![Results1](https://raw.githubusercontent.com/himasai97/GANs/main/WGAN-GP/Snap_loss_WGAN1.PNG)


![Results2](https://raw.githubusercontent.com/himasai97/GANs/main/WGAN-GP/Snap_loss_WGAN2.PNG)

## Observations

- Even on GPU, the training runs more slowly than the previous GAN models. This is because the gradient penalty requires the model to compute the gradient of a gradient, thus potentially adding additional run time per epoch.
- In this approach, the critic is updated multiple times every time the generator is update. This is done to prevent the generator from overpowering the critic.
- While WGAN-GP doesn't neccessarily improve the overall performance of the GAN, it increases the stability of the model and avoids mode collapse.


