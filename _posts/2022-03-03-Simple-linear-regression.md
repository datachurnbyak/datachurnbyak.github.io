---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: Eng_simple linear regression  
title: "Simple Linear regression: Intuitive way"  

# post specific
# if not specified, .name will be used from _data/owner.yml
author: Amrit Koirala
# multiple category is not supported
category: Statistics
# multiple tag entries are possible
tags: [regression]
# thumbnail image for post
img: ":simple-linear-regression-_files/figure-markdown/unnamed-chunk-3-1.svg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2022-03-03 08:11:06 +0900

# seo
# if not specified, date will be used.
meta_modify_date: 2022-03-03 08:11:06 +0900
# check the meta_common_description in _data/lang/[language].yml
meta_description: " Learning Linear regression with R "

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

### Simple Linear Regression
<!-- outline-start -->
This is a statistical method to establish a linear relationship between
two quantitative variables such that one variable can give an estimate
of the other. The variable being estimated is called dependent variable
and the variable used to estimate is called independent variable or
regressor.<!-- outline-end -->

The first step in linear regression is to find a line of best fit. You
may ask why do we want to fit a line, and not circle or a quadratic
function? Well it has to do with one of the most important assumption of
linear regression which states that there is a linear relationship
between dependent variable and independent variable thus a linear
regression with a single predictor can be represented as a equation for
a line:

$$y = \beta_0 + \beta_1x + \epsilon$$ 

where,

$$y$$ = dependent variable

$$\beta_0$$ = y-intercept of the predicted line and the value of
dependent variable when value of predictor variable $$x$$ is 0.

$$\beta_1$$ = slope of the line of fit or the coefficient by which
predictor variable needs to be adjusted to get the predicted value of y
($$\hat{y}$$). 

$$\epsilon$$ = there is always some difference between
predicted value and the actual value of y which is called error term The
variance of error term is equal for all observation and denoted by
$$\sigma^2$$.

Lets make a small dummy data where we want to predict the weight of 5
students by their height.
{% capture text-capture %}
``` r
height = c(5.5, 3.4, 4.4, 6)
weight = c(140, 120, 145, 170)

par(mfrow=c(1,2))
plot(weight~height, col = "black" , cex = 2, pch =20, main = "a")
#plot(weight~height, col = "red" , cex = 2, pch =20, ylim = c(0,170), xlim= c(0,6))

plot(weight~height, col = "black" , cex = 2, pch =20, main ="b")
abline(h=143.75, col="red")
abline(98, 10, col= "blue")
abline(70.287, 15.225, col= "purple")
abline(60, 16, col= "green")
abline(25,25, col ="brown")
abline(-25,35, col ="darkcyan")
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code1" button-text="click for code" toggle-text=text-capture  footer="done!" %}


![](:simple-linear-regression-_files/figure-markdown/unnamed-chunk-1-1.svg)

Fig 1: Simple linear regression

As seen in the scattered plot (figure 1.a), there appears to be some
form of linear relationship between height and weight and we are very
tempted to draw a line right across the points. Lets draw some potential
lines of different color (Figure 1.b) that could be a best line of fit.
Now how do we find which is the best line of fit? There are several
algorithms that targets to minimize the sum of square of deviation,
absolute deviation, lack of fit, or penalty for a cost function (example
ridge regression and lasso). We will focus to well known and most used
approach called least squares where the goal is to minimizing the sum of
square of deviation between predicted and observed value of y. Lets
implement the least squares in the 6 lines that we have drawn on our
plot (Figure 1.b). To calculate the sum of square of deviation (SS) for
each line defined by equation $$y=a+bx$$, we need to first predict the
value of y for given x. Once we have the predicted values, we can find
the differences between observed and predicted value (aka deviation or
residual or error in estimate) for all the data points. Followed by
calculating square of deviation and adding them up to get sum of
squares. Once we have SS for each line, we can plot those SS by lines to
visualize the line with least SS. See code below for detail of the
steps.
{% capture text-capture %}
``` r
#store each line as a vector of a and b, y=a+bx 
red = c(143.75, 0)
blue = c(98, 10)
purple = c(70.287, 15.225)
green = c(60, 16)
brown = c(25, 25)
darkcyan = c(-25, 35)
lines <- list(red=red, blue=blue, purple=purple, green=green, brown=brown, darkcyan=darkcyan)
#write a for loop which will take a line at time and give prediction of weight for each height
#first make a empty list to store predictions 
predicted_weight = vector(mode= "list", length=6)
names(predicted_weight) <- names(lines)
# take a lines at a time and apply that line equation to all height values 
for(line in names(lines)){
    predicted_weight[[line]] <-sapply(height, function(x){lines[[line]][1] + lines[[line]][2]*x})
}
#Now that we have predicted weight value from each line lets first plot 
par(mfrow= c(1,2))
plot(weight~height, col = "black" , cex = 2, pch =20, main="a")
for(line in names(lines)){
    abline(lines[[line]], col=line)
    points(height, predicted_weight[[line]], col=line, pch=20, cex=1)
}

