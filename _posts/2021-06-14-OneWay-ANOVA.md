---
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: Eng_OneWayANOVA
title: "One-Way ANOVA: Learn by experimenting with data."  

# post specific
# if not specified, .name will be used from _data/owner.yml
author: Amrit Koirala
# multiple category is not supported
category: Statistics
# multiple tag entries are possible
tags: [ANOVA , population difference, F test]
# thumbnail image for post
img: ":OneWayAnova_files/figure-markdown/head one way ANOVA.svg"
# disable comments on this page
#comments_disable: true

# publish date
date: 2021-01-13 08:11:06 +0900

# seo
# if not specified, date will be used.
meta_modify_date: 2021-01-13 08:11:06 +0900
# check the meta_common_description in _data/lang/[language].yml
meta_description: "One-way ANOVA explained in easy way"

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

### Introduction
<!-- outline-start -->
  T-test is a popular method of comparing means between two populations
but if we want to extend it beyond two populations there is serious
decrease in power of test. On careful study of variance in multiple
populations, early statisticians like Ronald Fisher found that analysis
of variation can be used to check if there is difference in means of
multiple populations which is popularly known as ANOVA in short.<!-- outline-end --> For
example, in the figure below (fig 1) we can see distribution of test
score in the population of four groups of students under two scenarios.
First scenario in the left is when variance is small and second scenario
on the right is when variance is large. Same group of students have same
mean in both scenarios. Eyeballing these plots, we can say that the
populations are more separated from each other in the first scenario
than in the second. What does this separation means in the statistical
terms is the difference in means in the populations in the left is more
obvious than the one on the right. The more overlap is there among these
populations less confident we are on the difference of their means.
ANOVA provides the statistical framework of determining this confidence
using just one sample from these populations. It compares if the
variance between groups is significantly greater than the variance
within the groups.

{% capture text-capture %}
``` r
set.seed(3456)
par(mfrow=c(1,2), mai= c(1,0,1,0))
#population defined by mean and variance 
poplist <- list(pop1=c(22, 4) , pop2= c(25, 4), pop3 = c(30, 6), pop4= c(34,3))
pop_col = c("blue", "darkgreen", "red", "purple")
shade = c(rgb(0,0,255, max=255,alpha=100),
          rgb(0, 238, 0,max=255, alpha=100),
              rgb(173,0,0,max=255, alpha=100),
              rgb(160,32,240, max=255,alpha=100))
  
plot(x=0,y=0,xlim= c(10,48), ylim=c(0,0.25), pch=NULL,  ylab= "", xlab="score", main="Distribution when variance is low \n means = (22, 25, 30, 34) \n variance = (4, 4, 6, 3) ", yaxt="n")
for(i in seq_along(poplist)){
  polygon(c(seq(10, 40, 0.5),10), c(dnorm(seq(10, 40, 0.5), mean =poplist[[i]][1], sd=sqrt(poplist[[i]][2])),0), col=shade[i])
    curve(dnorm(x, mean=poplist[[i]][1], sd=sqrt(poplist[[i]][2])), col=pop_col[i], lwd=2,add=T)
}

#population defined by mean and variance 
poplist <- list(pop1=c(22, 10) , pop2= c(25, 11), pop3 = c(30, 12), pop4= c(34,13))
pop_col = c("blue", "darkgreen", "red", "purple")
  
plot(x=0,y=0,xlim= c(10,48), ylim=c(0,0.25), pch=NULL,  ylab="",xlab="score", main="Distribution when variance is high \n means = (22, 25, 30, 34) \n variance = (10, 11, 12, 13) ", yaxt="n")
for(i in seq_along(poplist)){
   polygon(c(seq(10, 50, 0.5),10), c(dnorm(seq(10, 50, 0.5), mean =poplist[[i]][1], sd=sqrt(poplist[[i]][2])),0), col=shade[i])
    curve(dnorm(x, mean=poplist[[i]][1], sd=sqrt(poplist[[i]][2])), col=pop_col[i], lwd=2,add=T)
}
legend(30,0.25, legend = c("Group A ", "Group B", "Group C", "Group D"), col= c("blue", "darkgreen", "red", "purple"), lty= c(1,1,1,1), lwd=2)
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof23" button-text="R code for plot" toggle-text=text-capture  footer="done!" %}

![](:OneWayAnova_files/figure-markdown/unnamed-chunk-1-1.svg)

Fig 1: Distribution of population with different variance.

### Data collection

We will use data from the same populations we discussed in the
introduction. We will select a small sample of size(n)= 5 from each
population in each scenario. Using R lets get such samples and see the
distribution of the values in each sample using box plot (fig 2).

{% capture text-capture %}

``` r
set.seed(3456)
par(mfrow=c(1,2))
###Scenario I sampling 
levelA_1<- round(rnorm(5, 22, 4),0)
levelB_1 <- round(rnorm(5, 25,4),0)
levelC_1<- round(rnorm(5,30,6),0)
levelD_1<- round(rnorm (5, 34, 3),0)
levels <- c(rep("A",5),rep("B",5),rep("C",5),rep("D",5))
boxplot(c(levelA_1,levelB_1,levelC_1, levelD_1)~levels, col=pop_col, ylim=c(6,60), main="Scenario I", ylab="score", border="azure4")
points(c(mean(levelA_1),mean(levelB_1), mean(levelC_1), mean(levelD_1) ), pch =18, col=1, cex=2)
abline(h=mean(c(levelA_1,levelB_1,levelC_1, levelD_1)), col="blue")
stripchart(c(levelA_1,levelB_1,levelC_1, levelD_1)~levels, add =TRUE, vertical =TRUE, 
           method= "jitter", col="darkgrey", pch=19)


