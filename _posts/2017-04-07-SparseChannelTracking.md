---
layout: post
title: Algorithm：Compress-Aided Kalman Filter 
description: This blog will introduce compress-aided Kalman filter, which is an extension of <a href = "https://en.wikipedia.org/wiki/Kalman_filter">standard Kalman filter</a>, aiming to track dynamic sparse signals. Following the <a href = "https://danqing117.github.io/AMP">previous blog</a>, same example (sparse channel estimation) will be considered and extended.
category: blog
---
<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

This blog will introduce compress-aided Kalman filter, which is an extension of <a href = "https://en.wikipedia.org/wiki/Kalman_filter">standard Kalman filter</a>, aiming to track dynamic sparse signals. Following the <a href = "https://danqing117.github.io/AMP">previous blog</a>, same example (sparse channel estimation) will be considered and extended.

The full code is here.

## Problem Formalism
In the previous blog, <a href = "https://danqing117.github.io/AMP">Algorithm：Approximate Message Passing</a>, we have talked about how to solve an under-determined problem using AMP algorithm. Here, we still solve under-determined problems but with dynamic inputs. Considering sparse channel estimation in frequency domain, and the received OFDM symbols are expressed as:

$$\boldsymbol{Y}[n]=\mathbf{A}[n]\cdot\boldsymbol{h}[n] +\boldsymbol{v}[n]$$

It is ok that you may not understand how this equation comes, because this example is just to throw out a general problem later. If you are interested in this sparse channel problem, please check <a href = "https://www.researchgate.net/publication/303365519_Compression-Aided_Kalman_Filter_for_Recursive_Bayesian_Estimation_of_Sparse_Wideband_Channels_in_OFDM_Systems">this paper</a>. Back to the equation above, index \\(n\\) is a time notation, and \\(\boldsymbol{Y}[n]\\) is received samples of size \\(n \times 1\\), \\(\mathbf{A}[n]\\) is the sensing matrix of size \\(n\times N\\), and \\(\boldsymbol{h}[n]\\) is a vector of size \\( N\times 1\\). Our goal is to track \\(\boldsymbol{h}[n]\\) for each time slot, with knowing \\(\boldsymbol{Y}[n]\\) and \\(\mathbf{A}[n]\\) if \\(n<N \\). Important to note that vector \\(\boldsymbol{h}[n]\\) is sparse.

There are some famous algorithms that can solve this problem, such as <a href = "https://en.wikipedia.org/wiki/Recursive_least_squares_filter">RLS</a>, <a href = "https://en.wikipedia.org/wiki/Kalman_filter">standard Kalman filter</a> and <a href = "http://scikit-learn.org/stable/auto_examples/linear_model/plot_omp.html">OMP</a>. RLS and standard Kalman filter are both recursive algorithms, using previous measurements to predict the current estimate, therefore, they work at real time. As for OMP in this scenario, it is not recursive but only update the estimate using current measurements. For benchmark, the results of these three algorithms will be compared.

## What Is Compress-Aided Kalman Filter?
For the equation in the previous section, we know that \\(\boldsymbol{h}[n]\\) is a sparse vector. **Standard Kalman filter will track all the elements in that vector while compress-aided Kalman filter will only track dominant elements and other elements will be set to zero.** The reason is that tracking small-valued elements in \\(\boldsymbol{h}[n]\\) will give us larger errors and it can also make Kalman filter unstable.

If we only consider the dominant elements in \\(\boldsymbol{h}[n]\\), and denote the position of those dominant element as \\(\boldsymbol{l}\\) = [\\(l_1,l_2,\dots, l_m\\)], then we can rewrite the measurement model as:

$$\boldsymbol{Y}[n]\approx\mathbf{A}_\boldsymbol{l}[n]\cdot\boldsymbol{h}_\boldsymbol{l}[n] +\boldsymbol{v}[n]$$

Therefore, sening matrix \\(\mathbf{A}[n]\\) is of size \\(n\times m\\) and \\(\boldsymbol{h}_\boldsymbol{l}[n]\\) is of size \\(m\times 1\\). Simply you can think that measurement model has been 'compressed'. 

Before we can apply the compress-aided Kalman filter, we need a <a href = "https://en.wikipedia.org/wiki/Autoregressive_model">autoregressive(AR)</a> model of \\(\boldsymbol{h}[n]\\) which is a priori information (RLS do not require it). It is expressed as following:

$$\hat{\boldsymbol{h}}_{\boldsymbol{l}}[n] = \boldsymbol{F}\hat{\boldsymbol{h}}_{\boldsymbol{l}}[n-1]+\boldsymbol{w}[n]$$ 

This is an estimated model wich is can be constructed by applying the Yule-Walker equation. Here, matrix \\(\boldsymbol{F}\\) is an diagonal matrix. 

With measurement model and state model in hand, we can implememt compress-aided Kalman filter as following:

1. Initialization: 

$$n = 0, \hat{\boldsymbol{h}}_{\boldsymbol{l}}[-1|-1] = \boldsymbol{0}_{s \times 1}, \boldsymbol{C}[-1|-1] =\boldsymbol{0}_{s\times s}$$
2. Repeat following

I recomend to read reference [1] to fully understand how Kalman filter works. In the prediction step, the AR-1 state model is applied to predict the channel taps in the next iteration using the estimated channel taps in the current iteration, then the prediction covariance matrix \\(\boldsymbol{C}[n|n-1]\\) and Kalman gain \\(\boldsymbol{K}[n]\\) are computed. The larger Kalman gain indicates that more correction is needed.

# Numerical Results
I compared compress-aided Kalman filter with RLS, OMP and least suqares(LS) over tracking \\(\boldsymbol{h}[n]\\) with different level of variation (denoted by \\(\alpha\\), larger \\(\alpha\\) indicates larger variation). Measurement noise level is 20 dB, and sparsity parameter \\(\frac{n}{N} = 0.12\\). The following figures show the tracking performance of one tap. 




