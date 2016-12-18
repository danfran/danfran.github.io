---
title: How to find out if an interaction should be included or not in our model
categories: r
tags: anova regression-model
---

Based on this quiz question from the Coursera course for Data Science:

***Consider the mtcars data set. Fit a model with mpg as the outcome that considers number of cylinders as a factor variable and weight as confounder. Now fit a second model with mpg as the outcome model that considers the interaction between number of cylinders (as a factor variable) and weight. Give the P-value for the likelihood ratio test comparing the two models and suggest a model using 0.05 as a type I error rate significance benchmark.***

Before we prepare out dataset converting the `cyl` (cylinders) variable to a factor, and creating the two candidates model `fit1` and `fit2` using the `lm` command:

```r
data(mtcars)
mtcars$cyl<- factor(mtcars$cyl)
fit1<- lm(mpg~cyl+wt,mtcars)
fit2<- lm(mpg~cyl+wt+cyl:wt,mtcars)
```
The model `fit2` is including the interaction variable `cyl:wt`. Let's see if it has any real value for our model using the `anova` function:

```r
> anova(fit1,fit2)
Analysis of Variance Table

Model 1: mpg ~ cyl + wt
Model 2: mpg ~ cyl + wt + cyl:wt
  Res.Df    RSS Df Sum of Sq      F Pr(>F)
1     28 183.06                           
2     26 155.89  2     27.17 2.2658 0.1239
```

The value `Pr(>F) ==> 0.1239` is higher than `0.05` and it is telling us that we can reject the hypothesis.
Alternatively we can use `lrtest`, a generic function for comparisons of models via asymptotic likelihood ratio tests:

```r
> library(lmtest)
> lrtest(fit1,fit2)
Likelihood ratio test

Model 1: mpg ~ cyl + wt
Model 2: mpg ~ cyl + wt + cyl:wt
  #Df  LogLik Df  Chisq Pr(>Chisq)  
1   5 -73.311                       
2   7 -70.741  2 5.1412    0.07649 .
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

`0.07` is still higher than `0.05` so we can reject in this case too.
