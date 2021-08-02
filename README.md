# DL--BIU---2021

## Introdiction
Image-to-image translation is a class of vision and graphics problems where the goal is to learn the mapping between an input image and an output image using a training set of aligned image pairs. However, for many tasks, paired training data will not be available. We present an approach for learning to translate an image from a source domain X to a target domain Y in the absence of paired examples. Our goal is to learn a mapping G: X → Y, such that the distribution of images from G(X) is indistinguishable from the distribution Y using an adversarial loss. Because this mapping is highly under-constrained, we couple it with an inverse mapping F: Y → X and introduce a cycle consistency loss to push F(G(X)) ≈ X (and vice versa). Quantitative comparisons against several prior methods demonstrate the superiority of our approach.

Our goal is to build a CGAN that generates 7,038 Monet-style images with only 30 monet's paints. Since we only had to work with thirty images, we had to use special ways to optimize the familiar model.

## Getting Started
open the [colab](https://colab.research.google.com/drive/1vsGq3VUZJB3OOf2m5qeeyG45Nor22J7_#scrollTo=WoSotYff9nqG) link

## Solution
### General approach
As we mentioned in the introduction, since we only need to use 30 images, we had to use more efficient ways to achieve this goal.
Initially, we were looking for existing models for monet's drawings and wanted to learn the best from each one, so we came up with ideas for our solution.

The GAN architecture is an approach to training a model that is comprised of two models: a generator model and a discriminator model. The generator takes a point from a latent space as input and generates new plausible images from the domain, and the discriminator takes an image as input and predicts whether it is real (from a data set) or fake (generated). Both models are trained in a game, such that the generator is updated to better fool the discriminator and the discriminator is updated to better detect generated images.

The CycleGAN is an extension of the GAN architecture that involves the simultaneous training of two generator models and two discriminator models.

One generator takes images from the first domain as input and outputs images for the second domain, and the other generator takes images from the second domain as input and generates images for the first domain. Discriminator models are then used to determine how plausible the generated images are and update the generator models accordingly.

### Design
we used PyTorch framework.
As we mentioned above our model consists of 2 generators. The first produces paintings by Mont (Domain A), and the second produces photographs (Domain B).

#### Generator
We created 2 generators, one for each domain. But they both have the same architecture.
The generator starts from a noise image performing encoding for it and then decoding. We added Residual blocks.
We came to see that a generator without Residual blocks does not bring good results.
We set up 9 Residual blocks for the generator.

![image](https://user-images.githubusercontent.com/59120630/127920577-683f8502-8f95-4aae-89c7-b7accdf884da.png)

#### Discriminator
We created 2 Discriminators. Both with the same architecture.
The first for domain A (fake photo and fake ID) and the second for domain B (fake photo and photo ID).
The networks are based on the CNN model.
And then switch to the fully connected network.

#### The Training Process
First, we trained the model about 10 epochs. We have seen that there is indeed an improvement in output from epoch to epoch. We tried to train about 30 epochs and saw that the improvement continued to grow.
We used batches the size of 5 items per batch.
Second, the learning rate is 0.2. We also used "ADAM" for optimizing (according to recommendations we saw on Internet)
We trained the model as follows:

1. Training for the generators. Then calculate the loss and update the weights of the generators.
2. Training the discriminators, calculating their losses and then updating their weights.

#### Loss Functions
Our loss consists of several factors:
1. **Generator Loss**
   - **loss identity** - Measures the mean absolute error (MAE) between each element in the input x and target y. 
   This is the loss_Identity_Photo: 
   x = the output from Generator_photo_to_monet(real_Monet) function.
   y = real_Monet photo same as loss_Identity_Monet to sum: identity loss = loss_identity = (loss_Identity_Monet + loss_Identity_Photo)/2
   - **GAN Loss** - We will mention this below.
   - **Cycle Loss** - We focus on Monet Generator, but is same for the second. We get photo and generate fake Monet paint. Now we use Photo Generator with the fake paint we made. We get recovery photo. We check: 
   ![image](https://user-images.githubusercontent.com/59120630/127923950-8468cef4-43d7-47d5-9fbd-e8df48a1e051.png)
   We compare pixel with Measures  the  mean  absolute  error  (MAE) method.
   Finally, the total loss (for both Generators) is: loss_G = loss_GAN + (10.0\*loss_cycle) + (5.0\*loss_identity)
2. **Discriminator Loss**
   - **GAN Loss** - Creates a criterion that measures the mean squared error (squared L2 norm) between each element in the input x and target y.
    In fact we send pairs of image and label (fake/ real) to Monet Discriminator and to Photo Discriminator. We send 2 batch per Discriminator. First batch is real (Photo or paint, depended by the model) and we want to get label=1. The second batch is fake and we hope to get label=0 for all the images in this batch. we make average loss for those two batch (per model). 

#### Data augmentation
Our data set contain only 30 Monet paints. so we use Data augmentation technique for create more data by what we have. For example, random crop, resize or add some noise to the picture.
