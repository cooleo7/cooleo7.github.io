---
layout: post
title:  Text analysis with Logistic regression, VADER, and CNN
date:   2018-06-25 15:01:35 +0300
image:  06.webp
tags:   
---
<!--https://heung-bae-lee.github.io/2020/02/01/NLP_05/-->
This is a text classification using movie rating data from the Internet Movie Database (IMDB). It will predict sentiment (positive or negative) after checking the reviews. 
The project will be consist of 5 parts:
* EDA
* Data Preprocessing
* Modelling #1 (Logistic regression)
* Modelling #2 (VADER)
* Modelling #3 (CNN)
* Conclusion


## EDA
Here is dataset structure like below.
<p align="center"><img src="{{ site.baseurl }}/images/56.png" width="80%" height="50%"></p>
At first, I checked the distribution of the length of review! The length of the reviews are mostly 100 and 6000. I also checked the outliers of the length in reviews. See the image the below.
<p align="center"><img src="{{ site.baseurl }}/images/51.png" width="100%" height="70%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/52.png" width="100%" height="70%"></p>
We can check the distribution of the frequent of words in reviews using wordcloud. Refer to the image below.
<p align="center"><img src="{{ site.baseurl }}/images/53.png" width="100%" height="800%"></p>
We can see "br" is quite frequent in review. We can guess it includs html, so I will remove the html during data preprocessing.
I also have to check the distribution of labels so that we can get the result of training without a bias.
The distribution of sentiment is equally divided so that '1'(positive) is 12500 and '0'(negatibe) is 12500.
Refer to the image below.
<p align="center"><img src="{{ site.baseurl }}/images/54.png" width="80%" height="60%"></p>
In the next step, I want to see the frequency of word counts, as it needs to be same length in input matrix for the training.
We can see the data mostly has between 200 and 1000 words in average like below.
<p align="center"><img src="{{ site.baseurl }}/images/55.png" width="100%" height="50%"></p>
To cleanse the data, I checked the special letters, numbers, and the proportion of uppercase.
<p align="center"><img src="{{ site.baseurl }}/images/57.png" width="70%" height="50%"></p>


## Data Preprocessing
1. Remove HTML tag
2. Replace special letters (non-english) to space(" ")
3. Replace uppercase to lowercase and after that split (word split )
4. remove stopwords

From the preprocessing, we can compare the data before preprocessing and after. Refer to below the images.
<p align="center"><img src="{{ site.baseurl }}/images/58.png" width="100%" height="50%"></p>
*before preprocessing*
<p align="center"><img src="{{ site.baseurl }}/images/59.png" width="100%" height="50%"></p>
*after preprocessing*

To train the data, we have to convert the text to a matrix. (vectorisation) Currently, each data has a different length, but the length must be unified to be able to apply it to future models. So a specific length is set as the maximum length and for longer data, it has to be truncated the latter part and, in case of short data, it has to be padded with a 0 value. For padding processing, I used the pad_sequences function. When using this function, you can specify as arguments the data to apply padding to, the maximum length value, and whether to put the 0 value before or after the data. Also, note that words are counted starting from the last word. Here, the maximum length was set to 174, which was calculated when statistics on the number of words were calculated during the data analysis process. This is the median value. Usually, the median is used instead of the average, because the average is sensitive to outliers. After vectoriation, data structure is like below.
<p align="center"><img src="{{ site.baseurl }}/images/60.png" width="100%" height="50%"></p>
One important point when preprocessing evaluation data is that when creating an index vector through Tokenizer, the Tokenizer object previously applied to the learning data must be used. If you create a new one, the index of each word for the training data and evaluation data will be different.
This is because it cannot be applied properly to the model. If a word that is not included in the train data exists in the test data, the probability must be set to 0.


## Modelling #1 (Logistic regression)
To apply logistic regression algoritm, I vectorised the input data with TF-IDF method. What is TF-IDF?
It is the product of TF and IDF. It gives a weight to each term in a document, emphasizing terms that are important to that document but not common across the entire corpus.
TF-IDF(t,d,D)=TF(t,d)×IDF(t,D)
<p align="center"><img src="{{ site.baseurl }}/images/61.png" width="80%" height="50%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/62.png" width="80%" height="50%"></p>
In summary, TF-IDF is used to quantify the importance of a term within a specific document in the context of a larger corpus.
From logistic regression with TF-IDF, I got the accuracy of the model as __85%.__


## Modelling #2 (VADER)
In simple, I just applied the VADER. VADER is the rule-based lexicon for social media sentiment analysis. Therefore, we can use the function from the VADER. For example, I can get the result like below for the first review text. 'neg' is negative, 'neu' is neutral, and 'pos' is positive.
{'neg': 0.183, 'neu': 0.63, 'pos': 0.187, 'compound': -0.5583}
Interestingly, I got the lower accuracy from VADER compared to logistic regression.
{accuracy: 0.6762, precision: 0.6287, recall: 0.8606}


## Modelling #3 (CNN)
In this model, I vectorised the input data with a GLoVE embedding method. I experimented GLoVE embedding size 50 and 100 each. I utilised the CNN which has 100 filters using the window size 2,3,4, and 5. Here is the plot of the accuracy and loss having a GLoVE embedding size 50 and 100 each.
<p align="center"><img src="{{ site.baseurl }}/images/61.png" width="100%" height="50%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/62.png" width="100%" height="50%"></p>

