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


<br><br>
## EDA & Data preprocessing 
<!--![]({{ site.baseurl }}/images/88.png)-->
<p align="center"><img src="{{ site.baseurl }}/images/88.png" style="margin-bottom: -5px;" width="400" height="70"></p>
<p center>*Data Structure*</p>
<br>
This dataset is 2000 raws, 15 columns, and all values are Non-null. So I don't need to do null values preprocessing. Overall, I looked through the relations between all nemerical variables.
I found out the positive relation between Claims_Amount and  Claims_Count , weak positive relation between Premium and Price_Diff. 
<p align="center"><img src="{{ site.baseurl }}/images/89.png" width="100%" height="100%"></p>
In summsry, we can see the relations in the heatmap below.
* Strong positive relation between Claims_Amount and  Claims_Count. 
* Weak positive relation between Premium and  Price_Diff.  
* Weak positive relation between Claims_Amount  and  Plan_Count,  Claims_Count  and  Plan_Count.
<p align="center"><img src="{{ site.baseurl }}/images/90.png" style="margin-top: -20px; margin-bottom: -5px;" width=600 height=70></p>
<br>
Next, I looked through the relations between each feature and target. 
More claims customers have, more accepting an offer like below (left). (Claim_flag 1 : Claims they have, 0 : No claim.) Even though the premium decreased, there is no tendency to accept an offer. Even if the premium increases, they tend to accept an offer slightly more proportionally like below (right). (Price_Cat -1 : premium decrease, 0 : base premium, 1 : premium increase)
<p align="center"><img src="{{ site.baseurl }}/images/91.png" style="margin-top: -20px; margin-bottom: -15px;" 
 width="50%" height="50%"><img src="{{ site.baseurl }}/images/92.png" style="margin-top: -20px; margin-bottom: -15px;" width="60%" height="60%"></p>
More plans they have, more accepting an offer!
<p align="center"><img src="{{ site.baseurl }}/images/93.png" style="margin-top: -20px; margin-bottom: -20px;" width="100%" height="50%"></p>
Overall more age, more accepting an offer, but customers who have no plan are more likely to accept an offer after 2years. (Age_Cat 1 : less than 1 year, 2 : between 1 and 2 year, 3: more than 3years)
<p align="center"><img src="{{ site.baseurl }}/images/94.png" style="margin-top: -20px; margin-bottom: -10px;" width="50%" height="50%"><img src="{{ site.baseurl }}/images/95.png" style="margin-top: -20px; margin-bottom: -10px;" width="60%" height="60%"></p>
Next, I checked outliers of data. From the boxplots below, outliers of Premium and Purchase_Price needs to remove.
<p align="center"><img src="{{ site.baseurl }}/images/96.png" style="margin-top: -20px; margin-bottom: -20px;" width="50%" height="50%"><img src="{{ site.baseurl }}/images/97.png" style="margin-top: -20px; margin-bottom: -20px;" width="50%" height="50%"></p>
I also found out the wrong values in Age column. So, it needs to be modified. Age = Cover_Start_Date - Purchase_Date.


<br><br>
## Experiment - XGB, Logistic regression, LGBM, Random Forest, and MLP
I tried experimenting with various conditions and algorithms. This is the outcome of the trials.
As a final model, I chose the XGBoost. (red) Its accuracy is mostly the highest of all the models I tried, of course, but it also performs the best in terms of other metrics. However, we must exercise caution when evaluating the model's performance based just on accuracy. I believe that recall is a crucial component in this industry since it directly affects sales. The model's output comes first, and then marketing & pricing strategy, which in turn produces sales results. See the metrics like below.
<p align="center"><img src="{{ site.baseurl }}/images/98.png" style="margin-top: -10px; margin-bottom: -10px;" width="100%" height="100%"></p>


<br><br>
## Feture importance
To investigate the importance of features, I utilised the shap and the XGB plot_importance. Plan_flag, Plan_Count, Age_Cat, and Claims_Count are contributed in aspect of the shap. Price_Diff, Purchase_Price, Premium, and SP_CA are contributed more than other features based on XGB plot_importance.
<p align="center"><img src="{{ site.baseurl }}/images/99.png" style="margin-top: -20px; margin-bottom: -10px;" width="100%" height="40%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/100.png" style="margin-top: -10px;" width="85%" height="30%"></p>


<br>
## Experiment with the change in premium price
In this part, I experimented with the change in demand probability depending on the increase in premium from 1% to 10.5%. I also examined the change in recall according to the change in premium in the meantime, as recall is an important factor in this model, as I explained above. Surprisingly, the demand probability is higher when the price increases compared to the flat price. The ratio of increase is between 1% and 2.5%, and between 6% and 8.5%. The recall is also higher than the flat price, indicating its original price in some parts of the increased premium. Furthermore, we can observe that the higher the price increases, the greater the price elasticity. See the detailed charts below.
<p align="center"><img src="{{ site.baseurl }}/images/101.png" style="margin-top: -20px;" width="50%" height="50%"><img src="{{ site.baseurl }}/images/102.png" style="margin-top: -20px;" width="50%" height="50%"><img src="{{ site.baseurl }}/images/103.png" style="margin-top: -20px;" width="50%" height="50%"></p>


<br><br>
## Conclusion
As the price increases, it implies that there is a significant fluctuation in the quantity demanded in response to price changes. When the price elasticity approaches zero from negative values as the price increases, it indicates that the demand is relatively sensitive to price changes. When the price rises and the price elasticity approaches zero, it means that the demand decreases proportionally as the price increases. In conclusion, interpreting the graphs of accuracy, recall, and price elasticity, it can be seen that raising the price by 2% would be the most appropriate choice!




