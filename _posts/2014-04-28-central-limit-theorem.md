---
layout: post
title:  "Central limit theorem in experimental study"
date:   2014-04-25 16:12:32
categories: Statistics
---

Central limit theorem is said to be very very very important to probability problems, and is one of the foundations of experimental study.

Prior to the post I would just like to mention all Latex-like equations below are rendered by [MathJax](http://mathjax.org), "an open source JavaScript display engine for mathematics that works in all browsers".

It is really easy to use, one needs just to add the script in the html file, and then directly write Latex equations with delimiters like `$$`. Detailed instructions on how to write such equations could be found in the [documentation](http://docs.mathjax.org/en/latest/start.html).

## Central Limit Theorem

The formal definition of the theorem[1] is, given a sequence of _iid_ random variables with expected value \\( \mu\_X \\) and variance \\(\sigma^2\_X\\), the CDF of \\(Z\_n = (\sum\_{i=1}^n X\_i - n \mu\_X)/\sqrt{n\sigma^2\_X}\\) has the property

$$\lim\_{n \rightarrow \infty} F\_{Z\_n} (z) = \Phi (z)$$

This is essentially talking about a new random variable \\( W \\), where \\(W = \sum\_{i=1}^n X\_i \\).

According to the book, \\(W\\) could be arbitrary combination of multiple \\(X\\), and the property could thusly result in different forms. In general, the theorem says that, as \\(n\\) increases, the CDF of \\(W\\) resembles more and more like a Gaussian CDF.

More formally, with large enough \\(n\\), CDF of \\(W\\) becomes,

$$F\_{W\_n} (w) = P[W \leq w] = P[ \sqrt{n\sigma^2\_X} Z\_n + n \mu\_X \leq w] = F\_{Z\_n}(\frac{w - n\mu\_X}{\sqrt{n\sigma^2\_X}}) \approx \Phi (\frac{w - n\mu\_X}{\sqrt{n\sigma^2\_X}}) $$

## Experimental study

Now the math equations are not hard to understand, but how do they relate to practices, and in particular, experimental study?

Lets use an experimental study as an example. Say I want to study the difference of typing speed of three difference keyboards A, B and C. We go ahead and recruit \\(n\\) participants for each group.

In this experiment, we have three populations, each of which could be defined as _typing speed of people who use keyboard A, B or C_. To simplify the explanation, we only focus on one population, say people who use device A. All the participants we recruited for group A could be considered as random variables (i.e. \\(X\_i\\)) drawn from the same population. This group of random variables is called a **sample**, and all its memebers share the same population mean \\(\mu\_X\\) and population variance \\(\sigma^2\_X\\).

On the other hand, for this particular sample with **sample size** as \\(n\\), we have a sample mean \\(\mu\_A = \frac{1}{n} \sum\_{i=1}^n X\_i \\), which is known to us.

Now, say we also calculated the sample mean for other two groups (people who use device B and C) to be \\(\mu\_B\\) and \\(\mu\_C\\), respectively. And we use some methods to compare three sample means. The result shows they are different. Great. What is the conclusion?

So far, our conclusion is, _for our samples_, the average typing speed of people who use device A, B and C is different across the groups. However, our objective is to have a conclusion on the _entire population_ of people who use such devices. Put it another way, we want compare \\(\mu\_X\\) of three populations, not \\(\mu\_A\\). Howw could we do that? Population means are unknown.

This is where CLT comes to rescue. One could find that the sample mean is the sum of all random variables with a constant: \\(\frac{1}{n} \sum\_{i=1}^n X\_i = \frac{1}{n} W\\). According to CLT, we have,

$$F\_{W\_n} (w) = F\_{Z\_n}(\frac{w - n\mu\_X}{\sqrt{n\sigma^2\_X}}) \approx \Phi (\frac{w - n\mu\_X}{\sqrt{n\sigma^2\_X}}) $$

It means, given a large enough sample size \\(n\\), the value of \\(\frac{1}{n}W\\) is a distribution with mean \\(\mu\\) and variance of \\(\sigma^2\_X\\), _approximately_. That is a lot like the population!

To rephrase in non-math style, if we recruit many people to do our experiment, the conclusion we draw based on participants data could also be applied to the population where we draw participants from.

Such conversion is called **generability**. It is crucial in experiment study because it is mostly definitely impossible to measure the typing speed of the entire population, while with CLT we have the chance to infer that of the entire population by appripriate sample.

Two additional bonus of CLT: 

1. like I mentioned, \\(W\\) could be arbitrary form of \\(X\\)s, not just their sum;
2. it stands correct no matter what distribution the _sample_ has, as long as you have a large sample size.


## Sample size

Noth though, things are tricky for (2). What size is large enough for the sample? I don't know for now. In many current literature, \\(n \geq 30\\) is a rule of thumb to apply CLT; but more and more doubts are casting on this magic number. The determination of the sample size would be one of my future topics. 

One applicable rule for the sample size is, if the sample data mostly follows a normal distribution, CLT could be applied even if the sample size is very small. On the other hand, larger sample size is needed if the sample distribution is further from a normal distribution. This raises another question: what to do if we have a sample, that is neither normal distribution at all nor having a large sample size?

## Reference

[1] Yates, R., & Goodman, D. (2004). Probability and stochastic processes. John Wiley & Sons, Inc. Experimental Study of Comprehension on the World Wide Web, Information Processing and Management, 36, 607-621.
