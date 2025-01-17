---
title: Reshape Panel Data from Long to Wide
parent: Data Manipulation
has_children: false
nav_order: 1
---

# Reshape Panel Data from Long to Wide

Panel data is data in which individuals are observed at multiple points in time. There are two standard ways of storing this data:

In wide format, there is one row per individual. Then, for each variable in the data set that varies over time, there is one column per time period. For example:

| Individual | FixedCharacteristic | TimeVarying1990 | TimeVarying1991 | TimeVarying1992 |
|------------|---------------------|-----------------|-----------------|-----------------|
| 1          | C                   | 16              | 20              | 22              |
| 2          | H                   | 23.4            | 10              | 14              |

This format makes it easy to perform calculations across multiple years.

In long format, there is one row per individual per time period:

| Individual | FixedCharacteristic | Year | TimeVarying |
|------------|---------------------|------|-------------|
| 1          | C                   | 1990 | 16          |
| 1          | C                   | 1991 | 20          |
| 1          | C                   | 1992 | 22          |
| 2          | H                   | 1990 | 23.4        |
| 2          | H                   | 1991 | 10          |
| 2          | H                   | 1992 | 14          |

This format makes it easy to run models like [fixed effects](https://lost-stats.github.io/Model_Estimation/fixed_effects.html). 

Reshaping is the method of converting wide-format data to long and [vice versa](https://lost-stats.github.io/Data_Manipulation/reshape_panel_data_from_wide_to_long.html).

## Keep in Mind

- If your data has multiple observations per individual/time, then standard reshaping techniques generally won't work.
- It's a good idea to check your data by directly looking at it both before and after a reshape to check that it worked properly.

## Also Consider

- To go in the other direction, [reshape from wide to long](https://lost-stats.github.io/Data_Manipulation/reshape_panel_data_from_wide_to_long.html).
- [Determine the observation level of a data set](https://lost-stats.github.io/Data_Manipulation/determine_the_observation_level_of_a_data_set.html).

# Implementations

## R

There are many ways to reshape in R, including base-R `reshape` and the deprecated `reshape2::melt` and `cast` and `tidyr::gather` and `spread`. We will be using the `tidyr` package function `pivot_wider`, which requires `tidyr` version 1.0.0 or later.

```r
# install.packages('tidyr')
library(tidyr)

# Load in population, which has one row per country per year
data("population")

# If we look at the data, we'll see that we have:
# identifying information in "country",
# a time indicator in "year",
# and our values in "population"
head(population)
```

Now we think:
1. Think about the set of variables that contain the values we're interested in reshaping. Here's it's `population`. This list of variable names will be our `values_from` argument.
2. Think about what we want the new variables to be called. The `variable` variable says which variable we're looking at. So that will be our `names_from` argument. And we want to specify that each variable represents population in a given year (rather than some other variable, so we'll add "pop_" as our `names_prefix`.

```r
pop_wide <- pivot_wider(population,
                               names_from = year,
                               values_from = population,
                               names_prefix = "pop_")
```

## Stata

```stata
* Load blood pressure data in long format, which contains
* blood pressure both before and after a treatment for some patients
sysuse bplong.dta
```

The next steps involve thinking:
1. Think about the set of variables that identify individuals. Here it's `patient`. This will go in `i()`, so we have `i(patient)`.
2. Think about the set of variables that vary across time. Here's it's `bp`. This will be one of our "stub"s.
3. Think about which variable separates the different time periods within individual. Here we have "when", and this goes in `j()`, so we have `j(when)`.

```stata
* Syntax is:
* reshape wide stub, i(individualvars) j(newtimevar)
* So we have
reshape wide bp i(patient) j(when)
* Note that simply typing 
reshape
* will show the syntax for the function
```

With especially large datasets, the [Gtools](https://gtools.readthedocs.io/en/latest/index.html) package provides a much faster version of reshape known as greshape. The syntax can function exactly the same, though they provide alternative syntax that you may find more intuitive. 

```stata
* First, we will create a toy dataset that is very large to demonstrate the speed gains 
* If necessary, first install gtools:
* ssc install gtools

* Clear memory
clear all 
* Turn on return message to see command run time
set rmsg on 
* Set data size to 15 million observations
set obs 15000000 
* Create ten observations per person
generate person_id = floor((_n-1)/10)
* Number time periods from 1 to 10 for each person
generate time_id = mod((_n-1), 10) + 1

*Create an income in each period 
generate income = round(rnormal(100, 20))

* Demonstrate the comparative speed of these two Reshape approaches 
	
*The traditional reshape command
preserve 
reshape wide income, i(person_id) j(time_id) 
restore 
	
*The Gtools reshape command  
preserve
greshape wide income, i(person_id) j(time_id) 
restore 
	
*The Gtools reshape command, alternative syntax
preserve
greshape wide income, by(person_id) keys(time_id)
restore 
```

Note: there is much more guidance to the usage of greshape on the [Gtools reshape page](https://gtools.readthedocs.io/en/latest/usage/greshape/index.html). 