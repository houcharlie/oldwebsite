---
layout: post
title: Some first stabs at understanding ResNets
permalink: /blog/:title
---

This is an investigation inspired by this [paper](https://arxiv.org/pdf/1705.09886.pdf) which gave the first detailed convergence analysis of a neural network architecture with ReLU activations without certain independence assumptions that are not seen in practice.  In a few words, Li et al. analyzed the following architecture:
$$
\begin{equation}
f(x, W) = \| ReLU((I + W)^{T}x) \|_{1}
\end{equation}
$$

They did this by analyzing the gradient of the expected loss funciton between their student network and a teacher network (which generates the samples) of the same architecture.  They found that this gradient could be expressed in a way that showed that it starts off nonconvex but becomes one-point strongly convex with respect to the ground truth, given that the ground truth and the initialization of weights in the student network have low Frobenius norm.  Thus, SGD recovers the ground truth provably in this particular architecture, which has a ReLU activation.

I reproduced one of their experiments and found similar results
<p style="text-align:center;">
<img src="{{site.url}}/images/yuanzhi.png" width="300" alt="yuanzhi">
</p>  

In this experiment, we can see SGD smoothly making the error between the student weights and teacher weights approach zero.

Obviously, one thing that we might wonder after their work is whether or not this works with one-block ResNets.  Recall one-block ResNets are the following architecture (we take the one-norm to make it consistent with Li et al.'s architecture):

$$
\begin{equation}
f(x, W) = \| W_{2}^{T}ReLU((W_{1})^{T}x) + x \|_{1}
\end{equation}
$$

After all, the authors of the paper motivated their architecture by comparing it to ResNet and showing that it performs compariably.  So, why not study the original?  It turns out that nothing as convenient as the results they showed for their architecture is true for one-block ResNets:

<p style="text-align:center;">
<img src="{{site.url}}/images/resnet.png" width="300" alt="resnet">
</p>  

This is the same plot but with one-block ResNet student and teacher networks.  As training converges to the end, it clearly hits a local minimum and does not recover the ground truth.

Next, I tried using Li et al.'s architecture as a student network on a one-block ResNet teacher network.  The results are as follows (this time, we plot loss instead of distance, as there's no notion of distance between one weight vs two weights):

<p style="text-align:center;">
<img src="{{site.url}}/images/yuanzhiOnResnet.png" width="300" alt="initialization">
</p>  

Li et al.'s architecture doesn't manage to fit the one-block ResNet super well.  The next thing I tried was use the weights Li et al.'s architecture to initialize a one-block ResNet.  This was inspired from this [paper](https://arxiv.org/pdf/1711.02226.pdf) which proposed using a optimizing a convex relaxation of transformation learning and then using the results to optimize the true transformation learning problem, which performed very well in their context.  The reasoning here is that perhaps Li et al.'s architecture could be considered a relaxation of ResNets.  The result after using the weight in Li et al's architecture in both the ResNet weights are as follows:

<p style="text-align:center;">
<img src="{{site.url}}/images/resnetafterinitial.png" width="300" alt="relaxation">
</p>  

Here, we find another local minimum and don't manage to recover the ground truth.  In fact, we get farther away from the ground truth!

## Conclusion

My previous hypotheses about the problem weren't correct and it appears I won't be able to straightforwardly extend Li et al.'s results to understand ResNets better.  However, I think perhaps we can still pursue the idea of relaxation in Hashimoto et al.'s paper, as Li et al's architecture does seem somewhat close to a one-block ResNet.  If we want to think down this path, though, we need to think of the right notion of "relaxation", as it is not super clear how to go from one weight to two weights.