---
title: week seven - K Nearest Neighbors, Decision Trees,Random Forests.
bigimg: /img/wk-7.jpeg
comments: true
tags: [K Nearest Neighbors, Decision Trees,Random Forests]
---
Immersing self in machine learnings, regression and classification problems can be solved through a variety of steps. For this week the focus is on:

### What new skills have you learned?

📦   K Nearest Neighbors

📦  Decision Trees

📦  Random Forests

#### K Nearest Neighbors
KNN is a classification algorithm that classifies elements in a dataset based
on features of the closest (nearest) points.
K is used to set the no. of nearest neighbors that is used to classify an entity.

Key components used in creating the classifier are;

📭  Distance Metric

🔎  No. of `Nearest` neighbors to look at.

⛲  Optional weighting function

💥  Method of aggregating neighboring points.;Usually defaults to Simple majority vote

Here is a [notebook on fruit classification] 🍎 using K Nearest Neighbors.



#### Decision Trees.

Decision trees are a widely used models for classification and regression tasks. A set of splitting rules is used to segment the predictor via a hierarchy of “if-else” questions, leading to a decision.


🎋  Nodes : Split the value of attributes.

🌴  Edges : These are outcomes of a split to the next node.

🌲  Root : Node that does the first split.

🍃  Terminal nodes that predict the outcome.

Each node in the tree either represents a question, or a terminal node (also called a leaf) which contains the answer.

The edges connect the answers to a question with the next question you would ask.


#### Random Forests

Random forests incorporates use of many trees with a random sample of features
for every single tree at every single split.

Each time a split in a tree is considered, a random sample of m predictors is chosen as split candidates from the full set of p predictors. The split is allowed to use only one of those m predictors.


For classification, `m`, is typically chosen to be (squareroot of P == m).
(that is, the number of predictors considered at each split is approximately equal to the square root of the total number of predictors )

By randomly leaving out features, random forests *decorellates* the trees providing an improvement over the trees.


Check out this [Decision Tree and Random Forests notebook] working on sample kyphosis dataset - (excessive outward curvature of the spine) among patients

So that was the Seventh week.. 🔏

[Decision Tree and Random Forests notebook]: https://github.com/4bic/deliberate_practice/blob/master/jupyter%20notebooks/Decision-Trees-and-Random-Forests/Decision-Trees_and_Random_Forests_fdns.ipynb

[notebook on fruit classification]:https://github.com/4bic-attic/data_school/blob/data_school/applied_ML-Coursera/module1.ipynb
