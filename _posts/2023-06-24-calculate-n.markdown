---
layout: post
title:  "Sample size - when is enough, enough? A lesson by a lunatic"
categories: Statistics
---
<script>
MathJax = {
  tex: {
    inlineMath: [['$', '$'], ['\\(', '\\)']]
  }
};
</script>
<script id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js">
</script>

*"If you have cooked a large pan of soup, you do not need to eat it all to find out if it needs more seasoning. you can just taste a spoonful, provided you have given it a good stir"*

*- George Gallup*

# Introduction

Any experimentalist, especially biologists and clinicians, won't get far in designing their experiment until they start asking themselves: how many samples should I collect? The story below really revolves around this question.

This story centers around a bag that holds a million numbers and a poor soul who's been blackmailed to save a cancer patient's life. That poor soul, the protagonist of the story, is tasked to find out what the average of those million numbers really is.

The goal of this post is both to learn statistics and to simply enjoy a story. In this post, you'll learn what sample size is "good enough" when performing a hypothesis test.

This post takes the following format
1. The setup
2. The story

The setup is meant to be read first and gives you the information you'll know as *the reader* but not as *the protagonist*. In posts like this, the reader knows all and the protagonist has to figure out what it is the reader knows. You'll play both the reader and the protagonist. 

Enjoy!

# The setup

**What highlights mean**

Any time you see a box like
{% highlight r %}
this
{% endhighlight %}
know that its a simulation that the reader is aware of, but the protagonist isn't. It's snippet of R code to help us model what probabilistic phenomenon is actually happening whenever the protagonist acts.

**Mathematical theory**

The statistical principle shown here is how someone can determine what sample size they must collect to accurately (whatever "accurately" means to the experimentalist) estimate the average of whatever it is you're measuring.

The principal depends upon the central limit theorem. This theorem states that if you take any number (say *n*) data points and calculate their mean, that is you take *n* $x_i$'s and calculate 

$$\frac{\sum_{i=0}^n x_i}{n} = \bar{x}$$

and "normalize" it, meaning you center the average at 0 and transform its variation to 1 by subtracting the actual mean, $\mu$, from $\bar{x}$, and dividing by the sample's standard deviation, $\sigma_{\bar{x}}$

$$\frac{\bar{x} - \mu}{\sigma_{\bar{x}}}$$

