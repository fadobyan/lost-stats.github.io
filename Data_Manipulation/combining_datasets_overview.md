---
title: Combining Datasets Overview
parent: Data Manipulation
has_children: false
nav_order: 1
---

# Guide to Different Combination Types: 

- [Vertical combination](https://lost-stats.github.io/Data_Manipulation/combining_datasets_vertical_combination.html)
- [Horizontal Combination (Deterministic)](https://lost-stats.github.io/Data_Manipulation/combining_datasets_horizontal_combination_deterministic.html)

# Different Ways to Combine Datasets

There are two main ways to combine data: vertically and horizontally. That is, you can want to combine observations (adding new variables) or combine variables (adding new observations). This is perhaps easiest to show visually: 

<table>
<tr><th> Individual Name Info </th><th> Individual Age Info </th></tr>
<tr><td>

|Name| ID |
|--|--| 
|John Smith|A63240|
|Desiree Thomas|B78242|

</td><td>

|ID | Age | 
|--|--| 
|B78242|22|
|A63240|27|

</td></tr> </table>

In the case above, we would like to combine two datasets, the Individual Name Info and the Individual Date Info, that have different information about the same people, who are identified by the ID variable. The result from the merge would be to have a new dataset with more columns than the original datasets because it contains all of the information for each individual from both of the original datasets. Here we have to combine the files according to the ID variable, placing the information from observations with the same ID on the same row in the combined dataset. 

Alternatively, the below example has two datasets that collect the same information about different people. We would like to combine these datasets vertically, with the result containing more rows than the original dataset, because it contains all of the people that are present in each of the original datasets. Here we combine the files based on the name or position of the columns in the dataset. 

|Name|ID|Age|
|--|--|--|
|John Smith|A63240|22|
|Desiree Thomas|B78242|27|

|Name|ID|Age|
|--|--|--|
|Teresa Suarez|Y34208|19|
|Donald Akliberti|B72197|34|

These ways of combining data are referred to by different names across different programming languages, but will largely be referred to by one common set of terms (used by Stata and Python’s Pandas): merge for horizontal combination and append for for vertical combination. 

