---
layout: post
title:  "One-way ANOVA"
date:   2014-04-25 13:18:10
categories: HCI
---

According to [wiki](http://en.wikipedia.org/wiki/Analysis_of_variance), analysis of variance (ANOVA) is

- omnibus test
- testing whether three or more means are the same (null hypothesis).
- computing F-statistic or F-ratio
- comparing the ratio of systematic variance to unsystematic variance
- a special case of regression

In summary, in the context of ANOVA, the total variance of the data is splitted into parts explained by the model of group means, and parts cannot be explained by the model; given fixed amount of variance, the more variance a model explains, the better the model is. And ANOVA computes _F-ratio_, which is the ratio of explanable variance over unexplanable variance.

## variance: sum of squares

The most important stuff in ANOVA are two types of means (grand mean and group mean) and three type of variance (total variance, explained variance and unexplained variance). 

A _grand mean_ is just the mean of all the data, and _group mean_s are arithmetic average of each group in your data. With such terms, the alternative hypothesis in ANOVA could be rephrased as it is better to model the data using several group means instead of the grand mean. And the criterion to decide to accept the hypothesis are the ratio of two variances mentioned before.

The three variances are quantified by three **sum of squares** as below:

_total SS_: the sum of deviation (squared) of each single point in the data from the grand mean \\(X\_grand\\)

$$SS\_T = \sum\_{i=1}^{n} (x\_i - X\_{grand})^2 $$

_model SS_: the sum of deviation (squared) each group mean from grand mean times the size of the group

$$SS\_M = \sum\_{j=1}^{k} n\_j (X\_j - X\_{grand})^2 $$

_residual SS_: the sum of deviation (squared) every single point from the corresponding group mean

$$SS\_R = SS\_T - SS\_M = \sum\_{g=1}^{n\_k} (x\_{gk} - X\_k)^2 $$

One problem of such means is that they take into account the group size, which could influence the ratio of variances. Therefore, to compute _F-ratio_, we need average SS, which is the normal SS divided by _degree of freedom_(need to explain, df).

\\(df\\) of \\(SS\_T, SS\_M\\) are: sample size minus one (\\(N-1\\)), number of groups minus one; that for \\(SS\_R\\) is just the difference between that of previous two.

Now we have everything to compute _F-ratio_:

$$F = \frac{MS\_M}{MS\_R} = \frac{SS\_M/df\_M}{SS\_R/df\_R}$$

And each \\(F\\) is associated with a \\(p\\) value to let you know the resutl if significant or not.

Note this is the process of one-way ANOVA. That is, you have a [between-group](http://en.wikipedia.org/wiki/Between-group_design_) variable to split your data into several groups.

## Assumptions

- assumption of normality
- homogeneity of variance
- independent data

ANOVA in general is a pretty robust, in that it is still fairly accurate even if data violates normality or homogeneity of variance. However, this is under the condition that group sizes are equal. If not, the result should not be trusted without any other aid. In addition, the assumption of independence should be met in any case.

In the end, ANOVA is an omnibus test. It only tell you if theres any difference among groups, insead of how groups are different. If you find difference for your groups using ANOVA, you have to conduct follow-up post-hoc tests to discover how exactly the difference is in your groups.

## Reference

[1] A. Field, J. Miles, and Z. Field, Discovering Statistics Using R. SAGE Publications Ltd, 2012.
