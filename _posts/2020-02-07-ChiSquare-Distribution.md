---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: English_Chisq
title: Chi squared distribution 

# post specific
# if not specified, .name will be used from _data/owner.yml
author: Amrit Koirala
# multiple category is not supported
category: Statistics
# multiple tag entries are possible
tags: [degree of freedom, expected value, sample variance, Chi squared]
# thumbnail image for post
img: ":chisquared-distribution_files/figure-markdown/unnamed-chunk-3-1.svg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2020-02-07 08:11:06 +0900

# seo
# if not specified, date will be used.
meta_modify_date: 2020-02-07 08:11:06 +0900
# check the meta_common_description in _data/lang/[language].yml
meta_description: "chi squared distribution explained in plain english intuitive way "

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

### Distribution of sample variance
<!-- outline-start -->
Just like sampling distribution of sample mean, sample variance follows
a distribution but it is slightly complex than, normal distribution
followed sample mean. The shape of this distribution varies according
the sample size.<!-- outline-end --> Therefore, there is a different curve to represent
distribution of sample variance from samples of different sizes. Since,
mean of distribution of sample variance ([expected value]()) is equal to
population variance, sample variance is also called unbiased estimate of
population variance. The Mean and variance of such distribution is given
by following formulas (keep reading for proof).

$$mean(s^2) = \sigma^2$$

and variance of

$$Var(s^2)= \frac{2(\sigma^2)^2}{v}$$

Where, $$v$$ is [degree of freedom](/posts/2019-07-01-Degree-of-freedom) given by n-1.

To get some intuition, lets make some simulated distribution of sample
variance from a population with mean 70 and variance 100 taking samples
of size(n) = {3,5,9,15,50,100}, re-sampled 1000 times, Figure 1 shows
the histogram of sample variance of those 1000 samples for each size.
{% capture text-capture %}
``` r
par(mfrow=c(3,2))
set.seed(56)
#make a dummy population with N= 1000, \mu = 70 and \sigma= 10
pop<- rnorm(1000, mean= 70, 10)
sample_size = c(3,5,9,15,50,100)
for (size in 1:length(sample_size)) {
    sample_variance <- NULL
    for (i in 1:1000) {
    sample <- sample(pop, sample_size[size])
    sample_variance<-c (sample_variance, var(sample))
}
hist(sample_variance,breaks = 200, main=paste0('Dist of sample variance n = ',
                               sample_size[size],' \n mean of var = '
                               ,round(mean(sample_variance),1), 
                               " variance of var = ", round(var(sample_variance),1)),
                                xlim=c(0,400), ylim = c(0,60))
}
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code1" button-text="code here" toggle-text=text-capture  footer="done!" %}

![Distribution of sample variance](:chisquared-distribution_files/figure-markdown/unnamed-chunk-1-1.svg)

As you can see from the figure above the shape of histogram is different
for each sample size and mean and variance of these distribution are
also very close to the one that could be calculated by above formulas.
When the sample size is small (n=3), it is positively skewed with long
tail on right and most of the mass of distribution at zero. What this
suggesting is when we use only 3 observations, most of the time we get
very small variance. But how does it compare to the population variance?
With such small sample size although mean of the whole distribution is
still equal to the population variance, there is high probability that a
randomly selected sample variance will underestimate the true variance.
As we increase the sample size to 5, the mass of distribution moves
slightly right and is slightly less skewed than n=3. As we keep
increasing our sample size to 100, the distribution looks much like
normal distribution with both mean and median equal to population
variance and there is much less variance in sample variance compared to
our smallest sample size (n=3). This signifies that if we have big
enough sample size, a randomly selected sample is much likely to
represent the population variance.

### $$\chi^2$$ distribution

We know variance is mean of sum of square of deviations. If we look at
only the deviations, it is, a random variable that follows a normal
distribution (because both $$X$$ and $$\bar X$$ follows normal
distribution). Since, all other terms in variance are constant, we can
find the distribution of sample variance if we know the distribution of
sum of square of deviations. And this is exactly what $$\chi^2$$ gives.
To simplify the calculations it is always calculated from the standard
normal distribution and is applicable for any random variable that
follows standard normal distribution.

Suppose $$Z$$ is a random variable following standard normal
distribution $$Z \sim N(0,1)$$. Then the distribution given by the sum
of square of n items from this distribution follows $$\chi ^2$$
distribution which is defined by single parameter, the number of
independent items used in the calculation of sum of $$Z^2$$ ($$v$$).

$$\chi^2= \sum Z^2$$ 

$$\chi^2$$ distribution has following properties:

1.  Being the sum of squares, it can never be less than zero.
2.  The shape of distribution changes with the number of z used to
    calculation the sum.
3.  The mean of the distribution is given by $$v$$ and variance given by
    $$2v$$.
4.  When the sample size is larger (\>30), the $$\chi^2$$ distribution
    is very similar to normal distribution. It is so similar that a
    normal distribution with mean $$z=\frac{X-v}{\sqrt(2v)}$$ can be
    used calculate z and used std normal distribution.

To build the intuition, lets see it in R, first by making a standard
normal distribution and then sampling $$z$$ of different sample size and
then calculating the sum of square of $$z$$s in that sample. Histograms
shows $$\chi^2$$ distribution at degree of freedom equal to
{3,5,9,15,100}. See if our histograms comply with the properties of
$$\chi^2$$ distribution.

{% capture text-capture %}

``` r
par(mfrow=c(3,2))
set.seed(56)
stdNormal <- rnorm(1000)
sample_size = c(3,5,9,15,50,100)
for (size in 1:length(sample_size)) {
    sumXsq <- NULL
    for (i in 1:1000) {
    X <- sample(stdNormal, sample_size[size])
    sumXsq<-c (sumXsq, sum(X^2))
}
hist(sumXsq,breaks = 200, main=paste0('Dist of Sum of X squared n = ',
                               sample_size[size],' \n mean = '
                               ,round(mean(sumXsq),1), 
                               " variance = ", round(var(sumXsq),1)),
                                xlim=c(0,150), ylim = c(0,30))
}
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code2" button-text="code here" toggle-text=text-capture  footer="done!" %}

