---
layout: post
title:  "Introduction To Robust Statistics"
date:   2023-07-19 17:25:53 -0600
categories: jekyll update
---

Learning statistics in the presence of adverserial data is an important problem with applications in machine learning and data science. I introduce robust statistics, sample problems in the field, and some solutions we currently have. I focus on the problem of robust mean estimation in high dimensions and present an overview of a 2017 paper titled "Being Robust (In High Dimensions) Can Be Practical". 

# Background

* data matrices, data distributions, basic stats
* pca, svd, eigenstuff intuition

# What Is Robust Statistics?
Robust Statistics is a field of study that aims to learn statistical information about data in the presence of outliers. This has many important applications. Outliers may be present in data as a result of less refined data collection or even adversarial attacks. This has an important effect on any field using data as it leads to flawed statistics about the data. Improvements in robust statistics may be applied to make machine learning and data science methods perform better in the presence of outliers. For example, we might be able to train meaningful models with less refined datasets. Another application is mitigating the effects of adversarial attacks in which flawed training data is deliberately inserted into the training pipeline of a model to reduce performance.

A simple example of a problem in robust statistics that is surprisingly difficult is estimating central tendancy and dispersion. The standard way to calculate these quantities is by using the mean and variance (or in higher dimensions covariance). The mean is not robust to outliers. If you were to insert data points that deviate significantly from the true mean, the mean could be shifted significantly. On the other hand, the median is robust to outliers. Even if you inserted points with values that deviate significantly, the median won't change much as long as not too many points are added. Likewise, the interquartile range is a robust analog to the variance. As a result, in one dimensions, mean and variance estimation are solved problems, as we can simply use the median and interquartile range instead.

In higher dimensions, this runs into problems. The Tukey median and minimum volume ellipsoid are higher dimensional generalizations of the median and interquartile range. Although these are robust to outliers, they are computationally impractical to compute. They would be impossible to calculate on even very small datasets by today's standards. As a result, we must come up with other methods to estimate central tendency and dispersion that are robust to outliers. 

* Introduce the problem and motivate why it is important.
* Present some basic history and examples: Tukey & Huber -> median iq range to generalization and issues
* Problems and goals


# The Paper
In 2017, two papers were published concurrently and indendently which focus on the problem of mean and covariance estimation in higher dimensional data. Both paper's provide essentially the same algorithm [fact check], but I focus on "Being Robust (In High Dimensions) Can Be Practical". This paper proposes a practical algorithm for robust mean and covariance estimation of higher dimensional data that performs well regardless of the dimensionality of the data.

Particularly, this paper focuses on the problem of mean and covariance estimation of Gaussian data. I focus on the problem of mean estimation with identity covariance, which the paper focuses on. The paper extends this general framework for covariance estimation and mean estimation with unknown covariance, although the practical results still need work. I will first establish what the problem is better, then how the paper's algorithm works, and finally some results and limitations.

Imagine that there are n data points drawn from a d-dimensional multivariate Gaussian distribution with identity covariance. We then allow an adversary to replace an epsilon-fraction of our data points any way that it wishes. It may move points out arbitrarily far, overlap points, anything that it wants. Additionally, the adversary has full knowledge of the good data and of the underlying distribution so it may tactically place the outliers that would shift the mean the most while being hardest to detect. This is a powerful model of an adversary, so any algorithm that performs well against such a powerful opponent should perform just as well or better with less powerful opponents. We are then given this corrupted data, and have no knowledge of the underlying distribution that the "good" data comes from (aside from the fact that it has identity covariance - this assumption is crucial).

The algorithm works by iteratively filtering out outliers and eventually returning the sample mean of the pruned data. It is broken up into two steps: the detection step and the filtering step. In the detection step, we determine if the data has been meaningfully corrupted (i.e. the sample mean would be far from the true mean). If it has not been, we say that the data is good and return the sample mean. If if has been, we move on to the filtering step. In the filtering step, we classify outliers and then remove them. We repeat this algorithm until the data has been deemed good in the detection step, ultimately returning the sample mean. 

If outliers are to change the sample mean specifically, they must move it a specific direction and be concetrated in a certain direction. For example, if the true mean is 0 and we add an equal number of "outliers" at -1000 and 1000, the sample mean will remain approximately 0. This simple idea fuels the intuition behind how we use the top eigenvector and eigenvalue of the covariance matrix in these steps.

First, we center the data using the sample mean and scale it by sqrt(n), where n is the number of data points. We center it so that the eigenvalues and eigenvectors of the covariance will encode information on the variance of the data. We scale it by sqrt(n) so that these values are not dependant on data size. We then perform Singular Value Decomposition on this centered data matrix. From this, we derive the top eigenvalue and eigenvector of the covariance matrix as the top singular value squared and the top right singular vector. I will use these quantities in the following steps. Call the top eigenvalue lambda and the top eigenvector v from now on.

Detection Step:
If lambda is below a certain threshold, we say that the data is good and return the sample mean. If it is above the threshold, we move on to the filtering step. The intuition behind this is that lambda captures the variance of the data in the direction in which the data varies most. With identity covariance, we expect lambda to be 1, since variance would be 1 in any given direction. The paper uses the threshold 1 + 3 * O(epsilon log(1 / epsilon)). 

Filtering Step:
We calculate the projections of every data point onto the top eigenvector. We expect the projections to all be relatively small. We then filter out the points with the highest thresholds. We are able to utilize the properties of a Gaussian with identity covariance so that we don't just naively filter out the top fraction of points. The projection of Gaussian data onto a vector will also be Gaussian. We are able to bound the percentage of points that exceed a certain value of a Gaussian. Using this, we find a projection such that we have more points greater than this projection than we expect (with some slack). We then remove all points whose projection is greater than or equal to this. The intuitino behind this is that the top eigenvector points in the direction in which the mean is shifted. This is the direction in which the outliers have the most effect and are therefore located furthest from the sample mean. As a result, their projections onto v will be large.

* Pictures
* Code
* Algorithm from paper in LaTeX

# Extending These Results

* Discuss unknown covariance
* Discuss covariance estimation

# Problems

* Discuss low data size problem

