# CodeCodeCode

## Objective
In iterative fashion, step by step, using one concept at a time, acheive **99.4%** Validation Accuracy **consistently** on MNIST Dataset with in **15 epochs** and in under **10,000 paramaters**, even better if it's below **8,000 parameters**

## Approach
So, the target of **99.4%** is acheived in 6 iterations


### 1. Bare Skeleton Model - To check whether model is training or not.  
<img src="images/00_skeleton.jpg" width="400">

#### Targets:
To create a basic skeleton of the the model, and check Learning Capabilities.  
*Parameters:* Less than 10,000  
*Data Augmentations:* None  
*Regularization*: None  
*LR Scheduler*: None  
*No. of Epochs*: 14  
#### Results:
*Total Parameters Used*: 9,714  
*Train Accuracy*: 99.5  
*Test Accuracy*: **99.31**  
*Consistent From*: Not Consistent  
*Data Augmentation*: None
#### Analysis
The model is learning as we see that the training accuracy is going to 99.5, which is a good indicator initially, to continue on the skeleton of the model, and work on it to improve the Test Accuracy, as we see there is clearly Over fitting happening.

### 2. Lighter Model - Reduce No. of Parameters to 8000.  
<img src="images/01_lighter.png" width="400">

#### Targets:
Since, we have a Basic Skelton of the Archeiture ready, Now let's tweak the Number of Kernels being used and try to bring the count below 8000.  
*Parameters*: Less than 8,000  
*Data Augmentations*: None  
*Regularization*: None  
*LR Scheduler*: None  
*No. of Epochs*: 14 
#### Results:
*Total Parameters Used*: 7,826 
*Train Accuracy*: 99.51  
*Test Accuracy*: **99.22**  
*Consistent From*: Not Consistent  
*Data Augmentation*: None
#### Analysis
We have got the Parameters Count, to where we wanted, below **8000**. The model is still able to learn as we see that the training accuracy is going to 99.5 even with below 8000 parameters.

Now we can use this Model with lesser parameters, and add Regularization using DropOut or do some Data Augmentation to improve Test Accuracy, as we again see that there is still Over fitting going on

### 3. Add DropOut - Regularization to Avoid Over Fitting.  
<img src="images/02_dropout.jpg" width="400">

#### Targets:
Since we have finalized our Model and acheived the Number of Parameters required, We have seen that there is over-fitting occuring during the training as our Training Accuracy is reaching 99.5+ while Test Accuracy is still hovering around 99.2, and any further training may not translate to 99.4+ Test Accuracy

To combat this let's add DropOut.

*Parameters*: Less than 8,000  
*Data Augmentations*: None  
*Regularization*: DropOut  
*LR Scheduler*: None  
*No. of Epochs*: 14  
#### Results:
*Total Parameters Used*: 7,826  
*Train Accuracy*: 99.25  
*Test Accuracy*: **99.29**  
*Consistent From*: Consistent at *99.25-99.3*  
*Data Augmentation*: None
#### Analysis
We have used DropOut of 5%, which is like dropping 5 out of 100 weights after each layer.

We can observe that, the over-fitting issue has been resolved, as clearly both the Test Loss and Accuracy of the Model are higher than the Training Loss and Accuracy, so which means there is still Learning opportunities and we can puish the training further and get better Test Accuracies.

Also, when observing the Confusion Matrix and the Incorrect Identified Images above, we can see that the Model is confusing between 0 and 6 as there are quite high incorrect prediction of 6 as 0's. Same with 2 and 7.

So, we can now bring some Data Augmentation techniques to Randomly Rotate images while Training, and with some estimation the Rotations look between ±15°.

### 4. Data Augmentation - Increasing Training Data to Better Train.  
<img src="images/03_aug.jpg" width="400">

#### Targets:
As, we have solved the Over Fitting issue, now let's push the Training by Adding Data Augmentation.  
We have seen from Incorrect classified images, the issue looks like the numbers are having varies degree of orientation and the model is getting confused to predict few numbers, so let's add a Random Rotation.


*Parameters*: Less than 8,000  
*Data Augmentations*: RandomRotation ±15°  
*Regularization*: DropOut  
*LR Scheduler*: None  
*No. of Epochs*: 14  
#### Results:
*Total Parameters Used*: 7,826  
*Train Accuracy*: 98.73  
*Test Accuracy*: **99.36**  
*Consistent From*: Consistent at *99.25-99.3*  
*Data Augmentation*: None
#### Analysis
We have used DropOut of 5%, after each layer and along with that now we are virtually increasing our dataset by adding agumentation, here we are using RandomRotation, which rotates iamges randomly between ±15°.

We have even decreased the training accuracy, and increased test accuracy by using Augmentation, which is a good sign.

