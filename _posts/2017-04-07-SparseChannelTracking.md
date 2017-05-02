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
In the previous blog, <a href = "https://danqing117.github.io/AMP">Algorithm：Approximate Message Passing</a>, we have talked about how to solve an under-determined problem using AMP algorithm. Here, we still solve under-determined problems but with dynamic inputs. Considering sparse channel est             imation in frequency domain, and the received OFDM symbols are expressed as:

$$\boldsymbol{Y}[n]=\mathbf{A}[n]\cdot\boldsymbol{h}[n] +\boldsymbol{v}[n]$$

It is ok that you may not understand how this equation comes, because this example is just to throw out a general problem later. If you are interested in this sparse channel problem, please check <a href = "https://www.researchgate.net/publication/303365519_Compression-Aided_Kalman_Filter_for_Recursive_Bayesian_Estimation_of_Sparse_Wideband_Channels_in_OFDM_Systems">this paper</a>. Back to the equation above, index \\(n\\) is a time notation, and \\(\boldsymbol{Y}[n]\\) is received samples of size \\(n \times 1\\), \\(\mathbf{A}[n]\\) is the sensing matrix of size \\(n\times 1\\), and \\(\boldsymbol{h}[n]\\) is a vector of size \\( N\times 1\\). Our goal is to track \\(\boldsymbol{h}[n]\\) for each time slot, with knowing \\(\boldsymbol{Y}[n]\\) and \\(\mathbf{A}[n]\\) if \\(n<N \\). Important to note that vector \\(\boldsymbol{h}[n]\\) is sparse.

There are some famous algorithms that can solve this problem, such as <a href = "https://en.wikipedia.org/wiki/Recursive_least_squares_filter">RLS</a>, <a href = "https://en.wikipedia.org/wiki/Kalman_filter">standard Kalman filter</a> and <a href = "http://scikit-learn.org/stable/auto_examples/linear_model/plot_omp.html">OMP</a>. RLS and standard Kalman filter are both recursive algorithms, using current measurements and sensing matrix to esti




