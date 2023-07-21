---
layout: post
title:  "ML Digit Recognizer Website"
date:   2023-07-19 00:33:53 -0600
categories: jekyll update
---
Use this website to draw digits and to have different ML models predict what they are. See the Github Repo <a href="https://github.com/cullena20/DigitRecognizerWebsite" target="_blank"> here</a>.

![Digit Website Screenshot](/assets/images/projects/DigitWebsite.png)

<!-- excerpt-end -->

# Overview

This website uses Machine Learning to recognize handwritten digits that you draw. There are 3 ML models to choose from:
a simple Neural Network written in Tensorflow, a Convolutional Neural Network written in Tensorflow, and a simple Neural Network
that I wrote from scratch using numpy.  Alongside the prediction, a processed image is also displayed. This is the image that is fed into the ML
model for prediction. 

# Training Process
I used the MNIST training set to train these models. The MNIST dataset consists of 70,000 handwritten digits and was created in 1994.
I built trained 3 models on this dataset, using 60,000 training images and 10,000 testing images. The CNN achieves 99.2% test accuracy,
the Tensorflow Neural Network achieves 97.7% test accuracy, and my neural network achieves 93.7% test accuracy. I built my neural
network from scratch using numpy with the assistance of Michael Nielson's great book, <a href="http://neuralnetworksanddeeplearning.com/">
Neural Networks and Deep Learning</a>. Click different models for more architectural details.

It should be noted that all the models do not perform as well on digits drawn in this website as they do on testing data. This is a result
of the preprocessing step. To address this, models may be further trained on data gathered through this kind of preprocessing. My neural 
network appears to suffer the most from this, while the others are able to generalize better.

# Preprocessing

Getting these models to work on new handwritten digits not from the dataset requires some extra steps. Each image is 28x28 pixels, greyscale, 
inversed, and centered. To get a meaningful prediction from the model, images must first be processed.

The digit that you draw is first converted from RGBA (this is an image format that captures color using three color channels - RGB stands for Red, Green, Blue - and transparency using a fourth channel - A stands for alpha) to L (this is a greyscale image format that only captures information
on brightness - L stands for luminance). This converts an image that is originally stored as 4 channels by 28 pixels by 28 pixels to a 28 pixel by 28 pixel image. 

The image is then scaled down to 28x28 pixels using Bilinear Interpolation, which is a technique that allows rescaling while preserving information. These steps 
are necessary for the models to process the image at all because the models can only take inputs of specific shapes. 

The image is then inverted so that white becomes black and black becomes white. This step is necessary for the model to make meaningful predictions at all, 
because it has been trained on inversed images.

Finally, the image is centered. Because the training data consists entirely of centered images, the models perform poorly on non centered images. For example, 
if you were to draw a digit on the side of the box or to draw them very small, the model would perform poorly. To address this, any blank space is first stripped 
from an image. This solves the problem of digits that are drawn very smally. This stripped image is then scaled to be 20 pixels by 20 pixels. Blank space is then 
added to restore the image to 28 by 28 pixels. This results in a centered image that the model can perform better on. 

# Building The Website

I built this webiste using Python, Python's Flask Library, JavaScript, JavaScript's Fetch API, HTML, and CSS with Boostrap. 

The drawing board is implemented using HTML Canvas and Javascript. When the predict button is clicked, I use the Fetch API in JavaScript to send the image and chosen 
model data to Flask where it can be processed (to be precise, the chosen model data is sent to Flask when the website is opened and when the model is changed, it is 
then used here). Here, my Python code performs Image Recognition on the digit and returns a prediction, alongside the processed image. This is done using Python with 
Numpy and Pillow. The models were previously trained in a Jupyter Notebook using Tensorflow and Numpy. The prediction and processed image are returned to the JavaScript 
Fetch API, where they are displayed on the website.
