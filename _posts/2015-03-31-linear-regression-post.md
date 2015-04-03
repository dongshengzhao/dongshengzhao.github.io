---
layout: post
title: Linear Regression Post
description: "The basic linear regression alorithm"
modified: 2015-03-31
tags: [Machine Learning]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditli: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---
## 1. Problem setting
> We want to use a **predictor variable** X to predict a **quantitative response** Y, such as using living area (X) to predict the price (Y) of house. 


## 2. Basic idea
> - Becuase of just two variables, we can simply visualize the data on a scatter plot, then we can predict Y by the structure of the plot (Figure 1).
> - After getting the scatter plot, we can estimate the position of potiential point when given the x value and get the corresponding value y.
> ![Scatter Plot of Two variables]({{ site.url }}/images/linearRegression/1.jpg)


## 3. Can we do better 
> - So far it seems that the problem can be solved, however, we shold always ask the quesion, i.e., "Can we do better?"
> - What's the shortcoming of the above solution? 
>	- We have to firstly get the scatter plot, which is a problem when it scales to high dimension prediction problems.
>	- We need our human's eyes to find the position of the position of potential point. We humans are not happy with that, instead we want the computer to do all the work. In addition, we can easily get overwhelmed when amounts of prediction needed.

## 4. Better idea
> - If we look at the structure of the scatter plot above, it is not hard to figure that the Y value is increasing when X gets bigger. So it is possible to find a model to fit all the data, then use the model instead for prediction.
> - Then what's kind of model we should use? Linear model may be a good choice because of its simplicity and ability to show the general trend.
> - The next task is how to find the "best" line

$$
f(x\_i, W, b) =  W x\_i + b
$$

Here is an example MathJax inline rendering \\( 1/x^{2} \\), and here is a block rendering: 
\\[ \frac{1}{n^{2}} \\]

$$ \mathbf{X}\_{n,p} = \mathbf{A}\_{n,k} \mathbf{B}\_{k,p} $$