##Scenario II sampling 
levelA_2<- round(rnorm(5, 22, 10),0)
levelB_2 <- round(rnorm(5, 25,11),0)
levelC_2<- round(rnorm(5,30,12),0)
levelD_2<- round(rnorm (5, 34, 13),0)
levels <- c(rep("A",5),rep("B",5),rep("C",5),rep("D",5))
boxplot(c(levelA_2,levelB_2,levelC_2, levelD_2)~levels, col=pop_col, ylim=c(6,60), main= "Scenario II", ylab="score",border="azure4")
points(c(mean(levelA_2),mean(levelB_2), mean(levelC_2), mean(levelD_2) ), pch =18, col=1, cex=2)
abline(h=mean(c(levelA_2,levelB_2,levelC_2, levelD_2)), col="blue")
stripchart(c(levelA_2,levelB_2,levelC_2, levelD_2)~levels, add =TRUE, vertical =TRUE, 
           method= "jitter", col="darkgray", pch=19)
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof24" button-text="code here" toggle-text=text-capture  footer="done!" %}


![](:OneWayAnova_files/figure-markdown/unnamed-chunk-2-1.svg)

Fig 2: Box-plot of samples from two scenarios. 

In the box-plot above, the samples from the population shown in fig 1
are shown according to their group color in two different scenarios. The
black diamond in each group is the group mean for that group and the
blue horizontal line is the grand mean for all observations. Samples
from two scenarios clearly reflect the difference in the
variance in their population. Following the population variance, the
observations in left are located close to the mean and in the right are
more spread out. To get an idea about the difference in the means of
these groups we calculated mean of each group (shown as black diamond).
Group means in first scenario are 23.2, 25.0, 28.2, 33.2 in group A, B,
C, and D respectively and in the second scenario it is 31.6, 24.2, 25.2,
38.8 in the group A, B, C, and D respectively. Now we need to determine
if there is any difference in the mean of populations they are sampled from.

### Statement of the problem :

Now that we have seen the data we will need to work with, lets define
the question we are trying to solve. For each scenario, we saw some
differences in the group means for these four samples from each group of
students. But given these 4 samples (one from each group), do we have
enough evidence to say the population means for these four groups are
different ?

### Steps in ANOVA

#### Key definitions

We want to tackle this question by analysis of variance, ie we want to
see, of the total variance in these scores how much of it comes from
within the groups and how much of it comes from between the groups.

 **1.Total variance:** It is the measure of total variance in the data
(score). If we calculate the variance using the deviations of each
observation from the grand mean it is called total variance and the sum
of square used is called Total sum of squares (TSS). It has degree of
freedom equal to n-1 just like normal sample variance. 

**2. Variance within:** It is the measure of variance within each group. The deviation
used is between each observation and its respective group mean. Sum of
squares is called sum of square within (SSW). Since mean of each group
was used as an estimate of population mean for that group it has n-t
degree of freedom. See notes on [degree of freedom](2019-07-01-Degree-of-freedom) for details about
how to calculate df. Variance within is also referred as error as this
is the part of total variance which could not be explained by our
factor. 

