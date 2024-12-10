---
author: "Daniel Sherwood"
layout: post
title: "Bayesian Regression - Part 1"
date: 2024-12-09 20:58:00 -0500
categories: python
lead: "Bayesian Regression ~ OLS"
--- 
**I will admit** that the Bayesian approach to statistical modeling is not perfect. There are some who treat the word _Bayesian_ 
as a slur, and others who treat it as a sacrament to be enjoyed by the holy few. I am a little more ambivalent. Although it
is incredibly fun to work with, I've found it personally frustrating to fine-tune the priors of my model. But I am putting 
the cart before the horse. I have three separate posts in mind to explain my thoughts. The first, this post, will be about 
how similar Bayesian and orinary regression are to each other. The second will go into more details as to how they differ, 
and the third will talk about implications for prediction. 

**The first thing** we will do is generate some data. The following piece of code is ripped straight from a `PYMC` [notebook](https://www.pymc.io/projects/docs/en/stable/learn/core_notebooks/GLM_linear.html). 

```python
size = 200
true_intercept = 1
true_slope = 2

x = np.linspace(0, 1, size)
# y = a + b*x
true_regression_line = true_intercept + true_slope * x
# add noise
y = true_regression_line + rng.normal(scale=0.5, size=size)
```
**here's the data** we generated.

![the_data.png](/assets/images/the_data.png)