![](:chisquared-distribution_files/figure-markdown/unnamed-chunk-2-1.svg)

Also visualize how these distributions will actually look when produced
from the mathematical formula for $$\chi^2$$ distribution.

{% capture text-capture %}

``` r
curve(dnorm(x), from = -3,to= 20, col= "darkgreen", ylab= "density", xlab="Chi Squared")
linecol <- rainbow(6)
size = c(3,5,9,15,50,100)
for(i in 1:length(size)){
    curve(dchisq(x, df=size[i]-1), from = 0,to= 20, col= linecol[i], add = T)
}
legend(13,0.4, legend = c("Std Normal", paste0("df = ", size )), col= c("darkgreen",linecol), lty= c(1,1,1,1,1,1,1))
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code3" button-text="code here" toggle-text=text-capture  footer="done!" %}

![](:chisquared-distribution_files/figure-markdown/unnamed-chunk-3-1.svg)

### $$\chi^2$$ statistic

Now the we understand the $$\chi^2$$ distribution, lets see how can we
establish relationship between sample variance and this distribution.
From the equation for $$\chi^2$$ we know :

$$\chi^2= \sum Z^2$$

$$= \sum\left(\frac{Y-\bar Y}{\sigma}\right)^2$$

$$=  \frac{\sum(Y- \bar Y)^2}{\sigma^2}$$ We know the equation for
sample variance is $$s^2= \frac{\sum(Y- \bar Y)^2}{n-1}$$, Substituting
the value of sum of squares (SS);

$$\chi^2=  \frac{(n-1)s^2}{\sigma^2}$$

One we have this $$\chi^2$$ statistics we can get the idea about the
probability of variance of particular sample from $$\chi^2$$
distribution which follows n-1 degree of freedom. Check my notes on
[degree of freedom](/posts/2019-07-01-Degree-of-freedom) if you are not sure why we need to use n-1 degree
of freedom.

Lets also see how $$\chi^2$$ statistics would distribute in our simulate
data.

{% capture text-capture %}
``` r
par(mfrow=c(3,2))
set.seed(56)
#make a dummy population with N= 1000, \mu = 70 and \sigma= 10
pop<- rnorm(1000, mean= 70, 10)
sample_size = c(3,5,9,15,50,100)
for (size in 1:length(sample_size)) {
    sample_variance <- NULL
    for (i in 1:1000) {
    sample <- sample(pop, sample_size[size])
    sample_variance<-c (sample_variance, var(sample))
    chisq <- (length(sample)-1)*sample_variance/100
}
hist(chisq,breaks = 200, main=paste0('Dist of chisq n = ',
                               sample_size[size],' \n mean of chisq = '
                               ,round(mean(chisq),1), 
                               " variance of chisq = ", round(var(chisq),1)),
                                xlim=c(0,150), ylim = c(0,30))
}
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code4" button-text="code here" toggle-text=text-capture  footer="done!" %}

![](:chisquared-distribution_files/figure-markdown/unnamed-chunk-4-1.svg)

#### Derivation of mean of sampling distribution of variance

This distribution is exactly same as we got from standard normal
distribution in figure above. We can also use this equation and the
properties of $$\chi^2$$ to derive the mean and variance of distribution
of sample variance.

From the equation for $$\chi^2$$

$$\chi^2=  \frac{vs^2}{\sigma^2}$$
$$\text{or,         } s^2 = \frac{\chi^2 \sigma^2}{v}$$

From the property of $$\chi^2$$ distribution, the value of $$\chi^2$$ at
mean of the distribution is $$v$$.

$$mean(s^2)=\frac{v\sigma^2}{v}$$

$$mean(s^2)= \sigma^2$$

This again, shows expected value of sampling distribution of sample
variance is equal to population variance.

#### Variance of sampling distribution of variance

To prove this we can start from the fact that we know variance of
$$\chi^2$$ distribution is $$2v$$

$$Var(\chi^2) = 2v$$

$$Var\left(\frac{vs^2}{\sigma^2}\right)=2v$$

$$\frac{v^2}{\sigma^4}Var(s^2)= 2v$$

$$Var(s^2)=\frac{2\sigma^4}{v}$$

### Conclusion

Although we do not use $$\chi^2$$ most often directly in hypothesis
testing, but this distribution is the one which establish a connecting
link between population variance ($$\sigma^2$$) and sample variance
($$s^2$$). Therefore any hypothesis testing where population variance is
estimated based on the sample variance this distribution is being used
under the hood which we will see more clearly in the t-distribution and
F-distribution.