**3. Variance between:** It is the amount of variance explained
by our factor. So, it is calculated using the deviations of group mean
from the grand mean. It has degree of freedom equal to t-1 and sum of
square is called sum of square between (SSB). This represents the
variance explained by our factor.

Figure below shows the relationships between these sum of squares in signal to noise analogy. TSS is proportional to the total variance in the random variable which is represented as the total background noise. When we introduce some categorical variable (groups), some of this variance in the random variable is explained by the groups, giving us some signal that this categorical variable has some effect on the random variable (represented as a pattern). If this signal takes higher proportion of TSS, then most of the variance can be attributed to the effect. For each groups, there is always some level of difference in the observations in the groups, which represents the amount of total variance that couldn't be explained by our groups. The total sum of squares is always equal to sum of SSB and SSW. 

![Sum of squares in oneway ANOVA](:OneWayAnova_files/figure-markdown/SUMS one way ANOVA.svg)

Fig 3: Understanding the partitioning of sum of squares in one-way ANOVA.

#### Calculation of SS and mean SS.

To illustrate the calculation of Sum of square we will look into the
data from samples of Second scenario in more details. Figure below
illustrates the calculation of SS.

![Calculation of sum of squares](:OneWayAnova_files/figure-markdown/AnovaSS.svg)

SSB is calculated using the black deviations in the above figure
(deviation between grand mean and the group mean). In this example there
are 5 samples in each level so the square of deviation needs to be
multiplied by 5 before adding for each level.

SSW is calculated group wise, using the deviation between each
observation and the group mean (deviation represented by each group
color).

SST is just like regular variance using every observation in this data.
So the deviation used will be between each observation and the grand
mean. Now that we know how to calculate these sums lets calculate them
for our samples using R

{% capture text-capture %}
``` r
# Scenario I Sum of squares 
grand_mean_I <- mean(c(levelA_1,levelB_1,levelC_1, levelD_1))
group_mean_I <- c(mean(levelA_1),mean(levelB_1), mean(levelC_1), mean(levelD_1) )
#TSS
deviations <- c(levelA_1, levelB_1, levelC_1, levelD_1)-grand_mean_I 
TSS <- sum(sapply(deviations, function(x)x^2 ))
cat("TSS = ", TSS, "\n")
```
``` r
#SSW
deviations <- group_mean_I - grand_mean_I
SSB <- sum((sapply(deviations, function(x)x^2))*5)
cat("SSB = ", SSB, "\n")
```
``` r
#SSW
deviations <- c(levelA_1 - group_mean_I[1],
                levelB_1 - group_mean_I[2],
                levelC_1 - group_mean_I[3],
                levelD_1 - group_mean_I[4])
SSW <- sum((sapply(deviations, function(x)x^2)))
cat("SSW = ", SSW, "\n")
``` 
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof25" button-text="click to view calculation in R" toggle-text=text-capture  footer="done!" %}

    ## TSS =  594.8
    ## SSB =  288.4
    ## SSW =  306.4

#### F-test

Once we have all sum of squares we can calculate the mean sum of squares
by dividing the SS with degree of freedom. And since this mean sum of
square is only the estimate of corresponding variance in the population
we need to do formal hypothesis testing to see if it is statistically
significant. Since we are comparing to variance we use F-test.

$$F-statistic = \frac {\frac{SSB}{t-1}}{\frac {SSW}{n-t}}$$

$$= \frac{MSB}{MSW}$$