#Also calculate the SS for each lines 
deviation <- lapply(predicted_weight, function(x) x-weight)
#Square and sum each item in deviation 
SS <- lapply(deviation, function(x) sum(x[1]^2,x[2]^2,x[3]^2,x[4]^2))
##plot SS by the line 
barplot(unlist(SS), col= names(lines), ylab = "Sum of squares" , main="b")
text(x= 1:6, y= unlist(SS)-100, label= round(unlist(SS),1), cex = 0.6)
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code2" button-text="click for code" toggle-text=text-capture  footer="done!" %}

![](:simple-linear-regression-_files/figure-markdown/unnamed-chunk-2-1.svg)

Fig 2: Manually calculating least sum of square 

Figure 2.a shows the predicted points by each line as the colored dots
in the respective line. And in the barplot on the right we can see the
purple line has least sum of squares and is the best line of fit among
these six lines.

### Ordinary Least Square

In the above example we worked with only six lines and we could easily
find the best one minimizing the sum of squares, but the extent of
actual problem is much complex because there could be infinite number of
lines passing this data. It is essentially a minimization problem and
fortunately it is a well known and worked out problem in the field of
mathematics. It is solved using the ordinary least square
[(OLS)](https://en.wikipedia.org/wiki/Ordinary_least_squares) method.
Under the hood OLS uses principles of calculus, statistics and some
assumptions to give an estimate of $$\beta_0$$ and $$\beta_1$$ denoted
as $$\hat{\beta_0}$$ and $$\hat{\beta_1}$$ respectively for the line of
best fit. Although we may skip the details of how this method works, we
need to be clear about how to interpret them and what are their
limitations. Following are the equation to calculate $$\hat{\beta_0}$$
and $$\hat{\beta_1}$$.

$$\hat{\beta_1}=\frac{\sum(x-\bar{x})(y-\bar{y})}{\sum(x-\bar{x})^2}= \frac{S_{xy}}{S_{xx}}$$

$$\hat{\beta_0}=\bar{y}-\hat{\beta_1}\bar{x}$$

#### Intuitive explaination of $$\hat{\beta_1}$$

$$\hat{\beta_1}$$ is the amount by which a unit change in independent
variable will make the corresponding change in dependent variable. If
$$\hat{\beta_1}$$ is negative than a unit increase in x will decrease y
by $$\hat{\beta_1}$$. The above equation for $$\hat{\beta_1}$$ can also
be written as below:

$$\hat{\beta_1}= \frac{\text{Sample Covariance between x and y}}{\text{Sample Variance of x}}$$

Which means higher the covariance between x and y higher is the value of
$$\hat{\beta_1}$$ and it is inversely proportional to the variance of
independent variable.

#### y-intercept

These equations may look very technical but have some intuitive
features. Usually $$\hat{\beta_0}$$ is put first but, it is derived from
$$\hat{\beta_1}$$ and it is a constant needed to make the adjustment to
the line equation so that our line of best fit passes through the point
$$(\bar{x},\bar{y})$$. In absence of this intercept term, we force the
line to pass through origin (0,0) and this may not be good fit for the
data. Therefore we should always include this intercept term in
regression equation. $${\beta_0}$$ ensures the line of best fit is
passing through the center of the data $$(\bar x, \bar y)$$ and sum of
all the residuals (difference between estimated y and observed y) is
zero. Other than this, y-intercept does not have any value in explaining
the relationship between x and y. Although we get p-value and standard
error for the intercept because of mathematics behind OLS (and
statistical software like R report it), we should not be tempted to
force any interpretation to our data using the intercept. Theoretically,
it is the estimate of y when all the other independent variables have
value of 0 (no effect of any independent variable at all). In most
cases, it is impossible to have independent variable to be zero (example
its impossible to have a student with zero height). Even if your data is
centered around origin (for example predicting amount of snow by temp in
winter season), the value of y at the intercept is value of dependent
variable when there is no independent variable, which is against our
sole purpose which is to understand the effect of independent variable
on dependent variable. Intercept should always be taken only as a factor
used to adjust line of best fit and nothing more than that.

In addition, these estimates have some very useful statistical
properties: 1. Both $$\hat{\beta_0}$$, and $$\hat{\beta_1}$$ are an
estimate of population $${\beta_0}$$ and $${\beta_1}$$ using the current
sample of x and y. For example, Instead of just working with 4 students'
data as in our example, we can have data from 1000 of students, sampling
10 at a time and for each sample calculate $$\hat{\beta_0}$$, and
$$\hat{\beta_1}$$. Doing so for multiple of times (1000 times) will give
us the sampling distribution of $${\beta_0}$$ and $${\beta_1}$$.

{% capture text-capture %}

``` r
library(MASS)
set.seed(566)
d <- mvrnorm(n=1000, mu= c(5,143.75), Sigma = matrix(c(1, 0.6,0.6,1),nrow = 2))
heightPop <- d[,1]
weightPop <- d[,2]

par(mfrow=c(2,4))
sample_colour <- rainbow(6)
six_beta_0 <- c()
six_beta_1 <- c()
for (i in 1:6) {
    index <- sample(1:1000, 10)
    s_wt <- weightPop[index]
    s_he <- heightPop[index]
    plot(weightPop~heightPop,col = "grey" , cex = 2, pch =20, main= letters[i], xlab="Height", ylab="Weight" )
    points(s_he, s_wt, col= sample_colour[i], cex=2, pch=20 )
    m <- lm(s_wt~s_he)
    abline(m$coefficients, col= sample_colour[i])
    text(mean(s_he),140.4,labels =paste0("y= ",round(m$coefficients[1],1), " + ", round(m$coefficients[2],1), "x"))
    six_beta_0[i] <- m$coefficients[1]
    six_beta_1[i] <- m$coefficients[2]
    }

beta_0 <- c()
beta_1 <- c()
for (i in 1:1000) {
    index <- sample(1:1000, 10)
    s_wt <- weightPop[index]
    s_he <- heightPop[index]
 m <-lm(s_wt~s_he)
beta_0[i] <-m$coefficients[1]
beta_1[i] <- m$coefficients[2]
}

hist(beta_0, breaks =100, main = "Distribution of beta estimates")
for(i in 1:6)abline(v=six_beta_0[i], col=sample_colour[i])
hist(beta_1, breaks = 100, main = " (both sampled 1000 times)")
for(i in 1:6)abline(v=six_beta_1[i], col=sample_colour[i])
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code3" button-text="click for code" toggle-text=text-capture  footer="done!" %}

![](:simple-linear-regression-_files/figure-markdown/unnamed-chunk-3-1.svg)

Fig 3: Distribution of regression coefficient 

From the plots which shows distribution of 1000 $$\hat{\beta}$$s values
we can see that it is normally distributed hence we can apply central
limit theorem to these coefficients as well. Which gives that, the mean
of such distribution of $$\hat{\beta}$$s is the unbiased estimate of
$${\beta}$$s and the variance of such distribution of $$\hat{\beta}$$s
is given by OLS as follows:

$$Var(\hat{\beta_1)}= \sigma^2\frac{1}{S_{xx}}$$

$$Var(\hat{\beta_0)}= \sigma^2 \left( \frac{1}{n}+\frac{\bar{x}^2}{S_{xx}}\right)$$

Where: $$\sigma^2$$ is the variance of residuals or error terms. These
variance terms will be used for performing t-test for each coefficients
(will see later).

#### Assumptions

OLS gives us estimate of coefficients and great statistical power but
there are some limitations. For all the estimates to be valid, following
assumptions should be fulfilled: - The population under study should
strongly follows linear model and the parameters correctly specified. -
The samples were taken randomly from the population (this is the single
assumption necessary for distribution of sample mean). - There is
variation in explanatory variable. - Homoskedasticity : The variance of
residuals ($$\sigma^2$$) is equal for all observation. The deviation we
can calculate for each observation of y, $$\hat{e}$$ called residual is
a random variable which follows normal distribution, and have mean $$e$$
and variance equal to $$\sigma^2$$. Similar to $$\hat{\beta}$$) if we
sample 1000 times and see the distribution of residual for each
observation, it will have normal distribution and the variance needs to
be same for all observations. - The error term is independent of the
independent variable.

### Analysis of variance

One we have the line of best fit, we can do inference about the
statistical significance of the coefficients. Regression when we go back
to basics is also an analysis of variance just like ANOVA and the only
difference is, in ANOVA we want to find variance explained by a effect
and make statistical testing .See the details in [ANOVA](2021-06-14-OneWay-ANOVA). But, in
regression we will first find a line of best fit and find variance
explained by the line and do statistical testing to see if the variance
explained by the line is significantly different than the unexplained
variance. Lets plot our example of four sample and best line of fit we
found for it.

{% capture text-capture %}
``` r
plot(weight~height, col = "black" , cex = 2, pch =20)
    abline(70.287, 15.225, col="purple")
    abline(h=143.75, col= "red")
    points(height, predicted_weight[[3]], col="purple", pch=20, cex=1.5)
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-code4" button-text="click for code" toggle-text=text-capture  footer="done!" %}


![](:simple-linear-regression-_files/figure-markdown/Regression ANOVA.svg)

In absence of any independent variable, mean of y $$\bar{y}$$ is the
best estimate of y and total variance is calculated by taking deviation
of observed values from $$\bar{y}$$. This is called **Total Variance**.
We can estimate this by first calculating sum of square of deviation
(TSS). Adding squares of all the red colored deviations in above figure
will give TSS. Just like a normal variance of a sample this TSS has n-1
[degree of freedom](2019-07-01-Degree-of-freedom).

$$\hat{Var(Total)}= MST= \frac{SST}{n-1}$$

When we have the regression line some of the total variance is explained
by the line, which can estimated by calculating the Sum of square of
Regression (SSR). SSR is calculated by by adding up the square of
deviation between the estimated value of y and mean of y. The sum of
square of purple colored deviations in the above figure is equal to the
SSR. The degree of freedom for SSR is equal to the number of
coefficients in model - 1 (details in [degree of freedom](2019-07-01-Degree-of-freedom)) . It is
equal to the Sum of Square Between (SSB) calculated in ANOVA. The
estimate of variance explained by regression is mean of SSR (MSR).

$$\hat{Var(Regression)}= MSR= \frac{SSR}{\text{no. of } \beta s - 1}$$

There is always a second component of variance called variance of error
($$\sigma^2$$), which represents the variance not explained by
regression line. It is estimated using Sum of Square of Error (SSE). Sum
of square of deviation between estimated value of y and observed value
of y is equal to SSE. In the figure above, sum of square of all black
deviations is equal to SSE and is equal to the Sum of Square within
(SSW) in ANOVA. It has degree of freedom equal to n - no. of $$\beta$$s
and mean of SSE (MSE) is given by following equation.

$$\hat{Var(Error)}= \hat{\sigma ^2}= MSE= \frac{SSE}{n - \text{no. of } \beta s }$$

Once we have MSR, and MSE we can do a F-test similar to the one in
ANOVA.

$$F  = \frac{MSR}{MSE}$$

### Equivalent t-test for coefficients

It is also possible to statistically test if each of the coefficients
used in the model is equal to zero or not. OLS provides necessary
assumptions and properties to conduct a T-test on coefficients.

$$H_0 : \beta _ 1 = 0$$

$$H_1 : \beta _ 1 \neq  0$$

Although, I am showing for $$\beta _1 $$ same principle applies to all
the other coefficients in the model. z-statistic can be calculated using
following equation:

$$z =\frac{ \hat{\beta_1}-0}{\sqrt{\frac{\sigma^2}{Sxx}}}$$

Because we only have the estimate of $$\sigma^2$$, which is equal to
MSE, we can substitute that but our test statistic will now follow
t-distribution.

$$t= \frac{ \hat{\beta_1}-0}{\sqrt{\frac{MSE}{Sxx}}}$$

This t-statistics will have the same degree of freedom as MSE ie n - no.
of $$\beta$$s and the term $$\sqrt{\frac{MSE}{Sxx}}$$ is called
**Standard Error** in measure of $$\hat{\beta_1}$$

Lets now compare the result of anova and manually calculated values

``` r
aov(weight~height)
```

    ## Call:
    ##    aov(formula = weight ~ height)
    ## 
    ## Terms:
    ##                   height Residuals
    ## Sum of Squares  938.2682  330.4818
    ## Deg. of Freedom        1         2
    ## 
    ## Residual standard error: 12.85461
    ## Estimated effects may be unbalanced



REF
[1](https://statisticsbyjim.com/regression/interpret-constant-y-intercept-regression/)
[2](https://scholar.princeton.edu/sites/default/files/bstewart/files/lecture5handout.pdf)
