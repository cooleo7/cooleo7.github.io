---
layout: post
title:  The prediction churn of bank customers with KNN (K-Nearest Neigbours)
date:   2022-01-19 15:01:35 +0300
image:  03.webp
tags:   
---
We can identify which customers are likely to leave the bank through various ML & DL methods. It is crucial that banks conduct more thorough data analysis. The purpose of this project is to learn something new through careful data analysis such as __KNN (K-Nearest Neigbours).__

In the previous page, I experimented the bionomial losgistic regression. In this page, I exprimeted KNN with same dataset and the same conditions.
The project will be consist of 3 parts:
* Method explanation
* The condition in experiment
* Result
* Conclusion
* PCA & LGBM


## The concept of algorithm
<p align="center"><img src="{{ site.baseurl }}/images/21.png" width="100%" height="80%" title="px(픽셀) 크기 설정" alt="RubberDuck"></p>
step 1. Distance of Test data 1 : [4,3,2,1,0,9,10]
step 2. Try to sort elements in order as index : [4,3,2,1,0,5,6], since 4th element is the smallest and the next is 3rd element. Other elements are applied as same logic.
step 3. Match the element in Train label which the index is same : [1,0,1,0,0,0,0], since the 4th element of Train label is 1 and 3rd element of Train label is 0. Other elements are applied as same logic.
From [1,0,1,0,0,0,0], I calculated the probability of the occurrence of 0 and 1. The probability of the occurrence of 0 is assigned to 0th index and the probability of the occurrence of 1 is assigned to 1st index as [$\frac{5}{7}$, $\frac{2}{7}$]. Since $\frac{5}{7}$ > $\frac{2}{7}$, Test data 1 is classified to label 0 in this example.


## Conditions in experiment
I tried to apply each case below to the model. 
<p align="center"><img src="{{ site.baseurl }}/images/18.png" width="100%" height="80%" title="px(픽셀) 크기 설정" alt="RubberDuck"></p>


## Result
It’s interesting to see that KNN with log-transformed features outperforms Binomial Logistic Regression with log-transformed features. I carefully surmise that the fact that the algorithm’s foundation is based on distance is the reason why the outcome in case 9 is the best. In case 9, the extremely skewed balance is log converted, and the age outliers are completely eliminated. This demonstrates that log transformation works well with skewed data.
![]({{ site.baseurl }}/images/22.png)


## Conclusion
There is basically no performance difference between the two methods in this Bank data. The more productive method of modeling is the selection and manipulation of features. With the dimension reduction and the features standardised, binomial logistic regression performed well. But when the main features were log-transformed, KNN performed better.
The two algorithms’ different performance levels were a result of the two different approaches to data transformation, standardisation and log transformation. This study taught me that the algorithm utilised is not as important as how effectively the data qualities are understood and altered, such as by reducing dimensions and altering features.


## PCA & LGBM
I attempted to test the PCA and LGBM because I’m unsure if the feature selection for my model is functioning correctly or not. I tried to create two models and compare them to my model because LGBM is renowned as the highest-performing algorithm and PCA is helpful for dimension reduction.

1. PCA
   
   I made a two-dimensional PCA and applied it to KNN and binomial logistic regression. Unexpectedly, my feature selection outperforms the accuracy of two models when PCA is applied. (binomial logistic regression : 0.7963, KNN : 0.8147)

2. LGBM

   I tried to run plot importance provided by lightgbm library. Exceptionally, credit score was important feature as shown below. Despite using all features as input, the accu- racy is substantially higher than that of binary logistic regression and KNN.
   
<p align="center"><img src="{{ site.baseurl }}/images/23.png" width="70%" height="40%" alt="RubberDuck">
<img src="{{ site.baseurl }}/images/24.png" width="110%" height="60%" alt="RubberDuck"></p>

   
