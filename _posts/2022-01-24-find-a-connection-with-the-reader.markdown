---
layout: post
title:  The prediction churn of bank customers with Binomial Logistic Regression
date: 2022-01-24 15:01:35 +0300
image:  02.png
tags:   
---
We can identify which customers are likely to leave the bank through various ML & DL methods. It is crucial that banks conduct more thorough data analysis. The purpose of this project is to learn something new through careful data analysis such as __Binomial Logistic Regression.__

The goal of this project is finding out the accuracy of prediction depending on the condition in experiments. The project will be consist of 3 parts:
* EDA (Explatory Data Analysis)
* Method explanation
* The condition in experiment
* Result

## EDA - Data preprocessing 
![]({{ site.baseurl }}/images/11.png)
*Data Structure*

1. Data encoding
   
   Some variables are categorical, so I converted them to binary values. To apply cat- egorization issues, this is crucial (for instance, nation and
   gender). I consequently developed new features like country GE, country FR, country SP, and gender M and gender F.
   
2. Data scaling
   
   I used log transformation and standardisation to change the features’ scale.
   
3. Relation between variables
   ![]({{ site.baseurl }}/images/12.png)
   ![]({{ site.baseurl }}/images/13.png)
   
   As illustrated in Figure 1 below, I discovered that age, country GE, balance, and active member have better correlations with churn than other factors.
   I chose these four features as the main features as a result.
   ![]({{ site.baseurl }}/images/14.png)
   *Figure 1 heatmap : correlation between features*

4. Data Drop & replacement
   
   Since age contains certain outliers, as shown in (a) below, I tried to remove them and also did so for only churn=1. As can be seen in (b) below, there    are a lot of 0 values. I replaced these 0 values with a mean of balance.
   ![]({{ site.baseurl }}/images/15.png)

   
## The concept of algorithm

I tried separating train data and test data with 5-Fold cross validation to prevent the interruption from biased data and increase the accuracy of a model.
Below is a classification of regression as a special case.
![]({{ site.baseurl }}/images/16.png)
![]({{ site.baseurl }}/images/17.png)


## Conditions in experiment
I tried to apply each case below to the model.
![]({{ site.baseurl }}/images/18.png)

Say what? Throw her in the brig. Hey, you add a one and two zeros to that or we walk! You guys aren't Santa! You're not even robots. How dare you lie in front of Jesus? Ow, my spirit! Who's brave enough to fly into something we all keep calling a death sphere?

Hey, you add a one and two zeros to that or we walk! You won't have time for sleeping, soldier, not with all the bed making you'll be doing. It's okay, Bender. I like cooking too. Hey, what kinda party is this? There's no booze and only one hooker.

![]({{ site.baseurl }}/images/07.jpg)
*Minimalism*

Ummm…to eBay? But I know you in the future. I cleaned your poop. I'm just glad my fat, ugly mama isn't alive to see this day. My fellow Earthicans, as I have explained in my book 'Earth in the Balance'', and the much more popular ''Harry Potter and the Balance of Earth', we need to defend our planet against pollution. Also dark wizards.

Your best is an idiot! Fry, you can't just sit here in the dark listening to classical music. And remember, don't do anything that affects anything, unless it turns out you were supposed to, in which case, for the love of God, don't not do it!

You, a bobsleder!? That I'd like to see! I'm Santa Claus! There's no part of that sentence I didn't like! Noooooo! I can explain. It's very valuable.

I'm Santa Claus! Is the Space Pope reptilian!? Who's brave enough to fly into something we all keep calling a death sphere? I had more, but you go ahead.

It doesn't look so shiny to me. Kif might! You guys aren't Santa! You're not even robots. How dare you lie in front of Jesus? Oh, but you can. But you may have to metaphorically make a deal with the devil. And by "devil", I mean Robot Devil. And by "metaphorically", I mean get your coat.

Check it out, y'all. Everyone who was invited is here. Anyone who laughs is a communist! You're going to do his laundry? Michelle, I don't regret this, but I both rue and lament it.

Bender, we're trying our best. I daresay that Fry has discovered the smelliest object in the known universe! Oh, you're a dollar naughtier than most. Hi, I'm a naughty nurse, and I really need someone to talk to. $9.95 a minute.

You, a bobsleder!? That I'd like to see! No! The kind with looting and maybe starting a few fires! Good news, everyone! There's a report on TV with some very bad news! When I was first asked to make a film about my nephew, Hubert Farnsworth, I thought "Why should I?" Then later, Leela made the film. But if I did make it, you can bet there would have been more topless women on motorcycles. Roll film!

Eeeee! Now say "nuclear wessels"! Why did you bring us here? Yeah, and if you were the pope they'd be all, "Straighten your pope hat." And "Put on your good vestments." That's the ONLY thing about being a slave.
