#Executive Summary

#Exploratory Analysis
The first step was to load the data an examine the variables. 
```r
library(ggplot2)
data(mtcars)
mtcars[1:3, ]
```
Once the variables were identified, I loaded all of the variables besides MPG as separate factors to allow some analysis of individual variables. 
```r
mtcars$cyl <- as.factor(mtcars$cyl)
mtcars$vs <- as.factor(mtcars$vs)
mtcars$am <- factor(mtcars$am)
mtcars$gear <- factor(mtcars$gear)
mtcars$carb <- factor(mtcars$carb)
attach(mtcars)
```
The resulting plots are available as Figures [number-number] in Appendix 1. 
Before proceeding we must test whether to accept or reject the null hypothesis. In this case, the null hypothesis is a sample of cars in this data set will have the same mean MPG regardless of whether they have manual or automatic transmission. 
#Regression Analysis
#Appendix 1
