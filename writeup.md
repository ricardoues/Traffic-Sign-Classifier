
**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./images/bar_graph.png "Visualization"
[image2]: ./images/traffic_sign01.png "Grayscaling"
[image3]: ./images/traffic_sign02.png "Traffic Sign 1"
[image4]: ./images/traffic_sign03.png "Traffic Sign 2"
[image5]: ./images/traffic_sign04.png "Traffic Sign 3"
[image6]: ./images/traffic_sign05.png "Traffic Sign 4"
[image7]: ./images/traffic_sign06.png "Traffic Sign 5"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it! and here is a link to my [project code](https://github.com/ricardoues/Traffic-Sign-Classifier/)

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used pandas and numpy libraries to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3) 
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the frequency of the labels are distribuited in the range between 20 and 39.

![alt text][image1]

It seems that some labels are more frequent than others in the training set, so that this is an unbalanced classification problem. 

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to grayscale because the color is not relevant in order to identify the traffic sign. Moreover the following paper converts the images to grayscale in order to improve the perfomance of the deep learning model. 

[http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf](http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf)

Here is an example of a traffic sign image before and after grayscaling.

![alt text][image2]

As a last step, I normalized the image data in order to avoid problems with numerical precision. 

I decided to generate additional data because the data is unbalanced. To add more data to the the data set, I used oversampling because it is easy to implement but can increase the learning time. As Adam Optimizer (A variant of stochastic gradient descent) was used, the learning time was not a problem. Other techniques can be used to treat with unbalanced classification problems, such as: modified the cost function, generate augmented data using generative adversial models, and so on.  


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

First of all, I used LeNet architecture with some minor modifications. The Lenet architecture shows overfitting that is why we need to use dropout to improve the generalization of the model. 

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 grayscale image   							| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x64 				|
| Convolution 5x5	    | 1x1 stride, valid padding, outputs  10x10x6   |
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x64 				|
| Fully connected		|   input = 400, output = 120     									|
| RELU					|												|
| Fully connected		|   input = 120, output = 84     			|
| RELU					|												|
| Dropout					|			keep_prob = 0.25									|
| Output layer				|         									|


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used Adam optimizer to update networks weights iterative based in training data. Among the advantages of using Adam optimizer are computationally efficient, little memory requirements, well suited for problems that are large in terms of data and/or parameters, and hyperparameteres require little tuning. 

Trial and error approach was used to select the following parameters in the training phase: learning rate = 0.001, number of epochs = 10, batch size = 128 , and dropout = 0.25. The LeNet architecture without dropout shows 

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

Firstly, we have proposed two models using the LeNet architecture. The first model has the following hyperparameters: rate 0.001, epochs 10, and batch size of 128. The second model has a rate of 0.001, an epochs of 100, and a batch size of 256. The accuracy of these models on the train, validation and test set are as follows:

| Model   | Train accuracy | Valid accuracy | Test accuracy |
|---------|----------------|----------------|---------------|
| Model 1 | 0.988          | 0.895          | 0.881         |
| Model 2 | 1.0            | 0.932          | 0.923         |

It is clear that with the LeNet architecture, we have obtained overfitting models that is why we are going to modify the LeNet code to include dropout.

With the LeNet architecture with dropout, three models were proposed. The accuracy of these models on the train, validation and test set are as follows:

| Model                     | Train accuracy | Valid accuracy | Test accuracy |
|---------------------------|----------------|----------------|---------------|
| Model 1 (dropout = 0.5)   | 0.998          | 0.929          | 0.921         |
| Model 2 (dropout = 0.25)  | 0.998          | 0.950          | 0.931         |
| Model 3 (dropout = 0.5)   | 0.990          | 0.932          | 0.905         |


The model with better performance in the validation set is the model 2 with LeNet architecture and dropout equal 
0.25, it is the model that we choose to predict new images. Finally, the accuracy of this model in the test set is the highest among all models. 


My final model results were:
* training set accuracy of 0.998
* validation set accuracy of 0.950 
* test set accuracy of 0.931

If a well known architecture was chosen:
* What architecture was chosen? LeNet architecture was chosen.
* Why did you believe it would be relevant to the traffic sign application? The LeNet architecture is a good choice since it is straightforward and small. Moreover, with Lenet we have achieved extremly fast training times (training times less than 10 minutes).  
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well? The estimation of the accuracy on the training, validation and test set are very stable, we have proved these architecture three times and the results were very similar to those shown in the table above. It is possible to use a more sophisticated method such as cross validation but due to time restrictions, it was not possible.  
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image3] ![alt text][image4] ![alt text][image5] 
![alt text][image6] ![alt text][image7]

It is observed that the previous images are clear, it is possible that is the reason of they were classified correctly.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Speed limit (30km/h)      		| Speed limit (30km/h)  									| 
| Speed limit (70km/h)     			| Speed limit (70km/h)										|
| Yield					| Yield											|
| General caution	      		| General caution					 				|
| Priority road			| Priority road      							|


The model was able to correctly guess 5 of the 5 traffic signs, which gives an accuracy of 100%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 95th cell of the Ipython notebook.

For the first image, the model is completely sure that this is a Speed limit (30km/h) sign (probability of 9.99446690e-01), and the image does contain a Speed limit (30km/h) sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 9.99446690e-01        			| Speed limit (30km/h)    									| 
| 4.94337175e-04    				|  Speed limit (20km/h)										|
| 3.84170671e-05					| 	Speed limit (50km/h)									|
| 2.05533906e-05      			| 		Speed limit (70km/h)		 				|
| 5.61166331e-08			    |  Speed limit (120km/h)    							|


For the second image, the model is completely sure that is a Speed limit (70km/h) sign (probability of 9.63142693e-01), and 
the image does contain a Speed limit (70km/h) sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 9.63142693e-01        			| Speed limit (70km/h)    									| 
| 2.17354931e-02   				|  Speed limit (120km/h)										|
| 9.65417735e-03				| 	Speed limit (30km/h)									|
| 5.35637327e-03     			| 			Speed limit (50km/h)	 				|
| 4.99302623e-05			    |    Speed limit (80km/h)  							|


For the third image, the model is completely sure that is a Yield sign (probability of 1.00000000e+00 !!), and 
the image does contain a Yield sign. The Yield sign have a simple pattern that is why the model gives high value of probability. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.00000000e+00        			| Yield    									| 
| 1.24625038e-21   				|  No vehicles									|
| 2.98008868e-23				| 	Speed limit (60km/h)									|
| 4.12624183e-28     			| 			Speed limit (80km/h)				|
| 	7.38018582e-29		    |    Ahead only 							|


For the fourth image, the model is completely sure that is a General Caution (probability of 9.51410234e-01), and 
the image does contain a General Caution sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 9.51410234e-01        			| General Caution    									| 
| 4.79275174e-02  				|  				Traffic signals					|
| 6.56011689e-04			| 			Road narrows on the right							|
| 6.07826451e-06     			| 			Pedestrians				|
| 	1.47831400e-07	    |    			Dangerous curve to the right				|

For the fifth image, the model is completely sure that is a Priority road (probability of 9.93026018e-01), and 
the image does contain a Priority road sign. The top five soft max probabilities were

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
|   9.93026018e-01      			| Priority road   									| 
|   2.80808611e-03				|  				Roundabout mandatory				|
| 	1.72838150e-03	| 		End of all speed and passing limits						|
|  6.55918906e-04   			| 			Keep right				|
|  6.21806248e-04   |    			End of no passing			|



### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
####1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