But the Test Loss as we observe is not stable and is bouncing from 0.21 to 0.25 and not converging. So the Learning Rate of 0.01 may be too big at this point which is from epoch 6/7.  
So using some kind of Dynamic LR would be effecient.

### 5. LR Schedulers - To Stabalize Network.
<img src="images/04_lr.jpg" width="400">

#### Targets:
To converge the Loss, the LR of 0.01 is may be not the correct way, but using lower LR may take ages, so we will use a LR Scheduler, in particular StepLR to reduce the Lr after certain number of epochs, where we feel the Loss is bouncing.

*Parameters*: Less than 8,000  
*Data Augmentations*: RandomRotation ±15°  
*Regularization*: DropOut  
*LR Scheduler*: StepLR  
*No. of Epochs*: 14
#### Results:
*Total Parameters Used*: 7,826  
*Train Accuracy*: 99.165  
*Test Accuracy*: **99.48**  
*Consistent From*: **7th Epoch to End**  
*Data Augmentation*: Randam Rotation of ±15°  
*LR Scheduler*: StepLR
#### Analysis
The StepLR helped in stabilizing the learning, by reducing the learning rate to 10% after every 6th Epochs.

Why after every 6 epochs?  
We observed that the Loss was bouncing up and down, from 6th/7th epoch, so reducing the LR at that point would made the training stable.

And we can clearly see that the Accuracy/Loss in Testing is Bouncing heavily in the first 6 epochs, and after 6 epochs as we bring down the Learning Rate to 10% which is 0.001, the traning is stabilized and the loss as well as as Accuracy is pretty much stable and converging slowly from **99.39** at Epoch 7 to **99.48** by end of Training

So the use of LR, has brough stabilization to the training process, *but selecting the correct epoch to reduce LR is challenging task and is human-controlled so need's fine-tuning and several trials before getting it right.*

We have acheived the Final target of consistently getting 99.4% Test Accuracy, for half the epochs, proving very good convergence of the model.

But, we calculated the Receptive Field of the Model, and it's at 28, we wanted to see what happens when we increase the RF from 28 to 32, by adding one more Conv2f layer, but in effect keeping same number of total parameters.


### 6. Increasing Receptive Field - Why Stop at 28x28, Let's go Hihger.    
<img src="images/05_rf.png" width="400">

#### Targets:
To create a model with higher Recpetive Field than 28

*Parameters*: Less than 8,000  
*Data Augmentations*: RandomRotation ±15°  
*Regularization*: DropOut  
*LR Scheduler*: StepLR  
*No. of Epochs*: 14
#### Results:
*Total Parameters Used*: 7,946  
*Train Accuracy*: 99.2  
*Test Accuracy*: **99.51**  
*Consistent From*: *7th Epoch to End*  
*Data Augmentation*: Randam Rotation of ±15°  
*LR Scheduler*: StepLR
#### Analysis
The Increase in Receptive Field may have helped, as we were able to cross the **99.5%** barrier and the model is even more stable now

## Receptive Field

### Model 1:
![network_28](images/network_28.png)
|Layer No.|Operation|Input Feat.|Kernel Size|Padding|Stride|Output Feat.|Jump in|Jump Out|RF in|RF Out|
|:-------:|:-------:|:---------:|:---------:|:-----:|:----:|:----------:|:-----:|:------:|:---:|:----:|
|1|Conv2d|28|3|0|1|26|1|1|1|3|
|2|Conv2d|26|3|0|1|24|1|1|3|5|
|3|MaxPool|24|2|0|2|12|1|2|5|6|
|4|Conv2d|12|3|0|1|10|2|2|6|10|
|5|Conv2d|10|3|0|1|8|2|2|10|14|
|6|Conv2d|8|3|0|1|6|2|2|14|18|
|7|Conv2d|6|3|0|1|4|2|2|18|22|
|8|GAP|4|4|0|1|1|2|2|22|28|

### Model 2:
![network_32](images/network_32.png)
|Layer No.|Operation|Input Feat.|Kernel Size|Padding|Stride|Output Feat.|Jump in|Jump Out|RF in|RF Out|
|:-------:|:-------:|:---------:|:---------:|:-----:|:----:|:----------:|:-----:|:------:|:---:|:----:|
|1|Conv2d|28|3|**1**|1|28|1|1|1|3|
|2|Conv2d|28|3|**1**|1|28|1|1|3|5|
|3|MaxPool|28|2|0|2|14|1|2|5|6|
|4|Conv2d|14|3|0|1|12|2|2|6|10|
|5|Conv2d|12|3|0|1|10|2|2|10|14|
|6|Conv2d|10|3|0|1|8|2|2|14|18|
|7|Conv2d|8|3|0|1|6|2|2|18|22|
|8|Conv2d|6|3|0|1|4|2|2|22|26|
|9|GAP|4|4|0|1|1|2|2|26|32|
