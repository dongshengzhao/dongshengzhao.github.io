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
> From the particular example above, it is not hard to figure out we could find a line to seperate the two classes. Specifically we divide the 2-D plane into 2 parts according to a line, and then we can predict new sample by observing which part it belongs to. Mathematically if $$ z = w_0 + w_1x_1 + w_2x_2$$ >= 0, then y = 1; if $$ z = w_0 + w_1x_1 + w_2x_2$$ < 0, then y = 0. We can regard the linear function $$w^Tx$$ as a mapping from raw sample data ($$x_1, x_2$$) to classes scores. Intuitively we wish that the "correct" class has a score that is higher than the scores of "incorrect" classes.

## 3. How to find the best line
> The hypothesis is a linear model $$ w_0 + w_1x_1 + w_2x_2 = W^TX $$, the threshold is z = 0. The score value of z depends on the distance between the point and the target line, and the absolute value of z could be very large or small. We could **normalize the distances** for convenience, however, we had better not use linear normalization such as x / (max(x) - min(x)) and x / (std(x)), because the distinction between the two classes is more obvious when the absolution value of z is larger. Sigmoid or logistic function is well-known to be used here, following is the function and plot of sigmoid function.
>
> $$ g(z) = \frac{1}{1 + e^{-z}} $$
>
> ![Sigmoid function]({{ site.url }}/images/logisticRegression/2.png "Figure 2")
>
> The new model for classification is:
>
> $$ h(x) = \frac{1}{1 + e^{-w^Tx}} $$
> We can see from the figure above that when z > 0, g(z) > 0.5 and when the absolute vaule of v is very large the g(z) is more close to 1. By feeding the score to sigmoid function, not only the scores can be normalized from 0 to 1, which can make it much easier to find the loss function, but also the result can be interpreted from probabilistic aspect.

## 4. Figure out the loss function
> we need to find a way to measure the agreement between the predicted scores and the ground truth value.
>
> **Naive idea**
>
> We could use least square loss after normalizing the training data, the result is as following:
> $$L_0 = \frac{1}{m} \sum_{i=1}^m(h(x^{(i)}) - y^{(i)})^2 = \frac{1}{m} \sum_{i=1}^m(\frac{1}{1 + e^{-w^Tx^{(i)}}} - y^{(i)})^2$$, where $$x^{(i)}$$ is a vector for all features $$x_j^{(i)}$$ (j=0,1, ... , n) for single sample i, and $$y^{(i)}$$ is the target value for this example. However this loss function is not a convex function because of sigmoid function used here, which will make it very difficult to find the w to opimize the loss.
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
     &= -(y \frac{1}{g(w^Tx)} - (1-y)  \frac{1}{1 - g(w^Tx)})  \frac{\partial}{\partial w_j} g(w^Tx) \\
     &= -(y \frac{1}{g(w^Tx)} - (1-y)  \frac{1}{1 - g(w^Tx)})  g(w^Tx)(1 - g(w^Tx)) \frac{\partial}{\partial w_j} w^Tx \\
     &= -(y(1-g(w^Tx)) - (1-y)g(w^Tx))x_j \\
     &= (h(x)-y)x_j                                    
    \end{split}
    \end{equation} $$
> 
> Then we can use **batch decent algorithm** or **stochastic decent algorithm** to optimize **w**, i.e, $$w := w + \alpha \frac{\partial}{\partial w_j} L(w) $$
>
> We can see that the gradient or partial derivative is the same as gradient of linear regression except for the h(x). We can get a better understanding of this when interpreting the loss function from probabilistic aspect.

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

