---
title: "Multi-Task Learning for Image segmentation using attention and other aux-tasks"
excerpt: "<br/><img src='/images/MTAN.jpeg'>"
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