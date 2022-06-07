---
title: "Binary Classifier using Mixture of Gaussians"
excerpt: "<br/><img src='/images/post-1.png'>"
collection: portfolio
---


## Goal

Create a classifier class that can take in images with masks as examples, create binary classes,
in this case (apple vs Non-Apple) then train a Gaussian Mixture model with Expectation-Maximization.

## Notebook

The notebook containing the code and detailed analysis can be found [here](https://github.com/ArnabPushilal/GaussianMixture)

## Procedure

This was one of the training examples

<img src='/images/apple.jpg'> 

Figure: Apples

<img src='/images/mask.png'>

Figure: Mask

We can see it's been split into two distinct categories - Apples & non-apple pixels. To proceed we
would need to fit a mixture of gaussian of model to both categories individually. To that end we use the EM algorithm to make progress. 
 
## Evaluation / Metrics

To evaluate, given any image we need to calculate the posterior on the image to see the probability of apple/background. We use a prior that is just the previous proportion of apples/non-apples in the training images. Then we use the likelihood of the pixels under the two trainied mix of gaussians to calculate the posterior.

Example:

<img src='/images/post.png'>

Figure: Training Image

<img src='/images/post-1.png'>

Figure: Test Image

The metrics used for testing the model quantitively were F1 score and the ROC curve.

For instance, the F1 score for this particular test image was 0.92 and the ROC curve was as follows:

<img src='/images/roc.png'>







