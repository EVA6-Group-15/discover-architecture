# CodeCodeCode

## Objective
In iterative fashion, step by step, using one concept at a time, acheive **99.4%** Validation Accuracy **consistently** on MNIST Dataset with in **15 epochs** and in under **10,000 paramaters**.

## Approach
So, the target of **99.4%** is acheived in 6 iterations


### 1. Bare Skeleton Model - To check whether model is training or not.  
<img src="images/00_skeleton.jpg" width="400">

#### Targets:
To create a basic skeleton of the the model, and check Learning Capabilities.  
**Parameters:** Less than 10,000  
**Data Augmentations:** None  
**Regularization**: None  
**LR Scheduler**: None  
**No. of Epochs**: 14  
#### Results:
**Total Parameters Used**: 9,714  
**Train Accuracy**: 99.5  
**Test Accuracy**: **99.31**  
**Consistent From**: Not Consistent  
**Data Augmentation**: None
#### Analysis
The model is learning as we see that the training accuracy is going to 99.5, which is a good indicator initially, to continue on the skeleton of the model, and work on it to improve the Test Accuracy, as we see there is clearly Over fitting happening.

### 2. Lighter Model - Reduce No. of Parameters to 8000.  
<img src="images/01_lighter.png" width="400">

### 3. Add DropOut - Regularization to Avoid Over Fitting.  
<img src="images/02_dropout.jpg" width="400">

### 4. Data Augmentation - Increasing Training Data to Better Train.  
<img src="images/03_aug.jpg" width="400">

### 5. LR Schedulers - To Stabalize Network.
<img src="images/04_lr.jpg" width="400">

### 6. Increasing Receptive Field - Why Stop at 28x28, Let's go Hihger.    
<img src="images/05_rf.png" width="400">



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
