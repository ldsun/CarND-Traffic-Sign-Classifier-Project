**Traffic Sign Recognition** 

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

[survey_samples]: ./write_up/survey_samples.png "Survey samples"
[before_preprocessing]: ./write_up/before_preprocessing.png "before grayscaling"
[after_preprocessing]: ./write_up/after_preprocessing.png "after grayscaling"
[augment_image]: ./write_up/augment_image.png "augment image"
[test_image1]: ./test/test1.jpg "Right-of-way at the next intersection"
[test_image2]: ./test/test2.jpg "Traffic Sign 4"
[test_image3]: ./test/test3.jpg "Traffic Sign 5"
[test_image4]: ./test/test4.jpg "Traffic Sign 4"
[test_image5]: ./test/test5.jpg "Traffic Sign 5"
[top_5_softmax]: ./write_up/top_5_softmax.png "top 5 softmax"


## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---

###Data Set Summary & Exploration

####1. A basic summary of the data set.

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32*32*3
* The number of unique classes/labels in the data set is 43

####2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. I plotted all the classes in the training set, and I also plotted a bar chart to display the distribution. 

![survey samples][survey_samples]

###Design and Test a Model Architecture

####1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

Here are the steps that I preprocessed the image data. 

First, convert the image to grayscale so that it becomes one channel. This reduces the learning work-load, and for such task, usually there's no difference between color image and gray scale image.

Second I normalized the data so that the number during training won't get too large. 

To compare traffic sign image before and after preprocessing, I plotted first 10 images in each class. 

Before preprocessing:

![before preprocessing][before_preprocessing]

After preprocessing:

![after preprocessing][after_preprocessing]


From the training set distribution bar chart, we can see that some image has more samples, while some only has a few. So different images get different chance of training. To make sure all images get enough training, I generated additional data.  

To add more data to the the data set, I applied below transformation on original data:
1) scaling
2) warp
3) brightness

Here is an example of an original image and an augmented image:

![augment image][augment_image]

The difference between the original data set and the augmented data set:

Origial data set has size: 34799

Augmented data set has size: 51690

####2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 gray scale image   					| 
| Convolution 5x5     	| 1x1 stride, VALID padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6 		    		|
| Convolution 5x5	    | 1x1 stride, VALID padding, outputs 10x10x16  	|
| RELU	            	|           									|
| Max pooling			| 2x2 stride,  outputs 5x5x16 					|
| dropout   			| keep_probe 0.5								|
| Fully connected		|												|
| RELU					|												|
| dropout   			| keep_probe 0.5								|
| Fully connected		|												|
 


####3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used an AdamOptimizer with learing rate 0.001, batch size 128, and number of epochs is 50. 

####4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.966
* validation set accuracy of 0.940 
* test set accuracy of 0.800

I used classical LeNet as discribed in the course which only gave me an accuracy of 0.93 on the training set. To improve the LeNet, I added dropout layer and tuned other layers. I also tried different learning rate, batch size and number of epochs, and it turned out that for current network, accuracy becomes stable when reaching 50 epochs, and keep learning rate low as 0.001 show acceptable learning result. I didn't see much difference when changing batch size, this could be because of training data is small and can be all cached in memory. 
 
###Test a Model on New Images

####1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![test iamge 1][test_image1] ![test iamge 2][test_image2] ![test iamge 3][test_image3] 
![test iamge 4][test_image4] ![test iamge 5][test_image5]

The last image "Keep right" might be difficult to classify because if we convert image to grayscale, it is very similiar to sign 17, "No entry". That's why in the test, the model failed on this one. 

####2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			                           |     Prediction	        					 | 
|:----------------------------------------:|:-------------------------------------------:| 
| Keep right      		                   | No Entry   	                             | 
| Right-of-way at the next intersection    | Right-of-way at the next intersection 	     |
| Priority road					           | Priority road	    	                     |
| Double curve	      		               | Double curve			                     |
| Speed limit (50km/h)			           | Speed limit (50km/h)                        |


The model was able to correctly guess 4 of the 5 traffic signs, which gives an accuracy of 80%. 

####3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

I plotted a graph to show how certain the model is when predicting. In the plot, I showed top 5 softmax probabilities for each image. 

![top 5 softmax][top_5_softmax]

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
####1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


