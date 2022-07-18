---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: Expected value  
title: Expected value 

# post specific
# if not specified, .name will be used from _data/owner.yml
author: Amrit Koirala
# multiple category is not supported
category: Statistics
# multiple tag entries are possible
tags: [random variable , expected value, sampling distribution]
# thumbnail image for post
img: ":Expected-value_files/figure-markdown/unnamed-chunk-1-1.png"
# disable comments on this page
#comments_disable: true

# publish date
date: 2019-11-01 08:11:06 +0900

# seo
# if not specified, date will be used.
meta_modify_date: 2019-11-01 08:11:06 +0900
# check the meta_common_description in _data/lang/[language].yml
meta_description: "Expected value"

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

# Expected value

### Expected value of a random variable
<!-- outline-start -->

Frequently called Expected value, expectation, expectancy, mathematical
expectation, sometime referred simply as mean, average or first moment.
For a random variable X it is found to be represented by any one of
these symbols $$\text{  E(X),  E[X],  EX, E, \mathbb{E} }$$.
<!-- outline-end -->


#### Definition

Once we know the probability distribution of a random variable we can
find the expected value of that random variable, which is exactly what
the name is suggesting, it is average or mean value of the random
variable which we expect to find most often. Lets plot binomial
probability distribution of two random variables (X and Y) with same
number of trials but different p. 
{% capture text-capture %}
``` r
par(mfrow=c(1,2))
random_var <- 0:10
probability <- NULL
for (i in 1:11) {
  k=random_var[i]
  #probability[i] <- choose(10, k)*(0.2^k)*(0.8)^(10-k)
  probability[i]<- dbinom(k, size=10, prob = 0.2)
}

barplot( probability, names.arg = random_var, main = 
           "B(X~10,0.2)", 
         xlab= "random variable, X", ylab= "probability")
#Next binomial probability 
random_var <- 0:10
probability <- NULL
for (i in 1:11) {
  k=random_var[i]
  #probability[i] <- choose(10, k)*(0.2^k)*(0.8)^(10-k)
  probability[i]<- dbinom(k, size=10, prob = 0.7)
}

barplot( probability, names.arg = random_var, main = 
           "B(Y~10,0.7)", 
         xlab= "random variable, Y", ylab= "probability")
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code1" button-text="code here" toggle-text=text-capture  footer="done!" %}
![](:Expected-value_files/figure-markdown/unnamed-chunk-1-1.png) So for
the plot we can see that X, is most often expected be 2 and Y is most
often expected to be 7. Therefore,

$$E[X] = 2$$

and

$$E[Y] = 7$$

#### Finite probability distribution

Now lets see how to it is calculated mathematically. For a random
variable, X with finite probability distribution the expected value of
X, E\[X\] is equal to the weighted mean of all possible outcomes (note
that random variables are always numeric values). For example in
**binomial distribution** $$(X \sim B(n,p))$$ we have only n+1 number of
possible outcomes (k) each with probability of P(X=k) defined by
following equation:

$$P(Y=k)= {n \choose k}p^k(1-p)^{n-k}$$

Therefore, when we calculate the weighted average of all outcomes $$k$$,
(weighted by their probability) then it is the expected value of random
variable X.

$$E[X] = \sum_{k=0}^n k.{n \choose k}p^k(1-p)^{n-k}$$

On further solving the equation this simplifies to

$$= np$$

#### For continuous density function

Random variables like sample mean which follow normal distribution which
is a continuous probability density function the expected value needs to
be calculated by using integration but concept can be understood as
similar to above discussed for binomial distribution.

Wikipedia has elaborate explanation about expected value of different
probability distribution
[wiki](https://en.wikipedia.org/wiki/Expected_value) Check the table
half way in this page.
