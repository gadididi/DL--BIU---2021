# DL--BIU---2021

## Introdiction
Image-to-image translation is a class of vision and graphics problems where the goal is to learn the mapping between an input image and an output image using a training set of aligned image pairs. However, for many tasks, paired training data will not be available. We present an approach for learning to translate an image from a source domain X to a target domain Y in the absence of paired examples. Our goal is to learn a mapping G: X → Y, such that the distribution of images from G(X) is indistinguishable from the distribution Y using an adversarial loss. Because this mapping is highly under-constrained, we couple it with an inverse mapping F: Y → X and introduce a cycle consistency loss to push F(G(X)) ≈ X (and vice versa). Quantitative comparisons against several prior methods demonstrate the superiority of our approach.

Our goal is to build a CGAN that generates 7,038 Monet-style images with only 30 monet's paints. Since we only had to work with thirty images, we had to use special ways to optimize the familiar model.

## Solution
### General approach
As we mentioned in the introduction, since we only need to use 30 images, we had to use more efficient ways to achieve this goal.
Initially, we were looking for existing models for monet's drawings and wanted to learn the best from each one, so we came up with ideas for our solution.

The GAN architecture is an approach to training a model that is comprised of two models: a generator model and a discriminator model. The generator takes a point from a latent space as input and generates new plausible images from the domain, and the discriminator takes an image as input and predicts whether it is real (from a data set) or fake (generated). Both models are trained in a game, such that the generator is updated to better fool the discriminator and the discriminator is updated to better detect generated images.

The CycleGAN is an extension of the GAN architecture that involves the simultaneous training of two generator models and two discriminator models.

One generator takes images from the first domain as input and outputs images for the second domain, and the other generator takes images from the second domain as input and generates images for the first domain. Discriminator models are then used to determine how plausible the generated images are and update the generator models accordingly.
