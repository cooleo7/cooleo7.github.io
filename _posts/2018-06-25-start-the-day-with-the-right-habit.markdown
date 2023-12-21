---
layout: post
title:  Text analysis with Logistic regression and RNN
date:   2018-06-25 15:01:35 +0300
image:  06.jpeg
tags:   
---
This is a text classification using movie rating data from the Internet Movie Database (IMDB). It will predict sentiment (positive or negative) after checking the reviews. 
The project will be consist of 5 parts:
* EDA
* Data Preprocessing
* Modelling #1 (Logistic regression)
* Modelling #2 (RNN)
* Conclusion


## EDA
Here is dataset structure like below.
<p align="center"><img src="{{ site.baseurl }}/images/56.png" width="80%" height="50%"></p>
At first, I checked the distribution of the length of review! The length of the reviews are mostly 100 and 6000. I also checked the outliers of the length in reviews. See the image the below.
<p align="center"><img src="{{ site.baseurl }}/images/51.png" width="100%" height="70%"></p>
<p align="center"><img src="{{ site.baseurl }}/images/52.png" width="100%" height="70%"></p>
We can check the distribution of the frequent of words in reviews using wordcloud. Refer to the image below.
<p align="center"><img src="{{ site.baseurl }}/images/53.png" width="100%" height="60%"></p>
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
To train the data, we have to convert the text to a matrix. (vectorisation)
Currently, each data has a different length, but the length must be unified to be able to apply it to future models. So a specific length is set as the maximum length and for longer data, it has to be truncated the latter part and, in case of short data, it has to be padded with a 0 value.
For padding processing, I used the pad_sequences function. When using this function, you can specify as arguments the data to apply padding to, the maximum length value, and whether to put the 0 value before or after the data.
Also, note that words are counted starting from the last word. Here, the maximum length was set to 174, which was calculated when statistics on the number of words were calculated during the data analysis process. This is the median value. Usually, the median is used instead of the average, because the average is sensitive to outliers. After vectoriation, data structure is like below.
<p align="center"><img src="{{ site.baseurl }}/images/60.png" width="100%" height="50%"></p>


* Shinier than yours, meatbag.
* This is the worst part. The calm before the battle.
* Ooh, name it after me!

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
