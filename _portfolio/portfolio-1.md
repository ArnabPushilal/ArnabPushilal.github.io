---
title: "What's my age again?"
excerpt: "Web-app for age prediction<br/><img src='/images/formpage.png' >"
collection: portfolio
---

# Visit the website

whatsmyageagain.herokuapp.com 

# Goal

My goal was to create a web app where you could upload an image with your face in it. And the app
should return a predicted age. 

# Code

All the code and required information to run can be found at this [link](https://github.com/ArnabPushilal/whatsyourage)

## Stack Used

Python, SQLAchemy, Flask, HTML, Docker, Pytorch

## Dataset 

To train the initial model the UTKFACE dataset was used [link](https://susanqq.github.io/UTKFace/). This consists of 20K+ images with assosiated ages. I used the ones where the faces are already detected and cropped. This was a consicious decision, since I planned to perform some sort of face detection on the image before the age detection.  

# Prediction Model

## Face detector

Prior to testing the image on the model, a facedetector from mediapipe is used to return bounding
box co-ordinates for the face detected. The faces are cropped out and the cropped face is used for testing.

<img src="/images/og.jpg" alt="Lights"  width="300" height="300">

<p>Figure: Original Image</p>
           
<img src="/images/face.png" alt="Nature"  width="300" height="300">
           
<p>Figure: Detected facial features</p>

<img src="/images/crop.jpg" alt="Fjords"  width="300" height="300">
            
<p>Figure: Cropped Image</p>
        
            


## Age Predictions

The model was based on the encoder of a VGG network. Each convolutional layer consists of a conv2d --batchnorm--relu. The feature dimensions are downsampled using maxpooling and finally two fully connected networks generated the predicted age. An MSE loss was used and the model was trained with a learning rate of 1e-4 and a batch size of 12. The model was then saved to be used at test time 
depending on the user input of the image.


        
# Front end

The application had a very simple front-end interface. There is a base HTML file with a navbar where "home" redirects to the home page & "example images" shows some example images from a test set where the model has already had decent results.
The rest of the files: form.html, predictions.html & example.html are started off with the base template
## Form page

<img src='/images/formpage.png' title="Form page">
<p>Figure: Form Page</p>

The form given to the user consists of four entries. Validations were done for each of fields

as follows.
1. Real Age: Enforced between 0 and 120, cannot be left blank.
2. First Name: Must be more than 1 character
3. Image: Must have 3 channels and cannot be empty
4. Data Save Box: If yes, data will be stored and collected

Additionally, if no faces were detected on the face detector model, it won't move past this 
with another error message.

## Predict Page

<img src='/images/predict.png' title="predict">
<p>Figure: Predict Page</p>

The predict page consists of the image that the user uploaded along with the predicted age &
the true age that they inputed with the error.


# Storage

If the user checks the data saved box, then the data is stored in a table 
called "Images" where the following fields are saved:

1. first_name
2. true_age
3. predicted_age
4. img_filename 
5. img_data  

The idea was to store data continously and update the model as time goes by. (However this wasn't possible because Heroku did not support SQLite, maybe in the future I could update this)


# Deployment

The app was deployed using the heroku container registry with the dockerfile in the code.












