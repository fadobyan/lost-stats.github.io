---
title: Logit Model
parent: Model Estimation
has_children: false
mathjax: true
nav_order: 1
---

# Logit Regressions

A logistical regression (Logit) is a statistical method for a best-fit line between a binary [0/1] outcome variable $$Y$$ and any number of independent variables. Logit regressions follow a [logistical distribution](https://en.wikipedia.org/wiki/Logistic_distribution) and the predicted probabilities are bounded between 0 and 1. 

For more information about Logit, see [Wikipedia: Logit](https://en.wikipedia.org/wiki/Logit).

## Keep in Mind
- The beta coefficients from a logit model are maximum likelihood estimations. They are not the marginal effect, as you would see in an OLS estimation. So you cannot interpret the beta coefficient as a marginal effect of $$X$$ on $$Y$$.
- To obtain the marginal effect, you need to perform a post-estimation command to discover the marginal effect. In general, you can 'eye-ball' the marginal effect by dividing the logit beta coefficient by 4.

# Implementations

## R
R can run a logit regression using the `glm()` function. However, to get marginal effects you will need to calculate them by hand or use a package. We will use the `mfx` package, although the `margins` package is another good option, which produces tidy model output.

```r
# If necessary, install the mfx package
# install.packages('mfx')
# mfx is only needed for the marginal effect, not the regression itself
library(mfx)

# Load mtcars data
data(mtcars)

# Use the glm() function to run logit
# Here we are predicting engine type using 
# miles per gallon and number of cylinders as predictors
my_logit <- glm(vs ~ mpg + cyl, data = mtcars,
                family = binomial(link = 'logit'))
# The family argument says we are working with binary data
# and using a logit link function (rather than, say, probit)

# The results
summary(my_logit)

# Marginal effects
logitmfx(vs ~ mpg + cyl, data = mtcars)
```

## Stata

```stata
* Load auto data
sysuse auto.dta

* Logit Estimation
logit foreign mpg weight headroom trunk

* Recover the Marginal Effects (Beta Coefficient in OLS)
margins, dydx(*)
```

## Gretl

```gretl
# Load auto data
open auto.gdt

# Run logit using the auto data, with mpg as the outcome variable
# and headroom, trunk, and weight as predictors
logit mpg const headroom trunk weight
```
