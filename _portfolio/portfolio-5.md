---
title: "Mixture of Gaussians"
excerpt: "<br/><img src='/images/'>"
collection: portfolio
---


## Goal

Create a classifier class that can take in images with masks as examples, create binary classes,
in this case (apple vs Non-Apple) then train a Gaussian Mixture model with Expectation-Maximization.

## Procedure

This was one of the training examples

<img src='/images/apples.jpg'> 

Figure: Apples

<img src='/images/mask.png>

Figure: Mask

We can see it's been split into two distinct categories - Apples & non-apple pixels. To proceed we
would need to fit a mixture of gaussian of model to both categories individually. To that end we use the EM algorithm to make progress. 
 
## Evaluation / Metrics

To evaluate, given any image we need to calculate the posterior on the image to see the probability of apple/background. We use a prior that is just the previous proportion of apples/non-apples





