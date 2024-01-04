---
layout: post
title:  Bank customers segmentation using K-means depediong on features, meanshift,GMM, and RFM
date: 2022-01-25 10:01:35 +0300
image:  07.jpg
tags:   
---
This project is the bank customers segmentation using __k-means.__ This dataset can be downloaded <a href="[https://www.kaggle.com/datasets/tanishaj225/loancsv/code?datasetId=1198164](https://www.kaggle.com/datasets/shivamb/bank-customer-segmentation/data)">here</a>.

The goal of this project is finding out the importance of features extraction for calssification problem. The project will be consist of 4 parts:
* EDA - Data preprocessing 
* Method explanation
  
  (1) K-means with features selection
  
  (2) K-means with PCA
  
  (3) K-means with features extraction from Encoder
* Conclusion


## EDA - Data preprocessing 
![]({{ site.baseurl }}/images/71.png)
*Data Structure*
This dataset is quite huge over 1000000 raws. I removed null values and created the age variable that might be impactful feature. However, the bias of age is quite serious like below.
<p align="center" width="100%"><img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/72.png" align="center" width="45%">
<img style="margin:0px 0 0px 0" src="{{ site.baseurl }}/images/73.png" align="center" width="45%"></p>
Therefore, I removed the age under 0 and over 200. The preprocessed age feature distribution is like below.
<p align="center"><img src="{{ site.baseurl }}/images/74.png" width="80%" height="50%"></p>
Next, focus on the money variables! The bias is also heavy like the age.
<p align="center"><img src="{{ site.baseurl }}/images/75.png" width="100%" height="100%"></p>
It has to be normalised through log transformation.
<p align="center"><img src="{{ site.baseurl }}/images/76.png" width="100%" height="100%"></p>
Next, go through the locations! The location column has too many categories over 7500. I removed the categorical values that proportioned under 10%. In result, only 1000 locations left. After that, categorical columns like gender and location are converted to numerical values to apply K-means. I used the label encoder method. 
Find out the correlations between varialbes!
<p align="center"><img src="{{ site.baseurl }}/images/77.png" width="100%" height="100%"></p>
I discovered a bit strong relationship between the age and money column that log transformed. Before applying K-means, I made all features standardised and left only 100000 raws. Lastly, I checkd the bias of all features like below.
<p align="center"><img src="{{ site.baseurl }}/images/78.png" width="100%" height="100%"></p>
All columns are mostly normalised except the previous categorical variables. I removed the money variables that is not log transformed.


## K-means with features selection
I tried optimal K from K-means using the preprocessed dataset that includes all features such as TransactionDate, CustomerAge, CustAccountBalance_log, TransactionAmount (INR)_log, Gender, and Location. I set the parameters for K-means like "init":"k-means++", "max_iter":300, "random_state":0. To figure out the optimal K, I used the elbow method and checked the silhouette values like below.
<p align="center"><img src="{{ site.baseurl }}/images/79.png" width="100%" height="100%"></p>
From this, I got the optial K is 5. I visualised the area of silhouette coefficients depending on the number of clusters as I want to make sure that it's reasonable.
<p align="center"><img src="{{ site.baseurl }}/images/80.png" width="100%" height="100%"></p>
Even though the silhouette coefficients at K=4 is the biggest, the area of the silhouette coefficient in each cluster is mostly equal based on the average of silhouette score at K=5. 

From the correlation analysis, I found out a bit strong relationship between age and CustAccountBalance_log. Therefore, I visualised the clusters using 2 main features age and CustAccountBalance_log like the below. But it didn't look clearly classified.
<p align="center"><img src="{{ site.baseurl }}/images/81.png" width="100%" height="100%"></p>


## K-means with PCA
From the first experiment, I concluded it needed feature extraction. Based on the correlation analysis, I set the number of the main features to 3. I utilised PCA as features extraction. I tried optimal K from K-means using the preprocessed dataset that includes 3 principal features from PCA. 

The parameters for K-means like "init":"k-means++", "max_iter":300, "random_state":0
<p align="center"><img src="{{ site.baseurl }}/images/82.png" width="100%" height="100%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/83.png" width="100%" height="100%"></p>
I visualised the clusters using 2 principal features from PCA like below. It looked more clear than the first experiment, but it was not still celarly classified.
<p align="center"><img src="{{ site.baseurl }}/images/84.png" width="100%" height="100%"></p>


## K-means with Encoder method
From the PCA, it needed more exact features extraction. The idea of this method is to use the encoder part of Autoencoder method. From the encoder, I got 2 main features. I utilised 2 hidden layers each size 4 and 2.

The parameters for K-means like "init":"k-means++", "max_iter":300, "random_state":0
<p align="center"><img src="{{ site.baseurl }}/images/85.png" width="100%" height="100%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/86.png" width="100%" height="100%"></p>
I visualised the clusters using 2 main features from encoder like below. It was quite celarly classified like below.
<p align="center"><img src="{{ site.baseurl }}/images/87.png" width="100%" height="100%"></p>


## Conclusion
K-means is the one of the outperformed methods for classification. The most important thing is to set the K that affects the performace. From 3 experiments, I found out the importance of features extraction and the encoder was outperformed among them.




