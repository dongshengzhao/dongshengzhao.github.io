---
layout: post
title: Logistic Regression
description: "The logistic regression alorithm for binary classification"
modified: 2015-04-12
tags: [Machine Learning]
image:
  feature: abstract-1.jpg
  credit: dargadgetz
  creditli: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

## 1. Problem setting
> Classification problem is to classify different objects into different categories. It is like regression problem, except that the predictor y just has a small number of discrete values. For simplicity, we just focus on **binary classification** that y can take two values 1 or 0 (indicating two classes). 

## 2. Basic idea
> We could plot the data on a 2-D plane and try to figure out whethere there is any structure of the data (see following figure).
> 
>[Scatter Plot of Two variables]({{ site.url }}/images/logisticRegression/1.png "Figure 1")
> From the particular example above, it is not hard to figure out we could find a line to seperate the two classes. Specifically we divide the 2-D plane into 2 parts according to a line, and then we can predict new sample by observing which part it belongs to. Mathematically if $$ z = w_0 w_1x_1 w_2x_2$$ >= 0, then y = 1; if $$ z = w_0 w_1x_1 w_2x_2$$ < 0, then y = 0.

## 3. How to find the best line
> The hypothesis is a linear model $$ w_0 w_1x_1 w_2x_2 = W_TX $$, the threshold is z = 0. The value of z depends on the distance between the point and the target line, and the absolute value of z could be very large and small. We could **normalize the distances ** for convience, however, we had better not use linear normalization such as x / (max(x) - min(x)) and x / (std(x)), because the distinction between the two classes is more obvious when the absolution value of z is larger. Sigmoid or logistic function is well-known to be used here, following is the function and plot of sigmoid function.
>
> $$ g(z) = \frac{1}{1 + e^{-z}} $$
>
> The new model for classification is:
>
> $$ g(x) = \frac{1}{1 + e^{-W^TX}} $$
>
> [Sigmoid function]({{ site.url }}/images/logisticRegression/2.png "Figure 2")