{% capture text-capture %}
``` r
##Scenario I F-test
F_statistic <- (SSB/(4-1))/(SSW/(20-4)) 
F_statistic
p_val <- pf(F_statistic, 3, 16, lower.tail = F)
p_val
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof26" button-text="calculation in R" toggle-text=text-capture  footer="done!" %}

    ## F-statistic =  5.020017
    ## p-value = 0.01217514

We did it manually to illustrate the process. R has build in function to
perform ANOVA. Lets use that build in function to perform ANOVA for the
samples from both scenario.

{% capture text-capture %}
``` r
cat("#######---Scenario I----###########\n")
summary(aov(c(levelA_1,levelB_1,levelC_1, levelD_1)~levels))
cat("\n\n########----Scenario II----#######\n")
summary(aov(c(levelA_2,levelB_2,levelC_2, levelD_2)~levels))
```
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof28" button-text="code here" toggle-text=text-capture  footer="done!" %}

    ## #######---Scenario I----###########

    ##             Df Sum Sq Mean Sq F value Pr(>F)  
    ## levels       3  288.4   96.13    5.02 0.0122 *
    ## Residuals   16  306.4   19.15                 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## 
    ## ########----Scenario II----#######
    ##             Df Sum Sq Mean Sq F value Pr(>F)
    ## levels       3  683.4   227.8   1.604  0.228
    ## Residuals   16 2271.6   142.0


As we expected, because the samples in second scenario came from the
populations where there was more variance and they did not separate
well, our test came as insignificant. Only for the first scenario we can
say there is difference between means of these groups of students (given
the p-value of 0.012, there is less than 2% chance of error when we say
so). If we want to say same statement for scenario II, there is 22.8%
chance that we will be wrong. That is why test in second scenario is not
significant. It is very important to note that, true mean of groups in
our population is same in both scenarios and there is difference too.
Because of the difference in variance in these scenarios, samples from
first scenario has enough information to reflect that difference in
population mean but in second scenario that difference in group means is
lost in the noise added by high variance in that population. So our
sample in second case doesn't give us any conclusion, that is why
insignificant results should not be interpreted is no different in
means. It simply means the samples we are using do not have enough
information to say there is difference in population means. In our
situation, increasing the sample size to 30 will resolve the problem in
second scenario as well.

{% capture text-capture %}
``` r
###Scenario II large sample size
summary(aov(c(round(rnorm(30, 22, 10),0)
              ,round(rnorm(30, 25,11),0),
              round(rnorm(30,30,12),0),
              round(rnorm (30, 34, 13),0))~ c(rep("A",30),rep("B",30),rep("C",30),rep("D",30))))
```

    ##                                                            Df Sum Sq Mean Sq
    ## c(rep("A", 30), rep("B", 30), rep("C", 30), rep("D", 30))   3   5001  1667.0
    ## Residuals                                                 116  18849   162.5
    ##                                                           F value   Pr(>F)    
    ## c(rep("A", 30), rep("B", 30), rep("C", 30), rep("D", 30))   10.26 4.83e-06 ***
    ## Residuals                                                                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof29" button-text="See code for details" toggle-text=text-capture  footer="done!" %}


Will increasing the sample size always makes test significant ?

Not really. If the samples are from the population with same mean, we
cannot get significant result just by increasing the sample size.

{% capture text-capture %}

``` r
summary(aov(c(round(rnorm(300, 22, 10),0)
              ,round(rnorm(300, 22,11),0),
              round(rnorm(300,22,12),0),
              round(rnorm (300, 22, 13),0))~ c(rep("A",300),rep("B",300),rep("C",300),rep("D",300))))
```

    ##                                                                 Df Sum Sq
    ## c(rep("A", 300), rep("B", 300), rep("C", 300), rep("D", 300))    3    917
    ## Residuals                                                     1196 159133
    ##                                                               Mean Sq F value
    ## c(rep("A", 300), rep("B", 300), rep("C", 300), rep("D", 300))   305.5   2.296
    ## Residuals                                                       133.1        
    ##                                                               Pr(>F)  
    ## c(rep("A", 300), rep("B", 300), rep("C", 300), rep("D", 300)) 0.0761 .
    ## Residuals                                                             
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

{% endcapture %}

{% include widgets/toggle-field-code.html toggle-name="toggle-proof30" button-text="See code for details" toggle-text=text-capture  footer="done!" %}

### Conclusion

One way ANOVA is very efficient tool to check if there is any effect of
a factor we are interested. Effect of factor can be tested by performing
ANOVA across different levels within that factor. The table below
summarizes different sums of squares and degree of freedom used in
one-way ANOVA. See my notes on Two-way ANOVA to learn more about ANOVA.


|Source  |  df  |            SS |  Mean SS|F
| --------- |---------------------- |----------------------------------- |-----|
|Between    |t-1  | SSB            | MSB= SSB/t-1    |F= MSB/MSW
|Within  | n-t  | SSW            |   MSW=  SSW/n-t  
|Total  | n-1  | TSS   |          
