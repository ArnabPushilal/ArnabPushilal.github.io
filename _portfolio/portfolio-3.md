---
title: "Project-3"
excerpt: "What's my age again?<br/><img src='/images/vgg.png'>"
collection: portfolio
---

# Goal

My goal was to create a web app where you could upload an image with your face in it. And the app
should return a predicted age. 

## Stack Used

Python, SQLAchemy, Flask, HTML, Docker, Pytorch

## Dataset 

To train the initial model the UTKFACE dataset was used [link](https://susanqq.github.io/UTKFace/). This consists of 20K+ images with assosiated ages. I used the ones where the faces are already detected and cropped. This was a consicious decision, since I planned to perform some sort of face detection on the image before the age detection.  

Example Image from the training set.
![32 Year old]('images\32.jpg')



# Prediction Model

## Face detector

Prior to testing the image on the model, a facedetector from mediapipe is used to return bounding
box co-ordinates for the face detected. The faces are cropped out and the cropped face is used for testing.



## Age Predictions
The model was based on the encoder of a VGG network. Each convolutional layer consists of a conv2d --batchnorm--relu. The feature dimensions are downsampled using maxpooling and finally two fully connected networks generated the predicted age. An MSE loss was used and the model was trained with a learning rate of 1e-4 and a batch size of 12. The model was then saved to be used at test time 
depending on the user input of the image.





<img src="/images/og.jpg" alt="Lights"  width="50" height="50">

<p>Original Image</p>
           
<img src="/images/face.png" alt="Nature"  width="50" height="50">
           
<p>Detected facial features</p>

<img src="/images/crop.jpg" alt="Fjords"  width="50" height="50">
            
<p>Cropped Image</p>
        
            
        
# Front end

The application had a very simple front-end interface. One base HTML file with a navbar where "home"
redirects to the home page & "example images" Shows some example images from a test set where the model has already had decent results.

## Form page

<img src='/images/formpage.png' title="Form page">


The form given to the user consists of four entries. Validations were done for each of fields

as follows.
1. Real Age: Enforced between 0 and 120, cannot be left blank.
2. First Name: Must be more than 1 character
3. Image: Must have 3 channels and cannot be empty
4. Data Save Box: If yes, data will be stored and collected

Additionally, if no faces were detected on the face detector model, it won't move past this 
with another error message.











