---
layout: post
title: Support Vector Machine
description: "The support vector machine alorithm for classification"
modified: 2015-04-25
tags: [Machine Learning]
image:
  feature: abstract-2.jpg
  credit: dargadgetz
  creditli: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

> Support vector machine (SVM) is often considered one of the best “out of the box” classifiers, and in this post I try to explain how we can come up with this alogrithm from scratch. I also implement the SMV for image classification with [CIFAR-10 dataset](http://www.cs.toronto.edu/~kriz/cifar.html) by Python (numpy). [The first one]({{ site.url }}) is binary classification, [the second one]({{ site.url }}) is multi-classifiction using binary SVMs with one-vs-all trick and [the last one]({{ site.url }}) is mutli-classification SVM.

<!-- more -->

## 1. Problem setting
> Classification problem is to classify different objects into different categories. For simplicity, we just focus on **binary classification** that y can take two values 1 or -1 (indicating two classes), and we firstly assume the two classes are linearly separable. After all, it is reasonable to solve problems from simple to complex.

## 2. Basic idea (What we have known)
> If the data is linearly separable, our goal is to find the such a line $$w^Tx + b = 0$$ (2-dimension) that divides the plane into 2 parts and each part represent one class (see following figure). If the data is represented in high dimension say N-dimenstion, what we need to do is to find a hyperplane $$w^Tx + b = 0$$ which is subspace with dimension (N-1)dimension. So if $$w^Tx + b >= 0$$, the label $$y = 1$$, otherwise $$y = -1$$. However, the problem is that in fact there exists infinite such hyperplanes if the data can be perferctly linearly separated, because a given separating hyperplane can be shifted a tiny bit up or down, or rotated without coming into contact with any of the observations (the line 1, 2 and 3 in the following figure) . Of course we can randomly choose a separating line. 
>
>![Scatter Plot of Two variables]({{ site.url }}/images/SVM/1.png "linearly separable")
>

## 3. Maximal Margin Classifier 
> **Can we do better?**
>
> Is that possible for us to choose the even "best" line or hyperplane from the infinit possible separating hyperplanes? So the next question is how to define the "best" hyperplane. Because the final goal is trying to use the hyperplane as decision boundary to distinguish the two classes, so we can choose the hyperplane which can make the distinction more obvious. Intuitively the separating hyperplane should be farthest from the training observations, that's to say, the distance between the nearest observation and the hyperplane should be maximized. This distance is usually called margine and the corresponding classifier is known as maximal margin classifier, and the separating hyperplane has the farthest minimum distance to the training observations. Take the above figure for example, line 3 is better than line 1 and 2.
>
> From figure below, we can see that there are 3 training points having equal distance from the maximal margin line and the two dash lines indicate the width of margin. These 3 observations are known as **support vectors**. Since these points can interpreted as n-1 dimenstion vectors and define the maximal margin, in other words, these vectors can "support" the maximal margin hyperplane in the sense that if these points were moved slightly then the maximal margin hyperplane would move as well. What's more, the maximal margin hyperplane is only depends on the support vectors, not other observation.
>
> ![Support Vector]({{ site.url }}/images/SVM/2.png "support vector")
>
> **Calculate the maximal margin**
> In order to calculate the maximal margin, we should figure out how to calculate the geometric margin which is the distance from a point to a line or hyperplane. As following figure, the point at A representing the input $$x^{(i)}$$ of some training example. Its distance to the decision boundary (a line with (w, b)) is $$\gamma^{(i)}$$, is given by the line segment AB. And the distance $$\gamma^{(i)}$$ can be calculate in the following way: 
>
> ![geometric margin]({{ site.url }}/images/SVM/3.png "geometric margin")
>
> vector $$BA = x_A - x_B$$, unit vector is $$w/\|w\|$$, so the point B is given by $$x^{(i)} - \gamma^{(i)} w/\|w\|$$. And point B is on the decision boundary $$w^T x + b$$, therefore 
> 
> $$ w^T \big(x^{(i)} - \gamma^{(i)} \frac{w}{\|w\|}\big) + b = 0$$
>
>Then solving $$\gamma^{(i)}$$ yields:
>
>$$\gamma^{(i)} = \frac{w^T x^{(i)} + b}{\|w\|}$$
> 
> Using bias trick to represent the two parameters **w** and **b** as one, i.e. set $$x_0 = 1$$ and add $$w_0$$ to weights vector **w**.
> Then we get:
>
> $$\gamma^{(i)} = \frac{w^T x^{(i)}}{\|w\|}$$
>
>Therefore based on a set of m training observations $$x_1, x_2, ..., x_m$$ and associated class labels $$y_1, y_2, ..., y_m \in \big\{1, -1\big\} $$, the assumption that the training set is linearly separable, the maximal margin line or hyperplane is the solution to the optimization problem.
>
> $$Maximize_{w, M} \:\:\: \frac{M}{\|w\|}  \:\:\:......... (1)$$ 
>
> Subject to 
>
> $$y^{(i)} (W^Tx^{(i)}) >= M \:\: \forall i = 1, 2, ..., m \:\:\:......... (2)$$ 
>
> The constrains (2) guarantees that each observation will be on the correct side of the decision boundary and the value of $$y^{(i)} (W^Tx^{(i)})$$ is at least M, provided that M is positive. In addtion, the margin is given by $$\frac{w^T x^{(i)}}{\|w\|}$$, the objective function $$(1) \frac{M}{\|w\|}$$ ensures that each observation has at least a distance $$\frac{M}{\|w\|}$$ from the hyperplane or decision boundary. Hence, the optimization problem choose **w** and **M** to maximize $$\frac{M}{\|w\|}$$.
>
> **Solve the optimization problem**
>
>If we could solve the optimization problem above efficiently, then we would be done. In fact the optimization problem above is very difficult because we have a nasty objective $$\frac{M}{\|w\|}$$ function, which is non-convex. So can we do better?
>
>The final goal is to find the decision boundary $$w^T x = 0$$, so multiplying w by some constant can affect the margin but doesn't change the decision boundary. Therefore, we can set the value of $$w^T x_0$$ for the nearest point to be 1, i.e., $$M = 1$$. Additionally maximize $$\frac{1}{\|w\|}$$ is the same to minimize \|w\|, again is the same thing as minimizing $$\|w\|^2$$. Therefore we have the following optimization problem:
>
> $$Minimize_w \:\:\: \frac{1}{2}\|w\|^2  \:\:\:......... (1)$$ 
>
> Subject to 
>
> $$y^{(i)} (W^Tx^{(i)}) >= 1 \:\: \forall i = 1, 2, ..., m \:\:\:......... (2)$$ 
>	
> The new version of optimization problem can be efficiently solved, because the objective function is a convex quadratic function and all the constrains are linear. The problem can be solved by Quadratic Program (QR) software such as [CVXOPT](http://cvxopt.org) for Python.

<!-- ## New idea:
- asymmetric prediction, max margins for two classes are different.
- Two mimimal margin classifier
 -->


