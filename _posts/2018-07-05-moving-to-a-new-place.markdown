---
layout: post
title:  The prediction of the gait phase at time t from the motion sensor data
date:   2018-07-05 15:01:35 +0300
image:  05.avif
tags:   
---
The gait cycle is defined with reference to a given limb - left or right - and comprises stance phase (from the moment of heelstrike to the moment of toe-off) and swing phase (from toe-off to subsequent heelstrike); see Fig. 2. We further subdivide stance and swing into 6 phases, giving 12 phases in total so that the gait phase p lies in the interval (0,11). The phases are numbered consecutively from the start of stance to the end of swing. Phases 0-5 inclusive make up stance, with phase 0 representing the period immediately following heel-strike. Phases 6-11 make up swing, with phase 11 representing the period immediately preceding heelstrike. This project is to predict this scalar integer using the sensor data as input. That is, using any of the sensor data available up to and including (but not beyond) time t, predict the gait phase at time t.

The project will be consist of 5 parts:
* EDA
* Data Ingestion
* Modelling
* Result
* Conclusion


## EDA

The dataset is consist of 3 types of the motion such as joggin, walking, and other activities. The dataset structure is like below. There are 6 files such as jog1, jog2, walk1, walk2, var1, and var2.csv.
<p align="center"><img style="margin:0px 0 10px 0" src="{{ site.baseurl }}/images/35.png" width="100%" height="50%"></p>
I added time column as this data is the sequence data based on time assuming that each dataset collects and order by the time sequence.
<p align="center"><img style="margin:0px 0 10px 0" src="{{ site.baseurl }}/images/36.png" width="100%" height="50%"></p>
Check the distribution of labels for each dataset!!! We can see each dataset is needed the augmentation about label. Only var2 file is mostly uniformly distributed!
<p align="center"><img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/37.png" width="100%" height="50%"></p>
Check the outliers for each dataset! Each dataset looks lots of outliers in each boxplot. So, I assumed that when removing outliers, other factors check needed!
<p align="center" width="100%"><img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/38.png" align="center" width="45%">
<img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/43.png" align="center" width="45%"></p>
<p align="center" width="100%"><img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/44.png" width="45%">
<img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/45.png" width="45%"></p>
<p align="center" width="100%"><img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/46.png" width="45%">
<img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/47.png" width="45%"></p>




Each feature is normalised or not?? I found out the fact like below.

jog1 - acc_x_left, acc_x_right outliers remove needed

jog2 - acc_x_left, acc_x_right outliers remove needed

walk1 - no

walk2 - no

var1 - no

var2 - acc_x_left, acc_x_right outliers remove needed
<p align="center"><img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/39_0.png" width="100%" height="50%"></p>
Check the skewness for each dataset! if the skewness > 1,then log transform needed!
<p align="left"><img src="{{ site.baseurl }}/images/40.png" width="50%" height="50%"></p>
Check the correlation! When I try to remove outliers, I could refer this correlation.

jog1 - gyr_y_right negative correlation (-0.46), gyr_z_left positive correlation (0.72)

jog2 - gyr_y_right negative correlation (-0.35), gyr_z_left positive correlation (0.72)

walk1 - gyr_y_right negative correlation (-0.59), gyr_z_left positive correlation (0.65)

walk2 - acc_y_right, acc_z_left negative correlation (-0.32, -0.3), gyr_z_left positive correlation (0.57)

var1 - gyr_y_right negative correlation (-0.25), gyr_z_left positive correlation (0.56)

var2 - acc_z_left negative correlation (-0.16), gyr_z_left positive correlation (0.64)
<p align="center"><img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/41.png" width="100%" height="50%"></p>


## Data Ingestion
I divided the train/test dataset for the input dataset for the model.
I could basically get 3files (jog,walk,var) and preprocessed depending on method parameter. The method parametesr are like below.
1. Outliers removing
2. Log transformation
3. PCA
4. Feature selection
Of course, features standardisation is essential and the one-hot encoding for labels is in the model train step. To make even for the distirbution in labels, I utilised the SMOTE method. After augmentation for each dataset (jog,walk,var) is like below.
<p align="center"><img style="margin:0px 0 10px 0" src="{{ site.baseurl }}/images/42.png" width="100%" height="50%"></p>


## Modelling
I constructed the Autoencoder for extracting features and Softmax for classification.
1. Autoencoder
   
Autoencoder is an unsupervised learning technique that converts input into a signal through an encoder and then creates a label through a decoder. In this project, I utilised only the encoder part. It means I could get the specific features from the encoder part like the function of feature extraction. I used the latent vector Z for the input of the softmax classifier. (refer to the picture below)
<p align="center"><img style="margin:0px 0 10px 0" src="{{ site.baseurl }}/images/48.png" width="100%" height="50%"></p>
I utilised 2 hidden layers and sigmoid as an activation function in the encoder. For tuning the Autoencoder, I used RMSprop optimiser and MSE loss function. I defined the first hidden layer as 8, and the 2nd hidden layer as 4. 

2. Softmax
Softmax Regression can be considered a generalized logistic regression. It is not simply a model for binary classification, but a multinomial logistic regression (multinomial LR) for classifying multiple multi-classes. It is not a form of gathering and combining several binary classifiers, but a generalized form. Given a sample x, the softmax regression model calculates the score for each class 12 Calculate. The softmax function is applied to the scores to estimate the probability of each class. Refer to the image below.
<p align="center"><img style="margin:0px 0 10px 0" src="{{ site.baseurl }}/images/49.png" width="100%" height="50%"></p>
For tuning the Softmax, I used SGD optimiser and cross entropy loss function or hinge function. 
I defined the parameters models, dataset, learning rate, number of epochs, and display step so that I can experiment dependiong on parameters. Basically, I defined learning_rate_RMSProp=0.02, learning_rate_GradientDescent=0.5, num_epochs=100, display_step=1.


## Result
I used the accuracy of the model as the evaluation metric. I experimented the model dependion on the dataset (jog, walk, var) which was preprocessed and the parameters. Here is the results according to the conditions.
<p align="center"><img style="margin:0px 0 10px 0" src="{{ site.baseurl }}/images/50.png" width="100%" height="70%"></p>


## Conclusion
* jog file : All features are standardised only, was outperformed. This file is mostly not biased, so other preprocessed methods didn't work. It's enough for applying the extracted features from the Autoencoder.
* walk file : All features are standardised only, was outperformed. This file is mostly not biased, so other preprocessed methods didn't work. It's enough for applying the extracted features from the Autoencoder. Interestingly, the feature selection based on the correlation performed better than PCA.
* var file : This file is much more biased in 'acc_x_left' and 'acc_x_right'. Hence, the log transformation for these features worked well for extracting features from the Autoencoder.
  
  From this experiment, I found out the features extraction from the Autoencoder much more outperformed that other data preprocessing methods like PCA and feature selection.

