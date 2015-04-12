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
> We could plot the data on a 2-D plane and try to figure out whethere there is any structure (see following figure).
> 
>![Scatter Plot of Two variables]({{ site.url }}/images/logisticRegression/1.png "Figure 1")