---
layout: post
title:  The prediction of the gait phase at time t from the motion sensor data
date:   2018-07-05 15:01:35 +0300
image:  05.png
tags:   
---
The gait cycle is defined with reference to a given limb - left or right - and comprises stance phase (from the moment of heelstrike to the moment of toe-off) and swing phase (from toe-off to subsequent heelstrike); see Fig. 2. We further subdivide stance and swing into 6 phases, giving 12 phases in total so that the gait phase p lies in the interval  (0,11) . The phases are numbered consecutively from the start of stance to the end of swing. Phases 0-5 inclusive make up stance, with phase 0 representing the period immediately following heel-strike. Phases 6-11 make up swing, with phase 11 representing the period immediately preceding heelstrike. This project is to predict this scalar integer using the sensor data as input. That is, using any of the sensor data available up to and including (but not beyond) time t, predict the gait phase at time t.

The project will be consist of 5 parts:
* EDA
* Data Ingestion
* Modelling
* Validation
* Conclusion


## EDA

The dataset is consist of 3 types of the motion such as joggin, walking, and other activities. The dataset structure is like below. There are 6 files such as jog1, jog2, walk1, walk2, var1, and var2.csv.
<p align="center"><img src="{{ site.baseurl }}/images/35.png" width="100%" height="50%"></p>
I added time column as this data is the sequence data based on time assuming that each dataset collects and order by the time sequence.
<p align="center"><img src="{{ site.baseurl }}/images/36.png" width="100%" height="50%"></p>
Check the distribution of labels for each dataset!!! We can see each dataset is needed the augmentation about label. Only var2 file is mostly uniformly distributed!
<p align="center"><img src="{{ site.baseurl }}/images/37.png" width="100%" height="50%"></p>
Check the outliers for each dataset! Each dataset looks lots of outliers in each boxplot. So, I assumed that when removing outliers, other factors check needed!
<p align="center"><img src="{{ site.baseurl }}/images/38.png" width="100%" height="50%"></p>
Each feature is normalised or not?? I found out the fact like below.

jog1 - acc_x_left, acc_x_right outliers remove needed

jog2 - acc_x_left, acc_x_right outliers remove needed

walk1 - no

walk2 - no

var1 - no

var2 - acc_x_left, acc_x_right outliers remove needed
<p align="center"><img src="{{ site.baseurl }}/images/39.png" width="100%" height="100%"></p>
Check the skewness for each dataset! if the skewness > 1,then log transform needed!
<p align="center"><img src="{{ site.baseurl }}/images/40.png" width="100%" height="50%"></p>
Check the correlation! When I try to remove outliers, I could refer this correlation.

jog1 - gyr_y_right negative correlation (-0.46), gyr_z_left positive correlation (0.72)

jog2 - gyr_y_right negative correlation (-0.35), gyr_z_left positive correlation (0.72)

walk1 - gyr_y_right negative correlation (-0.59), gyr_z_left positive correlation (0.65)

walk2 - acc_y_right, acc_z_left negative correlation (-0.32, -0.3), gyr_z_left positive correlation (0.57)

var1 - gyr_y_right negative correlation (-0.25), gyr_z_left positive correlation (0.56)

var2 - acc_z_left negative correlation (-0.16), gyr_z_left positive correlation (0.64)
<p align="center"><img src="{{ site.baseurl }}/images/41.png" width="100%" height="50%"></p>


## Data Ingestion
I divided the train/test dataset for the input dataset for the model.
I could basically get 3files (jog,walk,var) and preprocessed depending on method parameter. The method parametesr are like below.
1. Outliers removing
2. Log transformation
3. PCA
4. Feature selection
Of course, features standardisation is essential and the one-hot encoding for labels is in the model train step. To make even for the distirbution in labels, I utilised the SMOTE method. After augmentation for each dataset (jog,walk,var) is like below.
<p align="center"><img src="{{ site.baseurl }}/images/42.png" width="100%" height="50%"></p>


### Why not indeed!

Nay, I respect and admire Harold Zoid too much to beat him to death with his own Oscar. I don't 'need' to drink. I can quit anytime I want! Soothe us with sweet lies. Bender?! You stole the atom. You don't know how to do any of those.

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
