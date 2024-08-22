---
layout: post
title:  Customer Churn Analysis using Tableau
date:   2024-08-01 15:01:35 +0300
image:  06.webp
tags:   
---
<!--https://heung-bae-lee.github.io/2020/02/01/NLP_05/-->
In this project, I analysed Telco customer churn data (you can download this data here) using Python and created a dashboard in Tableau. I focused more on creating a dashboard story than on experimenting with ML algorithms.


<br>
This dashboard story consists of 5 parts as outlined below:
* Overview
* Major Factor Analysis
* Minor Factor Analysis
* Analysis between Factors
* Churn Rate Simulation

<br>
I will briefly explain each part:
## In the overview dashboard page, 
you can see the total number of customers, the number of churned customers, and the churn rate. I also highlighted which features are impactful for the target variable, churn. When you click on each feature, the corresponding feature is highlighted in other charts.
<br>
## In the major factor analysis page,
I focused on the top 5 features influencing the target variable. For example, customers with a tenure of less than 30 months are more likely to churn. In terms of charges, customers who pay between £60 and £110 monthly and have a total payment of under £1000 are more likely to churn. Over 90% of churned customers have phone service. Regarding contract type, there are more churned customers with month-to-month contracts compared to other types.
<br>
## In the minor factor analysis page, 
you can see the proportion of churned customers depending on feature attributes.
<br>
## In the fourth page, 
as we can see from the heatmap, there are clear relationships between the factors. For example, the older the customer (senior citizen) and the more likely they are to have a partner, the higher the churn rate. Internet service-related features such as Online Security, Online Backup, Device Protection, Tech Support, Streaming TV, and Streaming Movies have strong correlations with each other. We can also see that customers who don't use the internet service features mentioned above are more likely to churn.
<br>
## In the final page, 
I experimented with the change in churn rate by altering major factors such as tenure, monthly charge, and contract type. You can change the tenure with a slide bar and the monthly charge using a select box. The more tenure months I add, the lower the churn rate becomes. The more monthly discount customers have, the lower the churn rate. When I change the contract type from month-to-month to a yearly contract, the churn rate decreases from 43.9% to 26.6%.

## In conclusion, I suggest targeting customers with month-to-month contracts and offering them a monthly discount to switch to a yearly contract. This would help retain them longer and ideally reduce the churn rate to 0%.
Take a look at the story dashboard using Tableau!

<br><br>
<div class='tableauPlaceholder' id='viz1724312417023' style='position: relative'><noscript><a href='#'><img alt='Churn Customer Analysis ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ch&#47;Churn_Customer_Analysis_17241053180540&#47;Story1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='Churn_Customer_Analysis_17241053180540&#47;Story1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Ch&#47;Churn_Customer_Analysis_17241053180540&#47;Story1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-GB' /></object></div>                <script type='text/javascript'>                    var divElement = document.getElementById('viz1724312417023');                    var vizElement = divElement.getElementsByTagName('object')[0];                    vizElement.style.width='1016px';vizElement.style.height='991px';                    var scriptElement = document.createElement('script');                    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';                    vizElement.parentNode.insertBefore(scriptElement, vizElement);                </script>


