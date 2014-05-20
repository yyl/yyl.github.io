---
layout: post
title:  "Repeated-measures ANOVA"
date:   2014-05-19 20:27:00
categories: Statistics
---

A repeated-measures ANOVA is an ANOVA model with a within-subject variable. A whtin-subject variable is the experiment design where the same participant receives different treatments.

The primary criteria is still Sum of Squares (SS) and F-statistics.

## SS

In repeated-measures ANOVA, the total SS is divided into two parts: within-subject SS (\\(SS\_W\\)) and between-subject SS (\\(SS\_B\\)). Here we mostly focus on \\(SS\_W\\) because our model is only trying to explain the within-subject variance:

$$SS\_W = \sum\_{i=1}^{n} (n\_i - 1)s^2\_{subject\_i} $$

Here \\(n\_i\\) stands for the number of treatments subject \\(i\\) receives, \\(s^2\_{subject\_i}\\) is the standard deviation within subject \\(i\\).

The model SS \\(SS\_M\\) is similar to one-way ANOVA, where it is defined as the distance from group mean to grand mean:

$$SS\_M = \sum\_{j=1}^{K} n\_j (X\_j - X\_{grand})^2 $$

\\(K\\) stands for the number of groups in within-subject variable, and \\(X\_j\\) is the group mean for group \\(j\\).

Finally, residual SS is the amount subtracts model SS from within-subject SS:

$$SS\_R = SS\_W - SS\_M$$

In fact, \\(SS\_T\\) is the same one as in one-way ANOVA, but now we have

$$SS\_T = SS\_B + SS\_W$$

Therefore, (1). repeated-measures model is not trying to explain the total variance; (2). we could calculate \\(SS\_B\\) based on \\(SS\_T\\) and \\(SS\_W\\).

## degree of freedom

The total degree of freedom \\(df\_T\\) is still the number of samples minus one (\\(N-1\\)).

The model \\(df\_M\\) is also the number of groups in within-subject variable minus one.

The within-subject \\(df\_W\\) is \\(df\_M\\) times the number of subjects.

The residual degree of freedom is then

$$df\_R = df\_W - df\_M$$

## F

F-statistic in repeated-measures ANOVA is the ratio of model-explained variance and residual from within-subject variance:

$$F = \frac{SS\_M/df\_M}{SS\_R/df\_R}$$
