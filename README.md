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
The resulting plots are available as Figures [number-number] in Appendix 1. 

#Regression Analysis
#Appendix 1
