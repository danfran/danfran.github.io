---
layout: post
title: "Find the slope in a linear model using R"
date:   2016-12-02
categories: r linear-model regression
---
Let's say we have an outcome `y` with and `x` as regressor passing through the origin with values:

```{r}
x <- c(0.8, 0.47, 0.51, 0.73, 0.36, 0.58, 0.57, 0.85, 0.44, 0.42)
y <- c(1.39, 0.72, 1.55, 0.48, 1.19, -1.59, 1.23, -0.65, 1.49, 0.05)
```

An easy way to calculate the slope of the linear regression in R is creating our data frame first:

```{r}
xy <- as.data.frame(cbind(y,x))
```

then using the `lm` (linear model) function:

```
lm(formula = y ~ x, data = as.data.frame(xy))
```

that returns:

```
Coefficients:
(Intercept)            x  
      1.567       -1.713
```

that returns the slope: `-1.713`
