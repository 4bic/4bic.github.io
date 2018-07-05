---
title: week eight - Support Vector Machines.
bigimg: /img/wk-8.jpeg
comments: true
tags: [Support Vector Machines, Maximal Margin Classifier, GridsearchCV]
---
By definition; Support vector machine (SVM) is an extension of the support vector classifier that results from enlarging the feature space in a specific way, using kernels. 📖

### What new skills have you learned?

📦   Support Vector Machines

#### Support Vector Machines
Often referred to as SVM. SVM's are supervised learning models with associated learning algorithms that analyze data and recognize patterns mainly used for classification and regression analysis.

🔅 An SVM model is a `representation of points` in space mapped so that categories are divided by a clear gap that is as wide as possible.

💥  Mapped points in the same space are predicted to belong to the same category.


##### Determinant Components in SVM's

-   Choose a `hyperplane`-(A flat affine subspace of dimension p - 1), that maximizes the margins between classes. The vector points that the margins lines touch are called *support vectors*

📌  C : A low C value equates to low Bias - high Variance

📌  gamma: small gamma results to gaussian with large Variance
    -   High gamma results to high Bias-low Variance.

Sample notebook with [iris dataset classification using SVM]


So that was the Eighth week.. 🔏

[iris dataset classification using SVM]:https://github.com/4bic/deliberate_practice/blob/master/jupyter%20notebooks/Support-Vector-Machines/svm_project.ipynb