the distribution of that normalized average converges (essentially becomes the same) to a standard normal distribution as the number of data points *n* gets larger and larger! The standard normal distribution is shown below (don't be intimidated by the integral! It's just simply a function. You know everything in the equation, except for *x*. Thats what you're going to input)!

$$\frac{1}{\sqrt{2\pi}}\int_{-\infty}^x e^{-x^{2}/2}dx$$

So why is this important in figuring out what sample size one ought to use? 

Well, we can now map our $\bar{x}$ value to some number between 0 and 1 that we are calling "probability". That standard normal distribution returns a number between 0 and 1 for any value inputted into it. For example, plug $x = 0$ into the standard normal function given above, and you'll get 0.5. Plug any *x* and you'll get some number between 0 and 1.

That *x* value has a special name and is called a *z-score*, and because that function above is one-to-one, every *z-score* has its own number (i.e., probability) between 0 and 1.

Thus, if we want a "probability" of being *p*% right, "right" being that our sample mean is the same as the population mean, we would take the *z-score* ($z$) that returned *p* in the above function and multiply it by the sample standard deviation $\sigma_\bar{x}$

$$z_{p}*\sigma_\bar{x}$$

This should make sense! Recall that the normalized $\bar{x}$ converges to the standard normal distribution. Thus, if we took the standard normal distribution and multiplied it (because we had to divide $\bar{x}$) by $\sigma_\bar{x}$, we'd get the numerator $\bar{x} - \mu$. And what is $\bar{x} - \mu$? The difference we can tolerate between our estimated average and the actual average! Lets call that difference $D$.

So, to calculate the sample size, we need to do the following:

1. Determine what probability (or confidence), *p*, we want to have when estimating the average
2. Determine the difference between our estimate and the actual population, $D$, we can allow
3. Solve for *n* using $z_{p}*\sigma_\bar{x}$

To solve for *n*, just set $z_{p}*\sigma_\bar{x}$ equal to the difference you'll allow between your estimate and the true mean.

$$z_{p}*\sigma_\bar{x} = \frac{z_{p}*\sigma}{\sqrt{n}} = D$$

$$\sqrt{n} = \frac{z_{p}*\sigma}{D}$$

$$n = (\frac{z_{p}*\sigma}{D})^2$$

and voila! You can now calculate *n* for any probability *p* and difference $D$ your heart desires!


**What the true distribution is in the story**

Okay, as said above, and said again here - the protagonist's goal is to find out what the mean of the million numbers is in the bag. As the reader, you should know that the mean and hence the answer is **70**. And to take it up a notch, the standard deviation of the numbers is 10, and here's what the million numbers actually look like.

{% highlight python %}
def clean_hist():
  ...

# generate samples
mu, sigma, n = 70, 10, int(1e6)
x = np.random.normal(mu, sigma, n) 

# plot the samples on a histogram
fig, axs = plt.subplots(figsize = (10,10),
                        tight_layout = True)

n, bins, patches = plt.hist(x,
                            bins = 30,
                            density = True,
                            alpha = 0.5)

y = ((1 / (np.sqrt(2 * np.pi) * sigma)) *
     np.exp(-0.5 * (1 / sigma * (bins - mu))**2))

plt.plot(bins,
         y,
         '--',
         color = 'black')

clean_hist()

plt.show()
{% endhighlight %}

<p align="center">
  <img src="/million-numbers.png" alt="million pic" style="height: 500px; width: 500px;"/>
</p>

# The story

You're on your daily stroll when some mad physician gets in your way. He puts a gun to your head and tells you that he has this bag that holds a *million* numbers. He then tells you that the average of these million numbers is the proper dose for the chemotherapy he needs to deliver to his patient. You tell the loony doctor to find it himself. He retorts simply with *do it, or else*. 

Damn, you think to yourself. This guys a lunatic... but you don't want this patient to get hurt - whether you were coerced into it or not. So you get to thinking. One way to get your answer is to actually get all million of those numbers and calculate the average. But you ain't got time for that. And who does anyway? Instead you say, I'll just grab 10 numbers and calculate the average. That should be a pretty good estimate.

You put your hand in the bag, grab 10 numbers, and calculate their average.

{% highlight python %}
# represents you grabbing the 10 samples
mu, sigma, n = 70, 10, 10
x = np.random.normal(mu, sigma, n) 

# represents you calculating the mean
np.mean(x)
{% endhighlight %}
{% highlight python %}
 [1] 71 68 73 66 73 71 71 74 69 66 # what those 10 numbers were
 [2] 70.2 # what the average was
{% endhighlight %}

Ok - the average of these numbers is 70.2. You're not confident in telling the doc this - *of course*!  You have no idea how good of an estimate that actually is. All you know is that there's a million numbers in that bag, you grabbed 10, and the average was 70.2. What if you just got unlucky and grabbed some numbers that didn't represent the million in the bag at all? You decide to do it again. Hand in the bag. Grab the numbers.

{% highlight python %}
# represents you grabbing the 10 samples
mu, sigma, n = 70, 10, 10
x = np.random.normal(mu, sigma, n) 

# represents you calculating the mean
np.mean(x)
{% endhighlight %}
{% highlight python %}
 [1] 68 68 72 50 63 73 64 64 63 67 # the 10 numbers
 [2] 65.2 # the new average
{% endhighlight %}

Alright - the average this time was 65.2. They're *different* and, to be more precise, different by 5 units. You're unsure if this difference is substantial so you ask Mr. Loony Doc if being off by 5 units is a lot. He throws the question back at you, and he frames *you* as the patient. You decide its a lot. And on second thought, still aren't even sure what the real average is. What do you do?

You break down the problem and draft up the best solution. *My goal is to give the doctor the average of those million numbers. And, if possible, I'd like to do it in the least cumbersome way. The most cumbersome way involves collecting all million of those numbers. The least cumbersome way is just collecting 1 number. How many numbers do I need to collect to feel comfortable? When is enough, enough?*

To find the answer to this question, we need to figure out some other things. Firstly, how do the numbers in that bag vary? In other words, are the numbers spread along a massive range (like 1 all the way to a million?), or are they all centered around some other number (like 100 or something?). 

You ask the doc if he has any idea. He tells you has no idea and you decide the man is useless. You look at the 20 numbers you first pulled and plot them on a histogram.

{% highlight python %}
def clean_hist():
  ...

# Represents you looking at the 20 numbers
x = [71,68,73,66,73,71,71,74,69,66,68,68,72,50,63,73,64,64,63,67]

# Plotting the 20 numbers on a histogram
fig, axs = plt.subplots(figsize = (10,10),
                        tight_layout = True)

n, bins, patches = plt.hist(x,
                            alpha = 0.5)

clean_hist()

plt.show()
{% endhighlight %}

<p align="center">
  <img src="/20-numbers.png" alt="first 20" style="height: 500px; width: 500px;"/>
</p>

Alright you tell yourself. Nearly half (8 of the 20) numbers are between 70 and 75. And the next largest bin is between 65 and 70 (6 out of the 20 numbers). There are a couple that are between 50 and 65. So 70% of the numbers are between 65 and 70. 

Upon this realization, you know one thing. Your hypothesis that the million numbers in the bag is simply just the sequence of numbers from 1 to a million is out the door. You can see that you've pulled 71 three times in the same grab, so there are repeats in that bag. If that wasn't enough, of the 20 you pulled, all 20 only represented the first 0.01% of all numbers between 1 and a million. Very unlikely.

Okay - if you we're to bet anything on what those million numbers looked like, you'd probably say that most of them are under a hundred (*just based off the information given*). You aren't entirely sure, but your 20 samples say so. 

You have 2 options now - the *you-did-your-best-so-get-home-as-soon-as-possible* option and the *i-have-all-the-time-in-the-world* option. The first option means you get to calculating right away. The second option means you collect more data to get an even more accurate depiction of how those million numbers vary. The truth of the matter is - you don't even have all day. You have a date scheduled at 6 o'clock and you're not the type to make bad first impressions. Twenty samples is good enough for you.*


* *NOTE: This is just the samples you took to have an understanding of the population of numbers you're drawing from. This is **not** the samples you're going to take to determine the average. In a practical setting, you ought to have some expert who knows how the numbers should vary. If you don't, like in this setting, then taking some samples to get an understanding of the variance will do.

You estimate the population's standard deviation by calculating the sample standard deviation

$$\sqrt{\frac{ \sum_{i=0}^{19}(x_i - \bar{x})^2}{n-1}}$$

{%highlight python%}
x = [71,68,73,66,73,71,71,74,69,66,68,68,72,50,63,73,64,64,63,67]
s1 = np.std(x) # Represents you calculating the standard deviation
{%endhighlight%}

The standard deviation was **5.33**, and thats what you're gonna run with. You now need to figure out the following 2 things:

1. With what confidence (or probability) do you want your estimate to be correct?
2. With that probability of being right, what error in your estimate are you willing to accept? In other words, whats the largest difference between your estimate and the true mean won't hurt the patient?

You look at the doc. He stares back angry and expecting an answer. You decide just to get as close to actual estimate as possible.

Okay, so assuming the standard deviation of those numbers in the bag is 5.33, you decide that you want to be within 1 unit of the actual mean 99% of the time. In other words, if you grabbed this sample size 100 times, 99 out of the 100 times you'd be within 1 unit of the actual mean.

You use the following equation

$$n = (\frac{z_{p}*\sigma}{D})^2 = (\frac{z_{0.995}*5.33}{1})^2 = (\frac{2.576*5.33}{1})^2 \approx 189$$

189 numbers need to be collected if you want to estimate the true mean within 1 unit. You go ahead and do that and calculate the mean

{%highlight python%}
x = (np.random.randn(int(1e6)))*10 + 70

# represents you grabbing the 189 numbers out of the bag
sample = []
for i in range(189):
    sample += [x[i]]

# represents you calculating their average
np.mean(sample)
{%endhighlight%}

The average of these 189 numbers was **69.85**. You do it once more for good measure and the average this time was **70.07**. Your feeling good about this and just tell the doc your first calculation, 69.85 units. You tell the insaniac that thats the dose he should give to his patient. The doctor looks at you with a skeptical brow. He doesn't seem to trust you. You throw your hands up and retort with thats all you got. Anxious to prescribe this patient the medication she needs, the doctor prescribes her 69.85 units of the medication to take twice a week.

A few months goes by. The date went well and you actually moved in with your starry-eyed lover. On occasion you thought of the psychotic dilemma that physician coerced you in. You've told no one yet. You get an anonymous phone call and hear that familiar voice. He tells you that the patient would've only been okay if you we're within 1 unit of the true mean in the bag like you had said. Your heart races. A few beats later, he tides the nerves by letting you know she's just fine.

You were correct this time. The question is, were you really going be within 1 unit of the true mean 99% of the time... or did you just get lucky? You may never know.

At this point the story is over. However, simply out of interest, lets calculate what percent of the time you would've been within 1 unit of the mean with the sample size the character drew, 189. To do this, just simply re-arrange the above equation to calculate the sample size, and we want to find the z-score given a sample size of 189, a standard deviation of 10, and  

$$ \frac{\sqrt{189}}{10} = z_{p}  \approx 1.37$$

Then plug $z_{p}$ into the probability function of the standard normal shown above

$$\frac{1}{\sqrt{2\pi}}\int_{-\infty}^{1.37} e^{-1.37^{2}/2}dx \approx 0.9147$$

So, in this case, drawing samples of 189 from that bag would've yielded a sample mean within the true mean of the bag 91% of the time - not 99% like the character had thought. This is simply a function of the estimate for the standard deviation of the population. The actual standard deviation was 10 but the character had assumed 5.33. If the character had collected a larger sample size, or the doctor had some information to share (for example whats the lowest and highest number in the bag), better estimates for the standard deviation could have been made. However, this often emulates what happens practically for anyone needing to collect samples to make an estimate about a population. Its all just estimation. It's a hell of a lot better than blindly guessing, but like blind guessing, it still has error involved.

This lunatic might have costed you some time - but hopefully taught an important lesson in sample collection and experiment design.










