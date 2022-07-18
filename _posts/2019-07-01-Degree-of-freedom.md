---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: English_all you need to know about degree of freedom 
title: All you need to know about degree of freedom in statistics

# post specific
# if not specified, .name will be used from _data/owner.yml
author: Amrit Koirala
# multiple category is not supported
category: Statistics
# multiple tag entries are possible
tags: [degree of freedom, expected value, sample variance, bias]
# thumbnail image for post
img: ":degree-of-freedom_files/figure-markdown/postImage.svg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2019-07-1 08:11:06 +0900

# seo
# if not specified, date will be used.
meta_modify_date: 2019-07-01 08:11:06 +0900
# check the meta_common_description in _data/lang/[language].yml
meta_description: "Most simplified and complete description of degree of freedom as used in statistics "

# optional
# if you enabled image_viewer_posts you don't need to enable this. This is only if image_viewer_posts = false
#image_viewer_on: true
# if you enabled image_lazy_loader_posts you don't need to enable this. This is only if image_lazy_loader_posts = false
#image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false
---
{%- comment -%} Please delete below and place your page content here {%- endcomment -%}

{%- include util/auto-content-generator.liquid -%}

### Background
<!-- outline-start -->
Degree of freedom (df) is a very important mathematical concept which is
implemented in multiple disciplines like mechanics, physics, chemistry,
and also in inferential statistics. In its essence, it is the number of
independent variables or parameters of a system.
<!-- outline-end --> Or it is the number of
independent quantities necessary to express the values of all the
variable properties of a system. For example a point moving in a 3D
space has degree of freedom equal to 3 because three coordinate values
are necessary to define the state of that point at any given time.
However, if we put a constrain on the system and fix one of the axis at
a constant, then the point is free to move along only two axis and has
only 2 degree of freedom.

### Df in statistics

Df is extensively applied in inferential statistics where we are trying
to estimate some population parameters (mean, variance, skewness, and
kurtosis) using the measurement made in a random sample from that
population (sample statistics). For any estimator of population
parameter, degree of freedom is equal to number of independent pieces of
information that go into the estimate of that parameter or alternatively
it is number of variables in a statistic minus the number of estimated
parameters used while computing that statistic.

These definitions may look vague but lets see how two most commonly used
statistics: sample mean ($$\bar x$$) and sample variance($$s^2$$) works
and come back to these definitions.

### Goal of inferential statistics

One of the main goal of statistics is to estimate the population
parameters like population mean ($$\mu$$) and population variance
($$\sigma^2$$). Theoretically, it may be possible to make measurement on population but practically, the
population always have infinite number of items so we are always forced
to take a sample of size (n). For many other practical reasons, this
sample size is usually very small which makes the situation even worse
(we will see how). So we need to be able to estimate population
parameters by using a small sample and that estimate needs to
**unbiased**.

> **Bias** is a very commonly used term in statistics and in addition to
> its meaning in normal English language it has some statistical value.
> *A random variable is considered to be an unbiased estimator of a
> population parameter only if the expected value of the random variable
> is equal to the population parameter*.

(See note on expected value [here]()).
Similarly a random variable is an biased estimate of a population
parameter if the expected value of the random variable is not equal to
the population parameter. It is called overestimate if the expected
value is greater than the population parameter and vice-versa.
Mathematically, an estimator gives an unbiased estimate of population
parameter if,

$$E[estimator] = \text {population parameter} $$

#### Sample mean
Now lets come back to **sample mean**. If X represents a random variable
that measure sampling distribution of sample means, (see note on
[distribution of sampling means]()) according to **the Central limit
theorem** it is an unbiased estimate of population mean.ie:

$$E[X] = \mu$$

{% capture text-capture %}
We can prove this with some manipulation of $$E[]$$ 


Substitute X with the formula for sample mean

$$E[X] = E\left[\frac{1}{n}\sum_{i=1}^nx_i\right]$$

$$=\frac{1}{n}\sum_{i=1}^n E[x_i]$$

Expectation of a single observation is equal to population mean $$\mu$$.

$$=\frac{1}{n}\sum_{i=1}^n \mu$$

$$= \frac{1}{n}n\mu$$

$$=\mu$$

