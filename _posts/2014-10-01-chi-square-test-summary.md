---
layout: post
title:  "Chi-square test summary"
date:   2014-10-01 17:00:00
categories: Statistic
---

In my project I need to analyze a bunch of categorical data. Most of them are binary measure, for example whether a person recalled the password or not, for two groups. To compare two groups of binary measures, chi-square test is the one to go.

Apparently Chi-square test is a category of statistical tests. Here we talk about Pearson's Chi-square test in specific.

### Chi-square distribution

The "squared" random variable whose distribution is normal is a chi-square distribution. For example, if \\(X \sim N(0,1)\\), then \\(X^2\\) has a chi-square distribution.

Chi-square distribution has another variable that is degree of freedom (df). As df increases, chi-square distribution changes, as in figure below (source:).

![Chi-square distribution]({{ site.url }}/images/Chi-square.png)

Formally, chi-square has been defined as 

$$Q = \sum\_{i=1}^{k} X^2\_i $$

where k is the degree of freedom, and \\(X \sim N(0,1)\\). It seems normal distributions are somehow additive in this definition.

### Chi-square test

Chi-square tests could be easily described by a __contingency table__. Let's make an easy example. Say for one experiment you recruited 92 participants, 48 Male and 44 Female. Now you need to show that the gender ratio in your sample is not biased. We could achieve that by performing a chi-square test.

A contingency table describes both observed and expected counts in each category. An unbiased ratio of gender would be of course 50 to 50. Therefore a contingency table looks like this

				Male	Female
	Unbiased	50		50
	Yours		48		44

Now running chi-square test tells you whether the unbiased ratio and your ratio are similar. This is also called a goodness-of-fit test. The null hypothesis is the gender ratio of your sample is similar to that of an unbiased sample.

For illustrative purposes, we also add up each row and each column in the table, and now it becomes

					Male	Female	Row Total
	Unbiased		50		50		100
	Yours			48		44		92
	Column total	98		94		
	
Now that the table is ready, let's take a look at the test statistic, \\(\chi^2\\). Chi-square test looks at the deviation from the model to observed value:

$$\chi^2 = \sum \frac{(observed\_{ij} - model\_{ij})^2}{model\_{ij}} $$

Now that values are not continuous, but categorical, therefore we look at "frequency" instead. \\(observed\\) is just the count of observation in each category. \\(model\\) is slightly complicated. It is defined as the "expected frequency": assuming the model (the independent variable) does not work, what frequency should we expect for each category? Formally, we have

$$Model\_{ij} = \frac{rowTotal\_{i} \times columnTotal\_{j}}{n} $$

As you can see, the expected frequency of the model is the count of \\(i\\)th row times that of \\(j\\)th column divided by total sample size \\(n\\). The names probably do not make much sense. In our example, "row total" indicates the total number in each sample, and "column total" indicates the expected gender ratio, assuming unbiased and yours are from the same distribution. Therefore the multiplication of the two is the expected number of a particular gender given one sample size. 

The exact calculation for \\(model\_{11}\\) would look like this:

$$ model\_{11} = (98 \times 100) / 192 = 51.04 $$

Do the same for all cells in the table, we get four expected frequencies. Then, we plug them into the function to get the chi-square statistic:

$$ \chi^2 = \frac{(50 - 51.04)^2}{51.04} + \frac{(50 - 48.96)^2}{48.96} + \frac{(48 - 46.96)^2}{46.96} + \frac{(44-45.04)^2}{45.04} = 0.09$$

Now that we have the chi-square statistic. we need another metric that is degree of freedom. It is straightforward: \\(df = (r-1)(c-1)\\), where \\(r\\) is the number of rows in the table, and \\(c\\) is the number of columns. In our case, df is 1.

One more thing we need is the critical probability, \\(p\\), a.k.a. alpha value. It is read as the probability we obtain such data given the null hypothesis is true. Generally we set it as \\(.05\\). Now with the probability and df, we are able to look for the critical chi-square value in the chi-square table, such as [this one](http://sites.stat.psu.edu/~mga/401/tables/Chi-square-table.pdf). 

If our chi-square statistic is equal or larger than the critical value, we are able to reject the null hypothesis. Because the probability for us to obtain such value is less than \\(.05\\) given it is true, which is very unlikely.

For our case, the critical value of \\(\chi^2\\) is \\(7.879\\), which is drastically large than our calculated value. This means we cannot reject the null hypothesis. Therefore, our conclusion is the gender ratio of our sample is unlikely very different from the unbiased ratio.

### Assumptions

There are two major assumptions the data has to hold in order to use chi-square test.

First, data point should be independent of each other. Basically, if you have a binary measure with two possible outcomes, then the result of one subject in your study or data should fall into only one outcome out of two. 

Second, if in the contingency table, there are cells with values that are too small, results are not reliable anymore. This criteria is defined as the minimum expected frequency of the table should be at least 5.



