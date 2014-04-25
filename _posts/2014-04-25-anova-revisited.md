---
layout: post
title:  "Anova revisited"
date:   2014-04-25 23:10:10
categories: HCI
---

Analysis of Variance (ANOVA) is

- omnibus test
- testing whether three or more means are the same (null hypothesis).
- computing F-statistic or F-ratio
- comparing the ratio of systematic variance to unsystematic variance
- a special case of regression

ANOVA is basically comparing the model built with the additional effect of variables (between-group or within-subject) with the basic model of total mean.

Note here we only discuss one-way ANOVA, in which only a between-group variable is involved.

## one-way ANOVA

Let's begin with an example study. The study is about typing speed people has when they use different devices. We recruited and group people into three groups, each group uses a different device (computer, smartphone, tablet). Apparently the between-group variable here is type of device. Each participant is asked to type the same content with the given device. The speed of the typing is recorded.

Therefore, in the experiment, we have three groups. We set computer group as the control group. With dummy codes we could describe the relationship between typing speed of each participant and their devices as:

    speed_i = b0 + b1*tablet_i + b2*smartphone_i + error_i

`b0` is the intercept, which is the speed of typing on computer. Variables `tablet_i` and `smartphone_i` each add one effect on the speed value. Finally we have an error term `error_i`.

Each participant's expriment setup could be determined by dummy codes as follows:

<table>
<th>
<td>tablet_i</td>
<td>smartphone_i</td>
</th>
<tr>
  <td>Computer group</td>
  <td>0</td>
  <td>0</td>
  </tr>
<tr>
  <td>Tablet group</td>
  <td>1</td>
  <td>0</td>
</tr>
<tr>
  <td>Smartphone group</td>
  <td>0</td>
  <td>1</td>
</tr>
</table>

For example when both variables are 0, the equation becomes

    speed_i = b0 + error_i
    X_computer = b0

This value of speed would be that of computer group, i.e. the control group.

And when `tablet_i` is 1 and `smartphone_i` is 0,

    speed_i = b0 + b1 + error_i
    X_tablet = X_computer + b1
    b1 = X_computer - X_tablet

Therefore, `b1` stands for the effect of tablet added to the value of speed in addition to the default `b0` of control group.

Now we have the general model, we proceed to the measurement in ANOVA. What is ANOVA comparing and looking for?

Generally speaking, ANOVA is stating that, for a population of people, with each participant has a value, instead of using the total mean, dividing the them into several groups and using the group means is better to describe this entire population. And it is using the sum of square value to measure if the statement is true or not.

Therefore, for our case, we have two models. One is the average typing speed of the entire experiment; the other one is three means indicating the average speed of three groups differed by the device used. Like I said, ANOVA uses the sum of error to compare two models. If the group-mean model has a much smaller sum of error, then we could safely say that our model is good enough.

The official name for sum of error is _sum of squares (SS)_, which is the square of the deviation of the observation (collected data) from the model (means).

We set the total mean of entire experiment as `X_grand`, i.e. grand mean. Then for the basic model we have

![Total sum of squares]({{ site.url }}/images/anova/ss_grand_mean.jpg)

This is called **total sum of squares**. It tells you how many errors/variances in the data overall. The objective of ANOVA model or any other models is to "account for" as much errors/variances as possible.

For our ANOVA model, it has three group means `X_k, (k=1,2,3)`. Then we have a different sum of squares,

![Model sum of squares]({{ site.url }}/images/anova/ss_model.jpg)

`SS_M` is called the **model sum of squares**. `n_k` is the number of observations each group has. This is the amount of errors/variances our model could "account for". Like I said, it should be as big as possible. The bigger it is, the better the model is.

With two sum of squares, we have then

![Residual sum of squares]({{ site.url }}/images/anova/ss_residual.jpg)

`SS_R` is called **residual sum of squares**, which means the errors/variances remaining that our model could not explain. It could be calculated by either removing `SS_M` from `SS_T`, or start from scratch to calculate the variance of each participant (`x_ik`) from their group mean `X_k`.

Ok, now we have our three sum of squares. To compute the _F-ratio_ we want from ANOVA, we have one more process to go. While the sum of squares tell us the amount of erros/variances each part has, the values themselves are influenced by the number of scores they used, therefore we cannot compare them directly. To eliminate the bias, we need to calcualte the **mean squares**, which is sum of squares divided by **degree of freedom**. We do not divide by number of scores for each sum of squares because we are trying to predict the population.

For `SS_T`, the degree of freedom would be one less than the total sample size (`N-1`); for `SS_M`, it is one less than the number of parameters/groups (`k-1`). For `SS_R`, it is that of `SS_T` minus that of `SS_M`.

Ok finally then to compute F-ratio from mean squares

![F-ratio by mean squares]({{ site.url }}/images/anova/f_ms.jpg)

So basically, `F` is the ratio of variances explained by the model and by unsystematic factors. If it is less than 1, our model must result in a non-significant effect; if it is bigger than 1, then it means our model explains more variances than mysterious unsystematic factors do, therefore it is a good enough model for the data.

### Assumptions

- assumption of normality
- homogeneity of variance
- independent data

ANOVA in general is a pretty robust, in that it is still fairly accurate even if data violates normality or homogeneity of variance. However, this is under the condition that group sizes are equal. If not, the result should not be trusted without any other aid. In addition, the assumption of independence should be met in any case.

In the end, ANOVA is an omnibus test. It only tell you if theres any difference among groups, insead of how groups are different. If you find difference for your groups using ANOVA, you have to conduct follow-up post-hoc tests to discover how exactly the difference is in your groups.
