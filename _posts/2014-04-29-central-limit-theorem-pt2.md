---
layout: post
title:  "Central limit theorem part II"
date:   2014-04-28 16:12:32
categories: Statistics
---

Here are some practical applications of CLT.

### Metrics

For a common experimental study, metrics of interest include:

- population
    - population mean \\(\mu\\)
    - population standard deviation \\(\sigma\_X\\)
- sample
    - sample mean \\(\bar{x} = \frac{1}{N} \sum x\_i \\)
    - sample standard deviation (SD) \\(s = \sqrt{ \frac{\sum (x\_i - \bar{x})^2}{N - 1} } \\)
- sampling distribution
    - sampling distribution mean \\(\mu\_{\bar{x}} \\)
    - standard error (SE, or sampling distribution standard deviation) \\( \sigma\_{\bar{x}} = \frac{s}{\sqrt{N}} \\)

The only pair of metrics is surly known is sample mean and SD, which could be calculated from collected data. Most of the time we do not know anything about the population. Also, we do not know the sampling distribution. However, if we know CLT is applicable, the standard deviation of the sampling distribution could be estimated by equation mentioned above.

### Calculate confidence interval

To do so we first look at a theorem derived from Chebyshv inequality: for a Gaussian (\\(\mu, \sigma\\)) random variable _X_, a confidence interval estimate of \\(\mu\\) of the form 

$$M\_n (X) - c \leq \mu \leq M\_n (X) + c$$

has confidence coefficient \\(1 - \alpha \\) where

$$\alpha/2 = 1 - \Phi (\frac{c \sqrt{n}}{\sigma})$$

One could see that this theorem is basically setting the lower and upper bound for the true population mean \\(\mu\\). The bounds are decided by _c_, which is further decided by the second equation once \\(\alpha\\) is fixed.

For example, let confidence coefficient be 0.95, which means \\(\alpha = 0.05\\), which is common in practice. Then second equation becomes,

$$0.025 = 1 - \Phi (\frac{c \sqrt{n}}{\sigma})$$

The \\(\Phi()\\) is essentially CDF of a standard normal distribution, and since we have a table to find the input (\\(\frac{c \sqrt{n}}{\sigma}\\)) for every possible result of the CDF, we could then determine the value of _c_, and eventually find out the bounds.

Now, with CLT, we know sampling distribution is _almost_ a normal distribution. Therefore, the theorem could be applied to the sampling distribution. Then the true mean here is the true sampling mean, which is equivalent to the true population mean \\(\mu\\). _SE_ becomes standard deviation. Therefore, we have the confidence estimate as

$$\bar{x} - c \leq \mu \leq \bar{x} + c$$

and has confidence coefficient \\(1-\alpha\\) where

$$\alpha/2 = 1 - \Phi (\frac{c \sqrt{n}}{\sigma\_{\bar{x}}})$$
