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

It is ok that you may not understand how this equation comes, because this example is just to throw out a general problem later. If you are interested in this sparse channel problem, please check <a href = "https://www.researchgate.net/publication/303365519_Compression-Aided_Kalman_Filter_for_Recursive_Bayesian_Estimation_of_Sparse_Wideband_Channels_in_OFDM_Systems">this paper</a>. Back to the equation above, index \\(n\\) is a time notation, and \\(\boldsymbol{Y}[n]\\) is received samples of size \\(n \times 1\\), \\(\mathbf{A}[n]\\) is a matrix of size \\(n\times 1\\), and \\(boldsymbol{h}[n]\\) is a vector of size \\( N\times 1\\). For each time slot, denoted by \\(n\\), our goal is to estimate \\(boldsymbol{h}[n]\\) with knowing \\(\boldsymbol{Y}[n]\\) and \\(\mathbf{A}[n]\\) with condition that \\(n<N \\).


