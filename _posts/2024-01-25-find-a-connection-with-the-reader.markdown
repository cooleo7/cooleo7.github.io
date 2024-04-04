---
layout: post
title:  Insurance pricing utilising XGBoost, Logistic Regression, LGBM, Random Forest, and MLP
date: 2024-01-25 10:01:35 +0300
image:  8.jpeg
tags:   
---
This project is about building the insurance premium pricing model utilising XGBoost, Logistic regression, LGBM, Rnadom Forest, and MLP. 

The goal of this project is finding out the optimal premium price for the customers. I also investigated which features are important for the final model and the relationship between the price elasticity and the change in the price. The project will be consist of 4 parts:
* EDA & Data preprocessing 
* Experiment - XGB, Logistic regression, LGBM, Random Forest, and MLP
* Feture importance
* Experiment with the change in premium price
* Conclusion


## EDA & Data preprocessing 
<!--![]({{ site.baseurl }}/images/88.png)-->
<p align="center"><img src="{{ site.baseurl }}/images/88.png" width="200" height=50></p>
*Data Structure*
This dataset is 2000 raws, 15 columns, and all values are Non-null. So I don't need to do null values preprocessing. Overall, I looked through the relations between all nemerical variables.
I found out the positive relation between Claims_Amount and  Claims_Count , weak positive relation between Premium and Price_Diff. 
<p align="center"><img src="{{ site.baseurl }}/images/89.png" width="100%" height="100%"></p>
In summsry, we can see the relations in the heatmap below.
<p align="center"><img src="{{ site.baseurl }}/images/90.png" width="100%" height="50%"></p>
* Strong positive relation between Claims_Amount and  Claims_Count. 
* Weak positive relation between Premium and  Price_Diff.  
* Weak positive relation between Claims_Amount  and  Plan_Count,  Claims_Count  and  Plan_Count.
Next, I looked through the relations between each feature and target. 
More claims customers have, more accepting an offer like below (left). (Claim_flag 1 : Claims they have, 0 : No claim.) Even though the premium decreased, there is no tendency to accept an offer. Even if the premium increases, they tend to accept an offer slightly more proportionally like below (right). (Price_Cat -1 : premium decrease, 0 : base premium, 1 : premium increase)
<p align="center"><img src="{{ site.baseurl }}/images/91.png" width="50%" height="50%"><img src="{{ site.baseurl }}/images/92.png" width="50%" height="50%"></p>
More plans they have, more accepting an offer!
<p align="center"><img src="{{ site.baseurl }}/images/93.png" width="100%" height="50%"></p>
Overall more age, more accepting an offer, but customers who have no plan are more likely to accept an offer after 2years. (Age_Cat 1 : less than 1 year, 2 : between 1 and 2 year, 3: more than 3years)
<p align="center"><img src="{{ site.baseurl }}/images/94.png" width="50%" height="50%"><img src="{{ site.baseurl }}/images/95.png" width="50%" height="50%"></p>
Next, I checked outliers of data. From the boxplots below, outliers of Premium and Purchase_Price needs to remove.
<p align="center"><img src="{{ site.baseurl }}/images/96.png" width="50%" height="50%"><img src="{{ site.baseurl }}/images/97.png" width="50%" height="50%"></p>
I also found out the wrong values in Age column. So, it needs to be modified. Age = Cover_Start_Date - Purchase_Date.


## Experiment - XGB, Logistic regression, LGBM, Random Forest, and MLP
I tried experimenting with various conditions and algorithms. This is the outcome of the trials.
As a final model, I chose the XGBoost. (red) Its accuracy is mostly the highest of all the models I tried, of course, but it also performs the best in terms of other metrics. However, we must exercise caution when evaluating the model's performance based just on accuracy. I believe that recall is a crucial component in this industry since it directly affects sales. The model's output comes first, and then marketing & pricing strategy, which in turn produces sales results. See the metrics like below.
<p align="center"><img src="{{ site.baseurl }}/images/99.png" width="50%" height="50%"><img src="{{ site.baseurl }}/images/100.png" width="50%" height="50%"></p>


## Feture importance
To investigate the importance of features, I utilised the shap and the XGB plot_importance. Plan_flag, Plan_Count, Age_Cat, and Claims_Count are contributed in aspect of the shap. Price_Diff, Purchase_Price, Premium, and SP_CA are contributed more than other features based on XGB plot_importance.
<p align="center"><img src="{{ site.baseurl }}/images/82.png" width="100%" height="100%"></p>


## K-means with Encoder method
From the PCA, it needed more exact features extraction. The idea of this method is to use the encoder part of Autoencoder method. From the encoder, I got 2 main features. I utilised 2 hidden layers each size 4 and 2.

The parameters for K-means like "init":"k-means++", "max_iter":300, "random_state":0
<p align="center"><img src="{{ site.baseurl }}/images/85.png" width="100%" height="100%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/86.png" width="100%" height="100%"></p>
I visualised the clusters using 2 main features from encoder like below. It was quite celarly classified like below.
<p align="center"><img src="{{ site.baseurl }}/images/87.png" width="100%" height="100%"></p>


## Conclusion
K-means is the one of the outperformed methods for classification. The most important thing is to set the K that affects the performace. From 3 experiments, I found out the importance of features extraction and the encoder was outperformed among them.




