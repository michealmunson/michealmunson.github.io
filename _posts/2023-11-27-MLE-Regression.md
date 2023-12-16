---
layout: post
title:  "Maximum Likelihood Estimation - Linear Regression"
categories: Data
---
<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>

## Introduction

Linear regression is a method to produce a line (e.g., y = mx + b) given a set of randomly or non-randomly generated numbers. People use this line to predict outputs for new observations.

We'll first discuss some theory of parameterized linear regression. In other words, there are multiple methods used to produce said line. We'll only be discussing one method below--namely maximum likelihood estimation. Then we'll see how to actually make that line with python.

## Theory

Suppose you have some noisy observations, **Y**, associated with some independent, non-random variable **X**, and there are **N** total observations of **X** and **Y**. 

Because each individual $$y_n$$ has some randomness to it, you may find that for every $$x_n$$ with the same value, you have a different $$y_n$$. For example, for two $$(x_n,y_n)$$ observations, you observe $$(5.0,5.1)$$ and $$(5.0,6.1)$$. Hence, the model you create should account for said randomness. To do this, we should use probability distributions like the normal, poisson, binomial, etc

We'll assume that the noise in our observations is normally distributed with central location parameter, often called $$\mu$$, as 0 and spread parameter, often called $$\sigma^2$$, as $$\sigma^2$$ and that $$Y$$ is some linear function of $$X$$. $$\theta$$ is the slope and is the parameter we're trying to estimate. So

$$
Y = X\theta + \epsilon, \epsilon \sim N(0, \sigma^2)
$$

Because $$Y$$ is a function of a constant added to a normally distributed random variable

$$
Y \sim N(X\theta, \sigma^2)
$$

 We proceed with a negative log likelihood function, $$f(\theta)$$, where $$\theta$$ is one parameter in our model. Your goal is to find the parameters, $$\theta$$, that maximizes $$f(\theta)$$. The negative log likelihood function in this case is the negative log of the normal distribution with the central location parameter as $$X_T\theta$$ and spread parameter as $$\sigma$$. The likelihood function itself is the product of the individual $$y_i$$'s conditioned on their respective $$x_i$$'s. We take the product because we assume each $$x_i$$ is independent of one another.

$$ f(\theta) = -log (p(Y | X, \theta)), Y \sim N(X^T\theta, \sigma^2)$$

$$ -log (p(Y | X, \theta)) = -log (\prod_{n=1}^{N}p(y_n | x_n\theta)) $$

$$X^T$$ is an $$NxD$$ matrix where each observation has $$D$$ features and there are $$N$$ observations. $$\theta$$ is a $$Dx1$$ matrix and hence their matrix multiple outputIn s an $$Nx1$$ vector, namely $$Y$$.

The idea here is that we're trying to find the $$\theta$$ that will maximize the likelihood. Hence, we're trying to find $$\theta_e$$

$$
\DeclareMathOperator*{\argmax}{arg\,max}
\theta_e = {arg\,max}_\theta p(Y|X,\theta)
$$

After some algebraic manipulation, then differentiating the negative log likelihood with respect to $$\theta$$ and finding the optimum, we arrive at a closed-form solution for $$\theta_e$$.

$$
\theta_e = (X^TX)^{-1}X^TY
$$

Very similar to the above logic, you can also produce a polynomial function ($$ y = \sum_{i=0}^{m}\theta^{T}x^i $$) using maximum likelihood estimation. This works because the polynomial function is linear in the parameters were trying to optimize for. It doesn't matter that inputs, $$x_i$$, are non-linear.
## Application

Try to fit a regression model via mle to an exponentially increasing curve with a lot of data points. Remember that you can fit a (n-1)th polynomial to n data points.

In this case, I gathered some data concerning the temperatures and humidity at some timepoint in San Francisco.

```python
import matplotlib.pyplot as plt

temperature = [
    55, 55, 54, 55, 54, 54, 54, 54, 54, 54, 55, 54, 53, 54, 53, 
    54, 55, 55, 56, 59, 60, 60, 61, 63, 62, 60, 58, 57, 57, 57
]
humidity = [
    80, 80, 86, 83, 86, 83, 83, 83, 86, 80, 80, 83, 83, 77, 77, 
    77, 77, 80, 69, 64, 64, 64, 60, 58, 60, 62, 65, 62, 67, 72
]

fig, ax = plt.subplots()
ax.scatter(temperature, humidity, color = 'black')
ax.set_title('Humidity vs Temperature in San Francisco on Some Evening')
ax.set_ylabel('Humidity (%)')
ax.set_xlabel('Temperature (F)')
plt.show()
```
<figure align="center">
<p align="center">
<img src="/humidity_v_temp.png" alt="humidity versus temp in SF" style="height: 400px; width: 500px;"/>
</p>
</figure>

To then model the data using maximum likelihood estimation, there are a litany of libraries one may import. You can just import scipy's stat module and use the linregress function and get the parameters we're interested in--namely slope and y-intercept. 

```python
from scipy import stats
slope, int, r, p, se = stats.linregress(temperature, humidity)
```

<figure align="center">
<p align="center">
<img src="/humidity_v_temp_reg.png" alt="humidity versus temp in SF regression" style="height: 400px; width: 500px;"/>
</p>
</figure>

There are then a number of ways to assess how good your model is. For brevity, we'll focus on one value which focuses on the distances between the predicted output and the actual output. 

In particular, this is the average of the sum of the squared differences between the observed output and the output the model predicted. This value is called the **mean squared error**. A mean squared error of 0 indicates that the model is has perfectly predicted the observed values. Anything larger than 0 indicates there is some error.

You can see the mean squared error is 14.16 for the model constructed via the maximum likelihood method in the image above. This was calculated via

```python
def calc_mse(expected: list, actual: list):
    sse = 0
    e_count = 0
    while e_count < len(expected):
        sse += (expected[e_count] - actual[e_count])**2
        e_count += 1
    return sse/len(expected)
```

The next question you may ask is what is a good mean squared error? In other words, how do we assign value to the mean squared error. Can we only assign value to this calculation relative to other models? Thus, one model with a lower mean squared error than the other is better? 

I think in mathematics we should be wary of value assignment to calculations. For example, 5 > 2, does this make 5 any better than 2? No, its just a statement saying that 5 is larger than 2 numerically, you actually need two and half 2s to make a 5. The same is said with this loss function. The sum of the squared distances from the predicted outputs and the actual outputs is less for one model than the other. If your goal is to accurately predict new predict observations, than this is indeed an indication of a better model fit.