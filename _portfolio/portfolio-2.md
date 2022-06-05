---
title: "Multi-Task Learning for Image segmentation using attention and other aux-tasks"
excerpt: "<br/><img src='/images/MTAN.jpeg' width="100" height="100>"
collection: portfolio
---


## Code and Report

For more details please view the detailed report [here](https://github.com/ArnabPushilal/MLT/blob/main/report%20(2).pdf).

The code can be found [here](https://github.com/ArnabPushilal/MLT)

## Abstract

In this study, the effectiveness of different MTL architectures was explored, by first only evaluating a baseline inspired on the Segnet architecture; then modifying the baseline to implement MTL framework with two additional tasks (image classfication and bounding-boxes prediction); and finally by fine-tuning the MTL network using pre-trained weights from VGG-16 trained on ImageNet . Furthermore, inspired by the success of attention approaches in computer vision, a more recent and novel MTL network that includes task specific attention modules was investigated. This network is called Multi-Task Attention Network. 

Finally, auxiliary tasks of image denoising, edge detection and image colourisation were implemented. For all of them, self-supervision strategies were used to create the ground-truth labels. The outlook of this research was to investigate the effect of parameter sharing on the effectiveness of learning, the effect of self-supervision on the model's performance and the effectiveness of fine-tuning in the context of simultaneous MTL. We also explored methods of combination of losses to improve our model.


## SegNet


<img src='/images/Segnet.jpeg'>


In this empirical study, a Segnet model was implemented as the baseline for image segmentation. This model is an encoder-decoder network for image segmentation, topologically inherited from a VGG-16 architecture. Initially, Segnet was used only for the single target task of segmentation. Then, an MTL architecture was implemented by adding two layers at the end of the Segnet encoder, in order to perform binary classification and bounding box regression.

## MTAN

Multi-Task Learning with Attention based on Segnet was implemented in order to enhance the relative importance given to each task in training. The parameter sharing scheme involved a shared network and task specific attention modules. The original implementation was modified by applying the attention module only to the encoder. The soft attention masks were applied to every one of the five blocks of the encoder, that learns the relative importance of the shared parameters for each task. As there were shared layers along with task specific attention modules, both shared and task specific features were learnt, in a self-supervised manner.

## Losses & Metrics

Cross-entropy was used for the loss of both segmentation & classification and mean-squared loss was used for the bounding-box. The metrics used for segmentation were F1, and Jaccard Index (IOU) and accuracy was used for classification. The IOU weighted by the classes was also monitored. Existing methods for loss tuning like SoftAdapt , DWA , GM  losses were investigated but did not improving the target task. Therefore, manually fixed weights were initialised for each of the tasks as follows:
    
$L_{total}  = \alpha_{1} L_{seg} + \alpha_{2} L_{cls} + \alpha_{3} L_{bbox}$ 

where $\alpha_{1} ,\alpha_{2} ,\alpha_{3}$ were chosen as hyper-parameters instead of being determined based on the rate of change of the losses. This was done with respect the scale of the losses and with keeping in mind the improvement of the target task.

## Auxillary tasks

Three auxiliary tasks were implemented on the above-mentioned MTAN architecture. The three auxiliary tasks tackled were the colourisation of grayscale images, the detection of edges in colour images (Canny filter) and the removal of normally distributed noise from an image.

<img src='/images/image.png'>

Figure: Original Image

<img src='/images/noise.png'>

Figure: Noisy Image with known noise

<img src='/images/canny.png'>

Figure: Canny edge detector

The auxillary tasks were added to the architecture similar to bounding box & classification from before.

## Denoising

The restoration from a noisy image to a clean image has been of great interest since it was introduced \cite{zhou1988image} from the perspective of neural networks. 
For this experiment, denoising was achieved by using as training data noisy images, constructed by adding small Gaussian noise with std 2 to clean, original, images, which were used as targets. %The noise added was normally distributed with a mean of 5 and standard deviation of 2 .The training loss used was the Mean Squared Error Loss (MSE). 

## Edge Filter Detection


A new task branch was added after the encoder, that predicts the boundaries in the image. 
In this experiment, a canny edge detector was implemented, as the proxy to find the boundaries of objects in the input image. Canny filter available in OpenCV was used to find the boundaries in images. A new task branch was added after the encoder in MTAN, that predicts the boundaries in the images.

 
## Colourisation

In order to implement colourisation, the baseline Segnet network was modified firstly by changing the number of input channels of the first layer from 3 to 1 color channel. RGB images were converted to LAB. A single L channel was taken as an input and used to predict the A and B channels which were outputted in the final layer of the network. Pre-trained weights were added by skipping the first layer, where there was a mismatch between VGG & our network for this task.

## Results and Analysis

For full results and experiments please view the report at [link](https://github.com/ArnabPushilal/MLT/blob/main/report%20(2).pdf)