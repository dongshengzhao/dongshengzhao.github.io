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
>![Scatter Plot of Two variables]({{ site.url }}/images/logisticRegression/1.png "Figure 1")
>
> From the particular example above, it is not hard to figure out we could find a line to seperate the two classes. Specifically we divide the 2-D plane into 2 parts according to a line, and then we can predict new sample by observing which part it belongs to. Mathematically if $$ z = w_0 + w_1x_1 + w_2x_2$$ >= 0, then y = 1; if $$ z = w_0 + w_1x_1 + w_2x_2$$ < 0, then y = 0.

## 3. How to find the best line
> The hypothesis is a linear model $$ w_0 + w_1x_1 + w_2x_2 = W^TX $$, the threshold is z = 0. The value of z depends on the distance between the point and the target line, and the absolute value of z could be very large and small. We could **normalize the distances** for convience, however, we had better not use linear normalization such as x / (max(x) - min(x)) and x / (std(x)), because the distinction between the two classes is more obvious when the absolution value of z is larger. Sigmoid or logistic function is well-known to be used here, following is the function and plot of sigmoid function.
>
> $$ g(z) = \frac{1}{1 + e^{-z}} $$
>
> ![Sigmoid function]({{ site.url }}/images/logisticRegression/2.png "Figure 2")
>
> The new model for classification is:
>
> $$ h(x) = \frac{1}{1 + e^{-w^Tx}} $$
> We can see from the figure above that when z > 0, g(z) > 0.5 and when the absolute vaule of v is very large the g(z) is more close to 1. 

## 4. Figure out the loss function
> we need to find a way to measure the agreement between the predicted scores and the ground truth value.
>
> **Naive idea**
>
> We could use least square loss after normalizing the training data, the result is as following:
> $$L_0 = \frac{1}{m} \sum_i^m(h(x^{(i)}) - y^{(i)})^2 = \frac{1}{m} \sum_i^m(\frac{1}{1 + e^{-w^Tx^{(i)}}} - y^{(i)})^2$$, where $$x^{(i)}$$ is a vector for all $$x_j$$ (j=0,1, ... , n), and $$y^{(i)}$$ is the target value for this example. However this loss function is not a convex function because of sigmoid function used here, which will make it very difficult to find the w to opimize the loss.
>
> **Can we do better?**
>
> Because of this is a binary classification problems, we can compute the loss for the two classes respectively. When target y = 1, the loss had better be very large when $$ h(x) = \frac{1}{1 + e^{-w^Tx}} $$ is close to zero, and the loss should be very small when h(x) is close to one; in the same way, when target y = 0, the loss had better be very small when h(x) is close to zero, and the loss should be very large when h(x) is close to one. In fact, we can find this kind of function: 
>
>$$ L(h(x), y) =\begin{cases} -log(h(x)) & y = 1\\ -log(1 - h(x)) & y  = 0 \end{cases} => L(h(x), y) = -ylog(h(x)) - (1-y)log(1-h(x))$$
>
>So the total loss: $$L(w) = - \frac{1}{m} \sum_i^m [y^{(i)}logh(x^{(i)}) + (1 - y^{(i)}) log(1-h(x^{(i)}))]$$
> 
> $$x^{(i)}$$ is a vector for all $$x_j$$ (j=0,1, ... , n), and $$y^{(i)}$$ is the target value for this example. 
> 
>$$h(x) = \frac{1}{1 + e^{-w^Tx}} $$
>
> The plots of loss function are shown below, and they meet the desirable properties discribed above.
> ![Loss function]({{ site.url }}/images/logisticRegression/3.png "Figure 3")

## 5 Find the best w to minimize the loss
> Like [linear regression]({{ site.url }}/linear-regression-post/) we can use **gradient descent algorithm** to optimize w step by step.
> Compute the gradient for just one sample:
>
> $$ \begin{equation}
     \begin{split} 
     \frac{\partial}{\partial w_j} L(w) 
     &= (y \frac{1}{g(w^Tx)} - (1-y)  \frac{1}{1 - g(w^Tx)})  \frac{\partial}{\partial w_j} g(w^Tx) \\
     &= (y \frac{1}{g(w^Tx)} - (1-y)  \frac{1}{1 - g(w^Tx)})  g(w^Tx)(1 - g(w^Tx)) \frac{\partial}{\partial w_j} w^Tx \\
     &= (y(1-g(w^Tx)) - (1-y)g(w^Tx))x_j \\
     &= (y-h(x))x_j                                    
    \end{split}
    \end{equation} $$
> 
> Then we can use **batch decent algorithm** or **stochastic decent algorithm** to optimize **w**, i.e, $$w := w + \alpha \frac{\partial}{\partial w_j} L(w) $$
>
> We can see that the gradient or partial derivative is the same as gradient of linear regression except for the h(x). We can get a better understanding of this when interpretating the loss function from probabilistic aspect.

## 6. Probabilistic interpretation
> Let us regard the value of h(x) as the probability:
>
> $$ \begin{cases} P(y=1|x;w) = h(x) \\ P(y = 0 | x; w) = 1 - h(x) \end{cases} => P(y|x;w) = (h(x))^y(1-h(x))^{1-y}$$
>
> So the likelihood is:
>
> $$\begin{equation}
     \begin{split} 
     L(w) &= p(y|X; w) \\ 
     &= \prod_{i = 1}^m p(y^{(i)}|x^{(i)};w) \\
     &= \prod_{i = 1}^m (h(x^{(i)}))^{y^{(i)}} (1-h(x^{(i)}))^{1-y^{(i)}} \\
     &= (y(1-g(w^Tx)) - (1-y)g(w^Tx))x_j \\
     &= (y-h(x))x_j                                 
    \end{split}
    \end{equation}$$
> 
> And the log likelihood:
> 
>$$ \begin{equation}
     \begin{split} 
     l(w) = log(L(w))
     &= \sum_{i = 1}^m y^{(i)} log(x^{(i)}) + (1 - y^{(i)}) log(1 - h(x^{(i)}))                            
    \end{split}
    \end{equation} $$
>
> This equation is the same as the the loss function when picking minus, so minimize the loss can be interpretated as maximize the likelihood of the y when given x `p(y|x)`. What's more, the value of h(x) can be interpretated as the probability of the sample to be classified to y = 1. I think this is why most people prefer sigmoid function for normalization, theoretically we can choose other functions that smoothly increase from 0 to 1.
>
> After we optimize the w, we get a line in 2-D space and the line is usually called decision boundary (h(x) = 0.5). We can also generalize to binary classification on n-D space, and the corresponding decision boundary is a (n-1) Dimension hyperplane (subspace) in n-D space.

## 7. From binary to multiclass classification
> We need to generalize to the multiple class case, that's to say, the value of y is not binary any more, instead y can equal to 0, 1, 2, ..., k.
>
> * ##### Basic idea -- Transfer multi-class classification into binary classification problem
> We need change multiple classes into two classes, and the idea is to construct several logistic classifier for each class. We set the value of y (label) of one class to 1, and 0 for other classes. Thus, if we have K classes, we build K logistic classifiers and use it for prediction. There is a potential problem that one sample might be classified to several classes or non-class. The solution is to compare all the values of h(x) and classify the sample to the class with the highest value of h(x). The idea is shown in following figure.
>
> ![One vs all]({{ site.url }}/images/logisticRegression/4.png "Figure 4")
>
> * #### Better idea -- Softmax

## 8. Get your hands dirty and have fun







