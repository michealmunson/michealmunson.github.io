---
layout: post
title:  "Sample size - when is enough, enough? A lesson by a lunatic"
categories: Statistics
---
*"If you have cooked a large pan of soup, you do not need to eat it all to find out if it needs more seasoning. you can just taste a spoonful, provided you have given it a good stir"*

*- George Gallup*

# Introduction

Any experimentalist, and especially biologists and clinicians, won't get far in designing their experiment until they start asking themselves: how many samples should I collect? The story below really revolves around this question.

This story centers around a bag that holds a million numbers and a poor soul who's been blackmailed to save a cancer patient's life. That poor soul, the protagonist of the story, is tasked to find out what the average of those million numbers really is.

The goal of this post is both to learn statistics and to simply enjoy a story. In this post, you'll learn what sample size is "good enough" when performing a hypothesis test.

This post takes the following format
1. The setup
2. The story

The setup is meant to be read first and gives you the information you'll know as *the reader* but not as *the protagonist*. In posts like this, the reader knows all and the protagonist has to figure out what it is the reader knows. You'll play both the reader and the protagonist. 

Enjoy!

# The setup

Any time you see a box like
{% highlight r %}
this
{% endhighlight %}
know that its a simulation that the reader is aware of, but the protagonist isn't. It's snippet of R code to help us model what probabilistic phenomenon is actually happening whenever the protagonist acts.

Okay, as said above, and said again here - the protagonist's goal is to find out what the mean of the million numbers is in the bag. As the reader, you should know that the mean and hence the answer is **70**. And to take it up a notch, the standard deviation of the numbers is 10, and here's what the million numbers actually look like.

{% highlight r %}
df <- as.data.frame(rnorm(1e6, 70, 10)) # generate the million random numbers
colnames(df)[1] <- "x"
ggplot(data = df) +
  geom_histogram(mapping = aes(x),
                 alpha = 0.3,
                 fill = 'green',
                 color = 'black',
                 linewidth = 0.1,
                 binwidth = 2
                 ) +
  geom_freqpoly(mapping = aes(x),
                binwidth = 2
                ) +
  ggtitle('Histogram of the million numbers') +
  ylab('Frequency of numbers') +
  xlab('The numbers') +
  theme_light() # plot the million random numbers
{% endhighlight %}

<p align="center">
  <img src="/million-numbers.jpeg" alt="million pic" style="height: 500px; width: 700px;"/>
</p>

# The story

You're on your daily stroll when some mad physician gets in your way. He puts a gun to your head and tells you that he has this bag that holds a *million* numbers. He then tells you that the average of these million numbers is the proper dose for the chemotherapy he needs to deliver to his patient. You tell the loony doctor to find it himself. He retorts simply with *do it, or else*. 

Damn, you think to yourself. This guys a lunatic... but you don't want this patient to get hurt - whether you were coerced into it or not. So you get to thinking. One way to get your answer is to actually get all million of those numbers and calculate the average. But you ain't got time for that. And who does anyway? Instead you say, I'll just grab 10 numbers and calculate the average. That should be a pretty good estimate.

You put your hand in the bag, grab 10 numbers, and calculate their average.

{% highlight r %}
x <- round(rnorm(10, 70, 10), 0) # represents you grabbing 10 numbers
mean(x) # represents you calculating the average
{% endhighlight %}
{% highlight r %}
 [1] 71 68 73 66 73 71 71 74 69 66 # what those 10 numbers were
 [2] 70.2 # what the average was
{% endhighlight %}

Ok - the average of these numbers is 70.2. You're not confident in telling the doc this - *of course*!  You have no idea how good of an estimate that actually is. All you know is that there's a million numbers in that bag, you grabbed 10, and the average was 70.2. What if you just got unlucky and grabbed some numbers that didn't represent the million in the bag at all? You decide to do it again. Hand in the bag. Grab the numbers.

{% highlight r %}
x <- round(rnorm(10, 70, 10), 0) # grab another 10
mean(x) # calculate the average
{% endhighlight %}
{% highlight r %}
 [1] 68 68 72 50 63 73 64 64 63 67 # the 10 numbers
 [2] 65.2 # the new average
{% endhighlight %}

Alright - the average this time was 65.2. They're *different* and, to be more precise, different by 5 units. You're unsure if this difference is substantial so you ask Mr. Loony Doc if being off by 5 units is a lot. He throws the question back at you, and he frames *you* as the patient. You decide its a lot. And on second thought, still aren't even sure what the real average is. What do you do?

You break down the problem and draft up the best solution. *My goal is to give the doctor the average of those million numbers. And, if possible, I'd like to do it in the least cumbersome way. The most cumbersome way involves collecting all million of those numbers. The least cumbersome way is just collecting 1 number. How many numbers do I need to collect to feel comfortable? When is enough, enough?*

To find the answer to this question, we need to figure out some other things. Firstly, how do the numbers in that bag vary? In other words, are the numbers spread along a massive range (like 1 all the way to a million?), or are they all centered around some other number (like 100 or something?). 

You ask the doc if he has any idea. He tells you has no idea and you decide the man is useless. You look at the 20 numbers you first pulled and plot them on a histogram.

{% highlight r %}
71 68 73 66 73 71 71 74 69 66 68 68 72 50 63 73 64 64 63 67 # looking at the 20 numbers
ggplot(data = df) +
  geom_histogram(mapping = aes(x),
                 alpha = 0.3,
                 fill = 'green',
                 color = 'black',
                 linewidth = 0.1,
                 binwidth = 2
                 ) +
  ggtitle('Histogram of the 20 numbers you pulled') +
  ylab('Frequency of numbers') +
  xlab('The numbers') +
  theme_light() # plot the 20 numbers
{% endhighlight %}

<p align="center">
  <img src="/first-20.jpeg" alt="first 20" style="height: 500px; width: 700px;"/>
</p>

Alright you tell yourself. Nearly half (8 of the 20) numbers are between 70 and 75. And the next largest bin is between 65 and 70 (6 out of the 20 numbers). There are a couple that are between 50 and 65. So 70% of the numbers are between 65 and 70. 

Upon this realization, you know one thing. Your hypothesis that the million numbers in the bag is simply just the sequence of numbers from 1 to a million is out the door. You can see that you've pulled 71 three times in the same grab, so there are repeats in that bag. If that wasn't enough, of the 20 you pulled, all 20 only represented the first 0.01% of all numbers between 1 and a million. Very unlikely.
















