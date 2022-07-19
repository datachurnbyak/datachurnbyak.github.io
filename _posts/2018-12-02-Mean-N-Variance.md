---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: Brief note mean and variance  
title: Mean and Variance; covering the base  

# post specific
# if not specified, .name will be used from _data/owner.yml
author: Amrit Koirala
# multiple category is not supported
category: Statistics
# multiple tag entries are possible
tags: [Mean, Variance]
# thumbnail image for post
img: ":meanVar.svg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2018-12-02 08:11:06 +0900

# seo
# if not specified, date will be used.
meta_modify_date: 2018-12-02 08:11:06 +0900
# check the meta_common_description in _data/lang/[language].yml
meta_description: " Starting statistics with mean and variance  "

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

## Basic concept

<!-- outline-start -->

It is very important to understand the measure of central tendency, mean
and measure of dispersion, variance in statistics.<!-- outline-end -->General formula for
mean is:

$$
\begin{equation}
\tag{eq 1}
Mean (\mu) = \frac{\sum x}{N} 
\end{equation}$$

When frequency table is given mean is:

$$
\begin{equation}
\tag{eq 2}
Mean (\mu) = \frac{\sum fx}{N}
\end{equation}
$$

When relative frequency (empirical probability/proportion) is given,
mean is given by:

$$
\begin{equation}
\tag{eq 3}
Mean (\mu) = \sum ( x\times p) 
\end{equation}
$$

where p is the proportion of particular random variable X.

In probability, [the expected value](2019-11-01-Expected-value) of a random variable, E\[x\] is
similar to mean of that random variable. And is given as:

$$
\begin{equation}
\tag{eq 4}
E[X] = \sum x \times P(X=x)
\end{equation}
$$

where:

-   E\[X\] = Expectation of random variable X
-   P(X=x) = Probability that random variable has value of x.

If all values of random variable are equally likely, than P(X=x) is
equal to 1/N and since 1/N is constant can be moved outside summation
which makes this Equation equal to the general equation for mean. More
in binomial distribution.

### Measure of dispersion

Dispersion is the extent of divergence from the center of data. Standard
deviation ($$\sigma$$) and variance ($$\sigma ^2$$) are the two measures
of dispersion. To understand why these two measures exist and what do
they actually explain about dispersion, lets first see how a dispersion
of a variable can be studied. To understand the extent of divergence is
a three step process:

1. First need to determine what is the center of
data (we use mean $$\mu$$ for this) 
2. Second measure the extent of
divergence of each observation from that center. 
3. Finally, summarize
these divergences by taking the average of all divergence. ie
$$ \frac{\sum divergence }{N}$$

The first and third step are relatively straight forward but we need to
look at the second step further. The simplest form of divergence we can
measure is deviation from mean ie ($$ x-\mu$$). But because of the
nature of mean, when we attempt to sum all the divergence, it is always
equal to 0 which is not helpful. Alternatively, to neutralize the effect
of negative deviation, we can square the deviation for each value of a
random variable and calculate the average of these squared deviation.
For this deviation from mean is first squared and added to get the sum
of square (SS).

$$
\begin{equation}
\tag{eq 5}
SS=\sum (x- \mu)^2
\end{equation}
$$

One natural question to raise here is why we are using squared
deviation? Taking absolute deviation would also solve the problem of 0
deviation in total. Squared deviations is not a random choice but very
carefully thought by early statisticians because squared deviations and
thus variance has some very useful mathematical properties when solving
calculus and derivative problems. 

Once we have SS it can be divided by N
to get the mean of squared deviation which is called **variance**.
Although variance is best for mathematical prospective, one problem with
variance is its unit. It has square unit compared to the random variable
we are studying. For example if we are studying weight in kg, variance
has unit of $$kg^2$$, which is difficult to interpret the dispersion.
Therefore standard deviation $$\sigma$$ is frequently reported which is
equal to square root of variance.

$$
\begin{equation}
\tag{eq 6}
\sigma^2=\frac {\sum (x- \mu)^2}{N}
\end{equation}
$$

The sum of squares (SS) and can be solved further to get simplified
form.

$$SS=\sum (x- \mu)^2$$

$$ = \sum x^2 -2x\mu + \mu^2 $$

Now summation can be processed into each term

$$= \sum x^2 -2\mu\sum x + \mu^2\sum1  $$

Now replace the value of $$\mu = \frac {\sum x}{N}$$

$$= \sum x^2 -2\frac {\sum x}{N}\sum x +  \left(\frac {\sum x}{N}\right)^2.N$$

Solving this we get

$$ 
\begin{equation}
\tag{eq 7}
\text{Sum of squares (SS)}=\sum x^2 - \frac {(\sum x)^2}{N} 
\end{equation}
$$

This equation 7 is used for calculating SS most often than equation 6
because it is computationally very easy to calculate both for human and
computers. For a population, variance is then obtained by dividing sum
of square of deviation by N.

$$
\begin{equation}
\tag{eq 8}
Variance = \sigma^2 = \frac {\sum x^2}{N}-\left(\frac {\sum x}{N}\right)^2 
\end{equation}
$$

This equation of variance can be further explained in terms of
probability. In this equation first term is mean of $$x^2$$ and second
term is square of mean of x. It is very important to identify the
difference between these two terms. As we know mean is also called as
expectation of a random variable in probability.

$$Variance = E[X^2]-(E[X])^2$$

$$\text{or, } Variance= \sum x^2.P(X=x^2)- \left(\sum x.P(X=x)\right)^2 $$

#### Sample variance

Usually variance is calculated from a sample of observation so, while
calculating the SS, deviations need to be calculated from sample mean
and sum of square of deviation is divided by n-1 to get an unbiased
estimate of population variance (See details in [degree of freedom](2019-07-01-Degree-of-freedom)).

$$
\begin{equation}
\tag{eq 9}
\text{Sample variance} = s^2 = \frac {\sum x^2 - \frac {\left(\sum x\right)^2}{n}}{n-1}
\end{equation}
$$
