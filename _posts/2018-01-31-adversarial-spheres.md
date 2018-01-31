---
layout: post
title: Adversarial Spheres
tags: [machine learning, paper]
---

State-of-the-art computer vision models are vulnerable to adversarial examples, i.e. for every image that is classified correctly you can find very small perturbations which causes the model to misclassify the image. The perturbations are so small that the original image and the modified image are indistinguishable for a human. The reason for that is still poorly understood and remains an open problem in the machine learning community.

To move one step forward some researchers from Google have published a [paper](https://arxiv.org/pdf/1801.02774.pdf) in which they have analyzed a simple synthetic dataset which consists of data points from two concentric high dimensional spheres. The data points of the inner sphere have a distance of 1 to the origin (more precisely the data points have an Euclidean norm of 1), the data points of the outer sphere have a distance of 1.3 to the origin. This dataset was analyzed experimentally (via a ReLU network with two hidden layers and 1000 hidden units) and analytically by using a very simple network (a “quadratic network”, i.e. a network with a single hidden layer with no bias in the hidden layer where the activation function is x²).

The authors found that this synthetic dataset shares properties of current image models. In particular, most randomly chosen points from the data distribution are correctly classified but are very close to an incorrectly classified input. The authors proved analytically that for this dataset _any_ model is vulnerable to adversarial examples if there exist just a small constant fraction of the sphere which is misclassified by the model. Furthermore, they have shown that in order to linearly increase the average distance to the nearest error (which would be the goal if a model should be robust to adversarial examples), the error rate of the model must decrease exponentially. Concretely, if the average distance to an error is denoted by d, then the size of perturbations that is required to create an adversarial example is limited by the upper bound O(1/sqrt(d)).

It is argued that the findings seem to be the result of the high dimensional geometry of the data manifold. For networks that were trained on the same dataset but with lower dimensions no errors were found. However, this changes when the dimension is around 60.

Of course, the results shown in this paper were just derived from a simple synthetic dataset and it is not known whether the results presented here can be applied to image data as well. However, real-world data and models are much more complex and things usually do not get better the more complex things are. So in my opinion we have to consider that with current models for image classification adversarial examples will always exist. Let’s wait and see what that means for future applications like self-driving cars for which adversarial examples could pose a severe risk for humans.