{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof2" button-text="proof here" toggle-text=text-capture  footer="done!" %}

Now, lets lets figure out the degree of freedom for the sample mean.
This is the equation for calculating a sample mean:

$$\bar x = \frac{1}{n}\sum_{i=1}^nx_i$$

For calculating the value of each sample mean, only the values coming
from each observation are used. If $$n$$ is the sample size, then all
$$n$$ values are used and **no any other intermediate estimates are
computed**. Therefore, the degree of freedom by our definition above is
$$n-0$$, or in simple form $$n$$. So, for the sample mean, the degree of
freedom is same as sample size. This seems relatively straight forward,
lets see sample variance next which will further clarify it.

#### Sample variance
The generally used formula for calculating a [sample variance]() is
given by or derived from this equation :

$$
\begin{equation}
\tag{equation 1}
s^2=\frac {\sum (x- \bar x)^2}{n-1}
\end{equation}
$$

By definition, variance is mean of squared deviation, so if we don't
think much and just go by definition, the equation for sample variance
should be:

$$
\begin{equation}
\tag{equation 2}
s^2_{bi}=\frac {\sum (x- \bar x)^2}{n}
\end{equation}
$$

This seems very plausible, but unlike sample mean, in calculation of
sample variance we need one intermediate estimate for population
parameter ($$\mu$$) given by sample mean $$\bar x$$. Although, sample
mean is an unbiased estimate of population mean, the mean calculated
from only one sample is subject to inherent randomness, and because of
this randomness the sample variance calculated using sample mean
according to equation 2 is a biased estimate of population variance. It
will almost always underestimate the population variance (report less
than what actually is). The amount of bias in the sample variance can be
mathematically shown to be equal to $$\frac{n}{n-1}$$ (see below for
proof). Therefore, to get an unbiased estimate of population variance we
need to multiply equation 2 by this factor.

{% capture text-capture %}
Proof for biased sample mean

Lets us write equation 2 into computational easy form. See my note on
[mean and variance]() if you are not familiar with derivation of
computational form of variance.

$$ s^2_{bi}=\frac {\sum x^2}{n}-\left(\frac {\sum x}{n}\right)^2  $$

And also write the symbol of s as random variable measuring sample
variance $$S^2_{bi}$$ and calculate expected value of that random
variable.

$$ E[S^2_{bi}]=E\left[\frac {\sum x^2}{n}-\left(\frac {\sum x}{n}\right)^2 \right] $$

$$=E\left[\frac {\sum x^2}{n}\right]-E\left[\left(\frac {\sum x}{n}\right)^2 \right] $$

The term $$\frac{\sum x}{n}$$ is the sample mean $$\bar x$$

$$=E\left[\frac {\sum x^2}{n}\right]-E\left[\left(\bar x\right)^2 \right] $$

Lets process $$E$$ further in

$$
\begin{equation}
\tag{equation 3}
=\frac {\sum E[x^2]}{n}-E\left[\bar x^2\right] 
\end{equation}
$$

For any given random variable Y we know,

$$Var(y) = E[y^2]-\left(E[y]\right)^2$$

$$\text{or, } E[y^2] = Var(y) + \left(E[y]\right)^2 $$

In equation 3 there are two terms which evaluates to expectation of
square, so expanding them equation 3 becomes

$$ E[S^2_{bi}] = \frac{\sum Var(x)+(E[x])^2}{n}- Var(\bar x) - (E[\bar x])^2$$

Now, lets simplify some of these terms, expectation of a variable is its
mean so, $$E[x] = \mu$$.

From, central limit theorem, variance of distribution of sample mean is
$$\frac{\sigma ^2}{n}$$ and sample mean being the unbiased estimate of
population mean $$E[\bar x] = \mu$$. Now, replace these values in the
equation above,

$$ E[S^2_{bi}] = \frac{\sum \sigma^2+(\mu)^2}{n}- \frac{\sigma ^2}{n} - (\mu)^2$$

Solving this:

$$  = \frac{( \sigma^2+(\mu)^2)n}{n}- \frac{\sigma ^2}{n} - (\mu)^2$$

$$ = \sigma ^2 + \mu ^2- \frac{\sigma ^2}{n}- \mu ^2$$

$$
\begin{equation}
\tag{equation 4}
E[S^2_{bi}] = \sigma ^2 \left(\frac{n-1}{n}\right)
\end{equation}
$$

Therefore, the expected value of biased sample variance is always less
than the population variance by a factor of (n-1)/n. For large sample
size this factor tends to be equal to 1 but on a small sample size it is
profound.
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof1" button-text="proof here" toggle-text=text-capture  footer="done!" %}




$$\text{Unbiased }s^2= s^2 = s^2_{bi}. \frac{n}{n-1}$$

$$=\frac {\sum (x- \bar x)^2}{n}.\frac{n}{n-1}$$

Removing n, it gives back the equation 1.

$$s^2 =\frac {\sum (x- \bar x)^2}{n-1}$$

Therefore, the equation we have been using for sample variance is
actually, equation 2 (given by the definition of variance) but adjusted
for bias associated with uncertainty in estimate of one the population
parameter during its calculation. And because we used this one
intermediate population estimate (sample mean, $$\bar x$$), we need to
subtract 1 from the number of values used to calculate sample variance.
Thus, sample variance has n-1 degree of freedom. This is very important
for a small size sample, because as n gets close to 1, subtracting 1 will
decrease denominator by large value increasing the expected value of
random variable measuring sample variance. This increase will balance
the variance underestimated by biased equation (equation 2). The plot
below show the distribution of sample variance (adjusted) with different
sample size n= {5,15,50,100}. The mean reported is the expected value of
each distribution. Regardless of the sample size all four plots have
expected value almost equal to the population variance (100).



![](:degree-of-freedom_files/figure-markdown/unnamed-chunk-1-1.svg)

*Figure 1: Distribution of sample variance.*


{% capture text-capture %}
``` r
par(mfrow=c(2,2))
set.seed(56)
#make a dummy population with N= 1000, \mu = 70 and \sigma= 10
pop<- rnorm(1000, mean= 70, 10)
sample_size = c(5,15,50,100)
for (size in 1:length(sample_size)) {
    sample_variance <- NULL
    for (i in 1:1000) {
    sample <- sample(pop, sample_size[size])
    sample_variance<-c (sample_variance, var(sample))
    }
hist(sample_variance, main=paste0('Dist of sample variance n = ',
                               sample_size[size],' \n mean of var = '
                               ,round(mean(sample_variance),1), 
                               " variance of var = ", round(var(sample_variance),1)),
                                xlim=c(0,400))
}

```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-thats" button-text="code for Fig: 1" toggle-text=text-capture  footer="done!" %}

On the other hand, the plots below are reporting the biased measure of
sample variance (given by equation 2 above) for exactly same experiment
reported above. The expected value is seriously lower than the
population variance and the situation is worse when sample size is
lowest. This under estimation occurs because with small sample size,
sample mean lies within that sample, all the deviations becomes smaller
than they actually need to be. With small sample size there is less
chance that true mean will be within or near that sample.


![](:degree-of-freedom_files/figure-markdown/unnamed-chunk-2-1.svg)  
*Figure 2: Distribution of biased variance*
{% capture text-capture %}
``` r
par(mfrow=c(2,2))
set.seed(56)
#make a dummy population with N= 1000, \mu = 70 and \sigma= 10
pop<- rnorm(1000, mean= 70, 10)
sample_size = c(5,15,50,100)
for (size in 1:length(sample_size)) {
    sample_variance <- NULL
    for (i in 1:1000) {
    sample <- sample(pop, sample_size[size])
    #biased variance
    sample_variance<-c (sample_variance, var(sample)*(sample_size[size]-1)/sample_size[size]) 
}
hist(sample_variance, main=paste0('Biased sample variance n = ',
                               sample_size[size],' \n mean of var = '
                               ,round(mean(sample_variance),1), 
                               " variance of var = ", round(var(sample_variance),1)),
                                xlim=c(0,400))
}
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="code2" button-text="code for Fig:2" toggle-text=text-capture  footer="done!" %}

### Quick recap

Lets summarize what we learned from sample mean and sample variance so
far:

1.  degree of freedom for a random variable estimating a population
    parameter is equal to number of independent information that went
    into calculation of that estimate minus the number of intermediate
    estimated population parameters used in calculation of such an
    estimate.
2.  All the estimates of population parameters should be an unbiased
    estimate of that population parameter. However, if degree of freedom
    is less than the number variables used in calculation of that
    estimate, it is always biased. ie

$$\text {df = n, --------> estimate is unbiased estimate of population parameter (Eg: sample mean)}$$
$$\text {df < n, --------> estimate is biased estimate of population parameter (Eg: sample variance)}$$

3.  This bias in the estimate of a population variance $$\sigma ^2$$
    using a sample variance can be adjusted by dividing the sum of
    squares (SS) by degree of freedom rather than taking the literal
    mean of squared deviations.

All these three statements above are universally applicable to any
situation where we need to estimate mean of squared deviations.
Therefore it is applicable to all the mean SS (MSB, MSW, Mean TSS, MSR,
MSE) calculated during ANOVA and linear regression.

### One way ANOVA

The source of variations in one way ANOVA are between levels of factors,
within each level and total variation. The table below shows how df is
calculated for each of the sources:

|Source  |  variables  |            intermediate pop estimates |  df|
| --------- |---------------------- |----------------------------------- |-----|
|Total     |all observations(n)   | mean of y (1)             |          n-1|
|Between  | number of levels (t)  | mean of y (1)             |          t-1
|Within   | all observations (n)  | mean of each levels(t)    |          n-t

Total variance calculation is same as the variance estimate we saw
above. But when it comes to the SSB, deviation used is the one between
mean of corresponding level to the estimate of population mean of y. So,
we have used one intermediate estimate. And within each level for a
balanced experiment, there are n/t number of observations, and n/t
number of deviations corresponding to each observation, but all the
deviations within a level is essentially using only one independent
piece of information (mean of that level). So, total number of
independent pieces of information is equal to the number of levels. And
therefore, df is t-1. For within variation, all observations contribute
their share of information but since deviation is taken from the mean of
the corresponding level that observation belonged to. Here level of each
mean is used as the estimate of true mean of that level not as a
variable, as in SSB. Therefore, df for within variance is n-t.

### Two way ANOVA

For a two way ANOVA with two factors A and C each with a and c number of
levels and n observation in each cell.

|  Source        |  variables        |     intermediate pop estimates |  df|
|  ---------------| ----------------|--------------------------------|---|
|  Total     |      all observations(acn-1) |  mean of y (1) |                   acn-1
|  Between cells |  no. of cells (ac)   |      mean of y (1) |                  ac-1
| Factor A   |     no. of levels in A (a) |   mean of y (1)  |                     a-1
| Factor C   |     no. of levels in C (c) |   mean of y (1)  |                     c-1
| A*C   |   ac, a, c                     |       mean of y (3 times) |(a-1)(c-1)
|  Within cells |   all observations (acn)  |  mean of each cell(ac) |        ac(n-1)

All the other SS has similar idea as one way ANOVA except interaction
term. Sum of square for AC is defined as:

$$SS_{AC} = SS_{Cells}-SS_A-SS_C$$ It is the variance explained only by
cells. It is calculated as

$$SS_{AC} = n\sum_1^{ac}[(mean_{cell} - \bar y_{total}) - (mean_{A} - \bar y_{total}) - (mean_{C} - \bar y_{total})]^2$$

The first deviation is from mean of a particular cell to estimate of
mean of y. The second deviation is from mean of particular level of A to
the estimate of mean of y. Since there are only a number of levels in A,
those each $$mean_A$$ will be reused c times. The third deviation is
from mean of a particular level of C to the estimate of mean of y. It is
clear that there is only one intermediate estimate of population
parameter $$\bar y$$. But this it is used three times for calculating
three different types of deviations. Each type of deviation has its own
df and total df is calculated by as df for cell minus df for A minus df
for C.

$$df(AC) = df(cell)- df(A) - df(C)$$

$$ = ac -1 - a + 1 - c +1$$

$$(a-1)(c-1)$$

### Simple linear regression

$$y = \beta_0 + \beta_1x + \epsilon$$

| Source   |    variables       |          intermediate pop estimates |  df|
| -----------|---------------------|-------------------------------|--------|
Total   |     all observations(n) |      mean of y (1) |                      n-1|
|  Regression |  number of reggressor +1  | mean of y (1) |                      1+1-1=1
|  Error  |      all observations (n)   |   number of regressor +1    |          n-2

This can be interpreted in very similar way to one way anova.

### Other application

So far we saw profound use of df to get unbiased estimate of any
type/partition of variance. In addition to this df is used to adjust the
standard normal distribution when the population parameters are unknown
and only sample statistics are used to calculate the test statistics.
The test statistics $$z=\frac{\bar x - \mu}{\frac{\sigma}{\sqrt n}}$$
follows standard normal distribution with mean 0 and variance 1. But in
may real case situation both $$\mu$$ and $$\sigma$$ are unknown. So,
standard normal distribution is not appropriate to calculate p-value.
The distribution needs to be adjusted for the added uncertanity because
of using sample variance. Therefore a new test statistics has been
deviced called t-statistic.

$$t=\frac{\bar x - \mu}{\frac{s}{\sqrt n}}$$

and the only parameter defining this distribution is degree of freedom
$$v$$associated with sample variance. This distribution has mean of 0
for $$v$$ \>1, otherwise undefined. and variance = $$\frac{v}{v-2}$$ for
$$v$$ \> 2. Unlike standard normal distribution, there is new curve for
each value of $$v$$. T-distribution always has fatter tails because of
the uncertainty associated with sample variance used in calculation of
the t-statistics.



![](:degree-of-freedom_files/figure-markdown/unnamed-chunk-3-1.svg)  
*Figure 3: PDF curve for Normal and t-distribution.* 

{% capture text-capture %}
``` r
curve(dnorm(x), from = -4,to= 4, col= "red", ylab= "density")
curve(dt(x, df=5), from = -4,to= 4, col= "blue", add = TRUE)
curve(dt(x, df=30), from = -4,to= 4, col= "green", add = TRUE)
legend(2,0.3, legend = c("Std. normal ", "t, df= 5", "t, df= 30"), col= c("red", "blue", "green"), lty= c(1,1,1))
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="code3" button-text="code for fig: 3" toggle-text=text-capture  footer="done!" %}

The $$\chi^2$$ and $$F$$ distribution also has probability density
function adjusted for the degree of freedom.

To build the intuition I highly suggested to watch this video by
[Justin](https://www.youtube.com/watch?v=N20rl2llHno&t=1s).

