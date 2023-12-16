---
layout: post
title:  "Gradient Descent and its Relationship to Maximum Likelihood Regression"
categories: Data
---

<script
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"
  type="text/javascript">
</script>

## Introduction

Gradient descent is an optimization algorithm. Gradient descent therefore is instructions aimed towards optimizing a function - in other words finding the inputs of the function that will yield an extremum of that function. 

Finding the optimum of a function is useful in statistics and thus gradient descent is a useful method. Despite its usefulness, there may be more effective solutions to finding the optimum of a function. Take for example the closed form solution to the maximum likelihood estimation setting in regression. A closed-form solution will be more efficient (require less steps) than gradient descent (may require more steps).

Below is a description of the gradient descent algorithm.

## Theory

Suppose you're given some function $$f$$ that takes in $$x_*$$ as an argument. Further suppose that the function is differentiable at every possible input. The algorithm to finding an optimum for $$f$$ then is first starting at some initial point $$x_o$$ then doing some operations on that initial point to then get a little closer to the nearest optimum, $$x_1$$, then re-iterating the some operation on that new point to get even closer to the optimum again.

More specifically, the algorithm is

1. Take some random input value to start, $$x_o$$. Your initial point then is 

$$x_o$$

2. Calculate the gradient of $$f$$, pick a sufficient scalar to act as a "step size" and subtract that value away from the initial point $$x_o$$. The "sufficient step size" follows a simple heuristic. The step size is too large if $$x_{n+1} > x_n$$, so re-do the calculation with a smaller step size. The step size is too small if $$x_{n+1} < x_n$$ so increase the step size.

$$

x_1 = x_o - \lambda*df/dx_o, \lambda = "sufficient step size"

$$

3. Eventually, you'll reach some $$x_n$$ that results in the gradient being 0 at that $$x_n$$. That value then is the optimum you're looking for.

Lets do an example using the polynomial $$f(x) = x^2$$.

## Application


