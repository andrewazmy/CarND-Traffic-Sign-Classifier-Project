## Traffic Sign Recognition 


---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./mark_up_images/histogram.png "Histogram"
[image2]: ./mark_up_images/before_norm.png "Standarizing"
[image3]: ./new_images/signs/2-1.jpg "Traffic Sign 1"
[image4]: ./new_images/signs/1-2.png "Traffic Sign 2"
[image5]: ./new_images/signs/25-1.png "Traffic Sign 3"
[image6]: ./new_images/signs/40-1.jpg "Traffic Sign 4"
[image7]: ./new_images/signs/14-1.jpg "Traffic Sign 5"
[image8]: ./mark_up_images/softmax.png "Softmax probabilities"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  
---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/andrewazmy/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set and identify where in your code the summary was done. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.
In the notebook, step 1, in the first 3 blocks of code a summary of the data set shape and number of samples is provided. In addition to plotting random samples of images from the training samples, and a histogram representing the sample count for each individual class.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset and identify where the code is in your code file.

The code for this step is contained in the fourth code cell of the IPython notebook.  

Here is an exploratory visualization of the data set. It is a bar chart showing how the data ...

![alt text][image1]

### Design and Test a Model Architecture

#### 1. Describe how, and identify where in your code, you preprocessed the image data. What tecniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc.

The code for this step is contained in the fifth code cell of the IPython notebook.

As a first step, I decided to stick to RGB images as input to the network, since i believe converting to grayscale causes a significant loss of important features such as the colour of a sign, which can differentiate between two classes with two different meanings.

In an attemp to achieve consistency in the training set of images, I tried normalizing/standardizing the images.

Here is an example of a traffic sign image before and after standardization .

![alt text][image2]


#### 2. Describe how, and identify where in your code, you set up training, validation and testing data. How much data was in each set? Explain what techniques were used to split the data into these sets. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, identify where in your code, and provide example images of the additional data)

The code for splitting the data into training and validation sets is contained in the eighth code cell of the IPython notebook.  

To cross validate my model, I randomly split the training data into a training set and validation set. I did this by the train_test_split function from sklearn.model_selection.

My final training set had 27839 number of images. My validation set and test set had 6960 and 12630 number of images.

#### 3. Describe, and identify where in your code, what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

The code for my final model is located in the seventh cell of the ipython notebook. 

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| (0) Input         		| 32x32x3 RGB image   							| 
 | (1) Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| (2) RELU					|												|
| (3)Max pooling	      	| 2x2 stride,  outputs 14x14x6 				|
| (4) dropout					|										0.5		|
| (5) Convolution 5x5	    | 1x1 stride, valid padding, outputs 10x10x16     									|
| (6) RELU					|												|
| (7) Max pooling	      	| 2x2 stride,  outputs 5x5x16 				|
| (8) Convolution 5x5	    | 1x1 stride, valid padding, outputs 1x1x400     									|
| (9) RELU					|												|
| (10) Flatten 7 and 9					|									400, 400			|
| (11) Concatenate					|							800					|
| (12) dropout					|										0.5		|
| (13) Fully connected		| outputs 43 classes        									|



#### 4. Describe how, and identify where in your code, you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

The code for training the model is located in the tenth cell of the ipython notebook. 

To train the model, I used an Adam optimizer, a batch size of 128, 60 epochs, and 0.001 learning rate.

#### 5. Describe the approach taken for finding a solution. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

The code for calculating the accuracy of the model is located in the twelvth cell of the Ipython notebook.

My final model results were:
* training set accuracy of 99.6%
* validation set accuracy of 99.1% 
* test set accuracy of 94.0%

First, I used the LeNet model from the lectures preceeding the project after modifying it to accept RGB images as input and 43 classes as output. However, it had some limitations such as overfitting the train data causing poor validation accuracy. Accordingly, I investigated different architectures that are capable of handling such application, yet not complex to an unneeded extent.
I came across this research paper: https://github.com/andrewazmy/CarND-Traffic-Sign-Classifier-Project/blob/master/mscn.pdf, carefully understood its content, and implemented the mentioned architecture. Overfitting was still an issue, however, there was a noticeble improvement over the LeNet, which led me to investigate possible modifications to improve the accuracy of the sermanet architecture. These modifications include:
* Changing the filter dimensions to accept RGB input instead of greyscale images.
* Adding a dropout after the first convolution layer to decrease the effect of over-fitting.
From this point onwards, tunning of the paramaters was the key to achieving high accuracy. Learning rate, batch size, epoch count and keep probability were the parameters to be tuned. The aim was to reach a minimum difference between the training and validation accuracy, indicating that no over-fitting has occured, while at the same time reaching a high validation accuracy. After tunning, the below parameters were found to yield the best results:
* Learning rate of 0.001
* Batch size of 128
* Epoch count of 30
* Keep probability of 0.5
The added dropout layer achieved its aim of reducing the effect of over-fitting.


### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are 5 samples from the 25 German traffic signs that I found on the web:

![alt text][image3] ![alt text][image4] ![alt text][image5] 
![alt text][image6] ![alt text][image7]

The first image (speed limit 50) was mistakenly classified by the model as 

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. Identify where in your code predictions were made. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

The code for making predictions on my final model is located in the sixteenth cell of the Ipython notebook.

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 50 km/h	      		| 30 km/h			 				|
| 30 km/h	      		| 30 km/h				 				|
| Road work			| Road work      							|
| Roundabout     			| Roundabout 										|
| Stop Sign      		| Stop sign   									| 


The model was able to correctly guess 24 of the 25 traffic signs, which gives an accuracy of 96%. This compares favorably to the accuracy on the test set of 94%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction and identify where in your code softmax probabilities were outputted. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)
The graphs below show the softmax probabilities for the top 5 classes predicted by the model for each image. As mentioned above, the 50 km/h speed limit sign is the only image of which the model is wrong. When we come to think of the reasons causing this, the most common one is invalid, since the 50 km/h speed sign is the class with the maximum number of training samples. Accordingly, this misjudgement can be due to the fact that, unlike the training data set, the '50' digits are not centered within the red circle. This can be an indication of over-fitting the model to only recognize speed limit signs with the digits in the exact center of the red circle.

Another image about which the model is uncertain but has predicted correctly is the roundabout sign. This can be due to the strange affine transformation of the input image and the fact that it is much more brighter than expected.

![alt text][image8]
