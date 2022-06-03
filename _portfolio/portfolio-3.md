---
title: "Project-1"
excerpt: "What's my age again?<br/><img src='/images/flowshabri.png'>"
collection: portfolio
---

### Goal

My goal was to create a web app where you could upload an image with your face in it. And the app
should return a predicted age. 

### Stack Used

Python, SQLAchemy, Flask, HTML, Docker, Pytorch

### Dataset 

To train the initial model the UTKFACE dataset was used[link](https://susanqq.github.io/UTKFace/). This consists of 20K+ images with assosiated ages. I used the ones where the faces are already detected and cropped. This was a consicious decision, since I planned to perform some sort of face detection on the image before the age detection.

### Age Prediction Model

The model was based on the encoder of a VGG network. Each convolutional layer consists of a conv2d --> batchnorm-->relu. The feature dimensions are downsampled using maxpooling and finally two fully connected networks generated the predicted age. An MSE loss was used and the model was trained with a learning rate of 1e-4 and a batch size of 12. The model was then saved to be used at test time 
depending on the user input of the image.

### 


