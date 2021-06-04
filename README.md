# Discover Network Architectures

## Objective

In iterative fashion, step by step, using one concept at a time, achieve **99.4%** Validation Accuracy **consistently** on MNIST Dataset with in **15 epochs** and in under **10,000 parameters**, even better if it's below **8,000 parameters**

## Approach

The target of **99.4%** is achieved in 6 iterations.

![Summary of all approaches](./images/Block%20Diagram.png)

### Target

- *Parameters:* Less than 10,000
- *Data Augmentations:* If required
- *Regularization*: If required
- *LR Scheduler*: If required
- *Max No. of Epochs*: 14
- *Test Accuracy*: 99.4%

### 1. Bare Skeleton Model - To check whether model is training or not

<img src="images/00_skeleton.jpg" width="400">

To create a basic skeleton of the model, and check learning capabilities.

#### Results

- Total Parameters used: 9,902
- Best Train Accuracy: 99.14%
- Best Test Accuracy: 99.00%
- Consistency: Did not achieve any consistency with test accuracy greater than 99.4
- Base network architecture design

#### Analysis

With the base network architecture, we achieved a test accuracy of 99% which is not even close to the target test accuracy to achieve. We plan to improve the test accuracy by doing the following approaches:

Split the process into two different networks by varying the number of parameters.

1. Keep the same network architecture, but removing the fully connected layers.
2. Reduce the number of parameters in the model.

In each of these approaches, we performed the following changes in order to assist in achieving the target test accuracy.

- Add Dropout layers
- Perform Data Augmentation
- Use Learning Rate Schedulers

### Less than 10k parameters

|Target<div style="width:200px"></div>|Results<div style="width:200px"></div>|Analysis|
|----|----|----|
|Remove FC layer|Parameters: 9,060<br>Train Accuracy: 99.45%<br>Test Accuracy:99.33%|We removed the fully connected layers from the base network architecture. This increased the test accuracy by \~0.15%. However, we did not reach the target test accuracy. <br> Next we add dropout layers to the network to check if it improves the test accuracy.|
|Add Dropout Layer|Parameters: 9,060<br>Train Accuracy: 99.06%<br>Test Accuracy:99.29%|Addition of Dropout layer with dropout value of 0.043, helps improve the test accuracy. However, we did not reach the target test accuracy. <br>Next we add data augmentation while training the network and check the increase in the test accuracy.|
|Data Augmentation|Parameters: 9,060<br>Train Accuracy: 98.88%<br>Test Accuracy:99.39%|Addition of Data augmentation (Random Rotation) increased the chances of reaching the target accuracy of 99.4%. We hit validation accuracy of 99.39 in the last two epochs. <br>In order to maintain a consistent accuracy of 99.4% in few more epochs we next add the Learning Rate scheduler. We observed that the accuracy was alternating between two ranges every 2 epochs. One reason could be that the network was not able to converge and reach a minima. Using LRScheduler provides us a better chance of reaching minima as the step size reduces.|
|Learning Rate Scheduler|Parameters: 9,060<br>Train Accuracy: 99.12%<br>Test Accuracy:99.47%|The StepLR helped in stabilizing the learning, by reducing the learning rate to 10% after every 6th Epochs. <br>Why after every 6 epochs? We observed that the loss goes up and down from the 6th/7th epoch; so reducing the LR at that point makes the training stable.|


### Less than 8k parameters

|Target<div style="width:200px"></div>|Results<div style="width:200px"></div>|Analysis|
|:----:|----|----|
|Reducing number of parameters<br>|Parameters: 7,826<br>Train Accuracy: 99.51%<br>Test Accuracy:99.22%|We achieved the required parameter count (< 8000). The model is still able to learn as we see that the training accuracy reaches 99.5% even with number of parameters < 8000.<br>Next we introduce reqularization techniques such as DropOut or Data Augmentation to improve Test Accuracy, as we observe that the network is over fitting.|
|Add Dropout Layer|Parameters: 7,826<br>Train Accuracy: 99.25%<br>Test Accuracy:99.29%|We add DropOut of 5% (i.e) 5 out of 100 weights are dropped after each layer during training.<br>We observe that over-fitting does not occur and both the test loss and accuracy are higher than the training loss and accuracy. This shows that the model's learning capabilities can be increased further to get better test accuracies.<br>Also, in the confusion matrix and incorrect identified images (in the notebook) we see that the model is confused between 0 and 6 as there are quite high incorrect prediction of 6 as 0s. Same behaviour is seen with 2 and 7.<br>Next we add data augmentation techniques such as RandomRotation of ±15° while training.|
|Data Augmentation|Parameters: 7,826<br>Train Accuracy: 98.73%<br>Test Accuracy:99.36%|Alongwith DropOut of 5% after each layer we increase our dataset by adding agumentation. We use RandomRotation that rotates iamges randomly between ±15°.<br>We observed a decrease in the training accuracy whereas there is an increase in test accuracy which is a good sign.<br>However the test loss is not stable and goes from 0.21 to 0.25 and does not converge. The learning rate of 0.01 may be too large at this point which is from epoch 6/7.<br>The next step is to use Dynamic LR.|
|Learning Rate Scheduler|Parameters: 7,826<br>Train Accuracy: 99.17%<br>Test Accuracy:99.48%|The StepLR helped to stabilize the learning by reducing the learning rate to 10% after every 6th Epochs.<br>Why after every 6 epochs?<br>We observed that the loss was bouncing up and down from 6th/7th epoch; reducing the LR at this point would make the training stable.<br>We observe that the accuracy/loss in testing is bouncing heavily in the first 6 epochs and after 6 epochs the learning rate is decreased to 10% (=0.001) thus, the traning stabilizes. The loss and accuracy is stable and converges slowly from 99.39 at Epoch 7 to 99.48 by end of training.<br>Therefore, the use of LR stabilizes the training process, but identifying the correct epoch to reduce LR is challenging and needs manual tuning with multiple runs.<br>We achieved the final target of consistently getting 99.4% test accuracy for half the epochs, proving very good convergence of the model.|

## Receptive Field

### Model 1

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

### Model 2

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

<br>
<br>

![Morgan!](./images/stopplayinggod.jpg)
