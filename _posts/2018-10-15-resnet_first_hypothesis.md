---
layout: post
title: A modified ResNet architecture with nice optimization properties
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
f(x, W) = W_{2}^{T}ReLU((W_{1})^{T}x) + x 
\end{equation}
$$

After all, the authors of the paper motivated their architecture by comparing it to ResNet and showing that it performs compariably.  So, why not study the original?  It turns out that nothing as convenient as the results they showed for their architecture is true for one-block ResNets:

<p style="text-align:center;">
<img src="{{site.url}}/images/resnet.png" width="300" alt="resnet">
</p>  


Somehow, though, Li et al.'s architecture seems to be very close to ResNet.  It seems strange that we are unable to see similar results.  So, I thought, the next thing that might be reasonable to do is try to adjust the architecture to make it as close to ResNet as as possible without losing the good optimization properties.

So I turned my attention to the following architecture (where sum is taken over elements of the resulting vector):

$$
\begin{equation}
f(x, W) = sum(W_{2}^{T}ReLU((W_{1} + I)^{T}x))
\end{equation}
$$


We add the $$W_{2}$$ in front because ResNet has the same.  What I found was that the training error was able to reach zero (with Nesterov acceleration added this time; it converges to zero with normal SGD, just slower).  Note here that this time, we are talking about the training loss, not the distance between parameters this time.  This is because actually, the parameters do not recover the ground truth; but the parameters that we do recover give us zero training error.  This suggests that the local minima (at least the reachable ones) are also global minima.

<p style="text-align:center;">
<img src="{{site.url}}/images/yuanzhiTwoWeight.png" width="300" alt="resnet">
</p>  

When it became apparent that this architecture had nice optimization properties, it was time to make the architecture even closer to a Resnet block.  So I looked at the following (note the one-norm or sum isn't there anymore): 

$$
\begin{equation}
f(x, W) = W_{2}^{T}ReLU((W_{1} + I)^{T}x) + x
\end{equation}
$$

Note that this architecture has the same exact expressive power as a ResNet: they are functionally equivalent.  However, in terms of optimization, they are different.  For this modified ResNet, we saw a similar result as the architecture we tried out right above.  The training error went to zero, but we still do not recover the ground truth, suggesting that the local minima are global minima as well.

<p style="text-align:center;">
<img src="{{site.url}}/images/modresnet10.png" width="300" alt="resnet">
</p>  

## Conclusion
Now that we've managed to find an interesting phenomenon, it opens the door to do some foundational research.  Why does the modified ResNet have these local minima that get zero training error?  Intuitively, it must be the case that these different global minima correspond to the ground truth parameters, up to some transformation.  This [paper](https://arxiv.org/pdf/1711.00501.pdf) investigates a something that is similar to this, so I think we might try to use some elements of their analysis if it is applicable.  I'm excited to see where the investigation takes us, and hopefully we can get a good, precise characterization soon!  The code that produced these results is [here](https://github.com/houcharlie/resnet_convergence).