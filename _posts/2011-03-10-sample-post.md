---
layout: post
title: Sample Post
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2014-12-24
tags: [sample post]
image:
  feature: abstract-3.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---

Below is just about everything you'll need to style in the theme. Check the source code to see the many embedded elements within paragraphs.


# Heading 1

## Heading 2

### Heading 3

#### Heading 4

##### Heading 5

###### Heading 6

### Body text


$$
\begin{align*}
  & \phi(x,y) = \phi \left(\sum_{i=1}^n x_ie_i, \sum_{j=1}^n y_je_j \right)
  = \sum_{i=1}^n \sum_{j=1}^n x_i y_j \phi(e_i, e_j) = \\
  & (x_1, \ldots, x_n) \left( \begin{array}{ccc}
      \phi(e_1, e_1) & \cdots & \phi(e_1, e_n) \\
      \vdots & \ddots & \vdots \\
      \phi(e_n, e_1) & \cdots & \phi(e_n, e_n)
    \end{array} \right)
  \left( \begin{array}{c}
      y_1 \\
      \vdots \\
      y_n
    \end{array} \right)
\end{align*}
$$

Lorem ipsum dolor sit amet, test link adipiscing elit. **This is strong**. Nullam dignissim convallis est. Quisque aliquam.

![Smithsonian Image]({{ site.url }}/images/3953273590_704e3899d5_m.jpg)
{: .image-right}

*This is emphasized*. Donec faucibus. Nunc iaculis suscipit dui. 53 = 125. Water is H<sub>2</sub>O. Nam sit amet sem. Aliquam libero nisi, imperdiet at, tincidunt nec, gravida vehicula, nisl. The New York Times <cite>(That’s a citation)</cite>. <u>Underline</u>. Maecenas ornare tortor. Donec sed tellus eget sapien fringilla nonummy. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus.

HTML and <abbr title="cascading stylesheets">CSS<abbr> are our tools. Mauris a ante. Suspendisse quam sem, consequat at, commodo vitae, feugiat in, nunc. Morbi imperdiet augue quis tellus. Praesent mattis, massa quis luctus fermentum, turpis mi volutpat justo, eu volutpat enim diam eget metus.

### Blockquotes

> Lorem ipsum dolor sit amet, test link adipiscing elit. Nullam dignissim convallis est. Quisque aliquam.

## List Types

### Ordered Lists

1. Item one
   1. sub item one
   2. sub item two
   3. sub item three
2. Item two

### Unordered Lists

* Item one
* Item two
* Item three

## Tables

| Header1 | Header2 | Header3 |
|:--------|:-------:|--------:|
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|----
| cell1   | cell2   | cell3   |
| cell4   | cell5   | cell6   |
|=====
| Foot1   | Foot2   | Foot3
{: rules="groups"}

## Code Snippets

Syntax highlighting via Pygments

{% highlight css %}
#container {
  float: left;
  margin: 0 -240px 0 0;
  width: 100%;
}
{% endhighlight %}


{% highlight matlab linenos %}
%======= Machine Learning Coursework2 Decision Tree =========
%
% Load data, and split it into training data and test data
clear; clc; close all;
load('emotions_data'); 
% x - A [num_sample, num_attr] matrix where each row is a sample
% y - A vector where y(i) is the label for x(i, :)
[num_sample, num_attr] = size(x);

num_uni_emo = length(unique(y));

%% Kfold cross validation
% Stratified cross-validation by K fold
K = 10;% K folds
cp = cvpartition(y, 'k', K);

% Store the trained trees for each fold training
trees_cell = cell(1, K); % each cell is a cell of 6 (emotions) trees here

% Store the precision, recall and F1 in a cell of size K
pre_recall_f1_cells = cell(1, K);

% Store the confusion matrix for each fold
cm_cell = cell(1, K);

% Store the prediciton 
output_cell = cell(1, K);

% Sum of all the precision, recall and f1 by following iteration
sum_pre_recall_f1 = zeros(num_uni_emo, 3); % [6, 3]

% Store the average of f1 score to help select the best model
ave_f1 = zeros(1, K);

% Sum of all confusion matrix by following iteration
sum_confusion_mat = zeros(num_uni_emo); % [6, 6]
%%
% Iterate all the K fold data to train net and compute related variable
for i = 1:cp.NumTestSets
    fprintf('\n\nThe %d iteration for kfold validation ...\n', i)
    
    % get the training and test index
    train_id = cp.training(i);
    test_id = cp.test(i);
    
    % Create and train decision trees 
    trees = train_decision_trees(x(train_id, :), y(train_id));
    
    % Compute and store the test dataset using the new trained trees
    output_test = predict_by_all_trees(trees, x(test_id, :));
    output_cell{i} = output_test;
    
    % compute the confusion matrix
    confusion_mat = compute_confusion_matrix(output_test, y(test_id));
    
    % Compute the accuracy
    accuracy = sum(diag(confusion_mat)) / sum(test_id);
    fprintf('the accuracy of %d fold is %d\n', i, accuracy);
    
    % Store the trees 
    trees_cell{i} = trees;
    
    % Store confusion matrix
    cm_cell{i} = confusion_mat; % [6, 6]

    % Compute and store the precision, recall, and F1
    pre_recall_f1 = compute_pre_fecall_f1(confusion_mat);
    pre_recall_f1_cells{i} = pre_recall_f1;
    
    % Add the new confusion matrix to sum_confusion_mat
    sum_confusion_mat = confusion_mat + sum_confusion_mat; % [6, 6]
    
    % Add the new precision, recall and F1 to sum_pre_recall_F1
    sum_pre_recall_f1 = pre_recall_f1 + sum_pre_recall_f1; % [6, 3]
    
    % Store average f1 value
    ave_f1(i) = mean(pre_recall_f1(:, 3));
  
end
%%
% Compute the average of confusion matrix
ave_confusion_mat = sum_confusion_mat / K; % [6, 6]

% Compute the average of precision, recall and F1
ave_pre_recall_f1 = sum_pre_recall_f1 / K; % [6, 3]

% print the average of confusion matrix
fprintf('\n\n\nThe average confusion matrix: \n');
disp(ave_confusion_mat);

% print the average of precision, recall and f1 score
fprintf('The average precision, recall and F1.\n  precision   recall   F1_score \n');
disp(ave_pre_recall_f1);

% Get the index of fold that have biggest f1 value
[~, ind_res] = max(ave_f1);

% Get the final decision trees with biggest f1 value
trees_res = trees_cell{ind_res};

% plot the all the trees in the final resutl
for i = 1:length(trees_res)
    DrawDecisionTree(trees_res{i}, emolab2str(i));
end
%%
% plot confusion matrix
output_best = output_cell{ind_res};
target = y(cp.test(ind_res));

% processing data for plotconfusion 
output_best_mat = trans_vect_bin_mat(output_best');
target_mat = trans_vect_bin_mat(target');
plotconfusion(target_mat, output_best_mat);
}
{% endhighlight %}

Non Pygments code example

    <div id="awesome">
        <p>This is great isn't it?</p>
    </div>

## Buttons

Make any link standout more when applying the `.btn` class.

{% highlight html %}
<a href="#" class="btn btn-success">Success Button</a>
{% endhighlight %}

<div markdown="0"><a href="#" class="btn">Primary Button</a></div>
<div markdown="0"><a href="#" class="btn btn-success">Success Button</a></div>
<div markdown="0"><a href="#" class="btn btn-warning">Warning Button</a></div>
<div markdown="0"><a href="#" class="btn btn-danger">Danger Button</a></div>
<div markdown="0"><a href="#" class="btn btn-info">Info Button</a></div>