## 7. Multiclass classification -- One vs all
> We need to generalize to the multiple class case, that's to say, the value of y is not binary any more, instead y can equal to 0, 1, 2, ..., k.
>
> #### Basic idea -- Transfer multi-class classification into binary classification problem
> We need change multiple classes into two classes, and the idea is to construct several logistic classifier for each class. We set the value of y (label) of one class to 1, and 0 for other classes. Thus, if we have K classes, we build K logistic classifiers and use it for prediction. There is a potential problem that one sample might be classified to several classes or non-class. The solution is to compare all the values of h(x) and classify the sample to the class with the highest value of h(x). The idea is shown in following figure (From Andrew's notes).
> ![One vs all]({{ site.url }}/images/logisticRegression/4.png "Figure 4")

## 8. Can we do better? -- Softmax
> In logistic regression classifier, we use linear function to map raw data (a sample) into a score z, which is feeded into logistic function for normalization, and then we interprete the results from logistic function as the probability of the "correct" class (y = 1). We just need a mapping function here because of just two classes (just need to decide whether one sample belongs to one class or not).
> For mutilple classes problems (K categoires), it is possible to establish a mapping function for each class. As above we can simply use a linear mapping for all classes (K mapping function):
>
> $$ f(x^{(i)}, W, b) = Wx{(i)} + b $$
> 
> Where $$x^{(i)}$$ is a vector for all features $$x_j^{(i)}$$ (j=0,1, ... , n) for single sample i, and $$x^{(i)}$$ is a single column vector of shape $$[D, 1]$$. **W** is a matrix of shape $$[K, D]$$ called **weights**, **K** is the number of categories, and **b** is a vecotr of $$[K, 1]$$ called **bias vector**. It is a little cumbersome to keep track of two sets of parameters (**W** and **b**), in factor we can combine the two into a single matrix. Specifically we can extend the feature vector $$x^{(i)}$$ with an addition bias dimention holding constant 1, while extending **W** matrix with a new column (at the first or last column). Thus we get score mapping function:
>
> $$f(x^{(i)}, W) = Wx^{(i)} $$
>
> Where **W** is a matrix of shape $$[K, D+1]$$, $$x^{(i)}$$ is vector of shape $$[D+1, 1]$$, and $$f(x^{(i)}, W)$$ is a vector of shape $$[K, 1]$$ indicating the different scores of every class for the $$i^{th}$$ sample.
>
> #### Find the loss function
> Similar to logistic regression classifier, we need to normalize the scores from 0 to 1. However we should not use a linear normalization as discussed in the logistic regression because the bigger the score of one class is, the more chance the sample belong to this category. What's more, the chance is similar high when the scores are very large (see the plot of logistic function above).
> Similar to logistic function, people use exponent function (non-linear) to preprocess the scores and then compute the percentage of each score in the sum of all the scores. What's more, the percentages can be interpreted as the probability of each class for one sample. Here is formula for the $$i^{th}$$ sample:
>
> $$h(x^{(i)}) = \frac{e^{w_{y_j}^Tx^{(i)}}} {\sum_{j = 1}^k e^{w_j^Tx^{(i)}}} $$
>
> Here is the plot of h(x) for two classes in 3D space, you can rotate the graph by clicking the arrows to get a better understanding the shape of the h(x).
> Where $$x^{(i)}$$ is vector of all features of sample i, $$w_j$$ is the weights for the $$j^{th}$$ class, and $$y_j$$ is the correct class for the $$i^{th}$$ sample.

<object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=7,0,0,0" width="600" height="700" id="function_plotter" align="middle">
  <param name="movie" value="http://dwudljvm154gg.cloudfront.net/graph3d.swf?lpf=e^x / (e^x%2Be^y)&lpxmin=-5&lpxmax=5&lpymin=-5&lpymax=5&lpzmin=0&lpzmax=1" />
  <param name="quality" value="high" />
  <param name="bgcolor" value="#ffffff" />
  <embed src="http://dwudljvm154gg.cloudfront.net/graph3d.swf?lpf=e^x / (e^x%2Be^y)&lpxmin=-3&lpxmax=3&lpymin=-3&lpymax=3&lpzmin=0&lpzmax=1" quality="high" bgcolor="#ffffff" width="700" height="750" name="function_plotter" align="middle" allowScriptAccess="sameDomain" type="application/x-shockwave-flash" pluginspage="http://www.macromedia.com/go/getflashplayer" />
</object>

>
>So why exponent function? In my opinion, it is natually to come up with.
>
> * It is a very simple and widely used non-linear function
> * This function is strictly increasing
> * This function is a convex function and its derivative is strictly increasing. That's to say, when the score is large, then make it even more larger.
>
> The lesson is that we should put exponent function in our toolbox for non-linear problems
>
> After normalizing the scores, we can use the same concept to define the loss function, which should make the loss small when the normalized score of h(x) is large, and penlize more when h(x) is small. Thus, we can use $$-log(h(x))$$ to compute the loss, and the loss for one sample is as following:
>
> $$ L_i = -log \big(h(x^{(i)})\big) = -log \big(\frac{e^{f_{y_j}^{(i)}}} {\sum_{j = 1}^k e^{f_j^{(i)}}}\big) = -log \big(\frac{e^{w_{y_j}^Tx^{(i)}}} {\sum_{j = 1}^k e^{w_j^Tx^{(i)}}}\big) $$
>
> Total loss for all sample is:
>
> $$ L = \frac{1}{m} \sum_{i = 1}^m L_i = - \frac{1}{m} \sum_{i = 1}^m log \big(h(x^{(i)})\big) = - \frac{1}{m} \sum_{i = 1}^m log \big(\frac{e^{f_{y_j}^{(i)}}} {\sum_{j = 1}^k e^{f_j^{(i)}}}\big) = - \frac{1}{m} \sum_{i = 1}^m log \big(\frac{e^{w_{y_j}^Tx^{(i)}}} {\sum_{j = 1}^k e^{w_j^Tx^{(i)}}}\big)$$
>
> #### Is there any problem with the loss function
> When writing code to implement the softmax function in practice, we should first compute the intermediate terms $$e^{f_j}$$ to make the scores bigger and use a logarithm function to make the score smaller. However, the value of $$e^{f_j}$$ may be very large due to the exponentials and dividing large numbers could be numerically unstable, so we should make $$e^{f_j}$$ smaller before division. Here is the trick by multiply the numerator and denominator by a constant C:
>
> $$\frac{e^{f_{y_j}^{(i)}}} {\sum_{j = 1}^k e^{f_j^{(i)}}} = \frac{C e^{f_{y_j}^{(i)}}} {C \sum_{j = 1}^k e^{f_j^{(i)}}} = \frac{e^{f_{y_j}^{(i)} + logC}} {\sum_{j = 1}^k e^{f_j^{(i)} + logC}}$$
>
> Because we have the flexibility to choose any number of C, we can choose C to make $$ e^{f_j^{(i)}} + logC $$ small. A common choice for C is to set $$ logC = -max_jf_j^{(i)}$$. This trick makes the highest value of $$f_j^{(i)} + logC$$ to be zero and less than 0 for others. So the values of $$ e^{f_j^{(i)}} + logC $$ are restricted from 0 to 1, which should be more appropriate from division.
> 
> #### Probabilistic interpretation
> We can interprete $$h(x) = P(y^{(i)}) = \frac{e^{w_{y_j}^Tx^{(i)}}} {\sum_{j = 1}^k e^{w_j^Tx^{(i)}}} $$ as the normalized probability of assigned to the correct label $$y^{(i)}$$ given sample x^{(i)} and parameters **W**. Firstly the score $$f(x^{(i)}, W) = Wx^{(i)} $$ can be interpreted as the unnormalized log probabilities. Then exponentiating the scores with on-linear function $$e^x$$ gives the unnormalized probabilities (may call frequency). Last using division for normalization to make the probabilities sum to one. Like logistic regression, the minimize the negative log likelihood of the correct class can also be interpreted as performing **Maximum Likelihood Estimation**. The loss function can be also deduced from probabilistic theory like logistic regression, in fact linear regression, logistic regression and softmax regression all belong to [Generalized Linear Model](http://en.wikipedia.org/wiki/Generalized_linear_model). 
> 

## 8. Regularization to avoid overfitting
> In practice we often add a **regularization loss** to the loss function provided above to penalize large **weights** to improve generalization. The most common regularization penalty **R(W)** is the **L2** norm.
>
> $$R(W) =  \sum_k  \sum_d W_{k, d}^2 $$
>
> So the total loss is the **data loss** and the **regularization loss**, so the full loss becomes:
>
> $$ L = \frac{1}{m} \sum_{i = 1}^m L_i + \lambda \sum_k  \sum_d W_{k, d}^2$$
>  
> The advantage of penalizing large weights is to improve generalization and make the trained model work well for unseen data, because it means that no input dimension can have a very large influence on the scores all by itself and the final classifier is encouraged to take into account allnput dimensions to small amounts rather than a few dimensions and very strongly.
>
> I have wrote **another posts** to discuss regularization in more details, especially how to interprete it. You can find the post [here]()

## 9. Get your hands dirty and have fun
> * Purpose: Implement loistic regression and softmax regression classifer. 
> * Data: CIFAR-10 dataset, consists of 60000 32x32 colour images in 10 classes, with 6000 images per class. There are 50000 training images and 10000 test images. The data is availabe [here](http://www.cs.toronto.edu/~kriz/cifar.html)
> * Setup: I choose Python (IPython, numpy etc.) on Mac for implementation, and the results are published in a IPython notebook, ***[click here]({{ site.url }}/implementation/Logistic_Softmax.html)*** for the details.
> * Following is code to implement the logistic and softmax classifers by gradient decent algorithm.


























