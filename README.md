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
```r
hypothesis_result <- t.test(mpg ~ am)
hypothesis_result$p.value
hypothesis_result$estimate
```
This demonstrates that cars with manual and automatic transmission have different mean MPG, and we reject the null hypothesis. 
#Regression Analysis
The first step is to model all variables against MPG. 

```{r echo = FALSE, message = FALSE}
fullModel <- lm(mpg ~ ., data=mtcars)
summary(fullModel)
```
Whilst this model explains 77.9% of MPG variation between all characteristics, none of the individual characteristics shows a statistically significant p-value (ie, p <= 0.05). This means that we should winnow our variables in search of the key drivers of MPG variability. 

```{r echo=FALSE, message = FALSE}
stepModel <- step(fullModel, k=log(nrow(mtcars)))
summary(stepModel)
```
The ammended model which results is
```r
#lm(formula = mpg ~ wt + qsec + am, data = mtcars)
```
However, our earlier exploratory analysis indicates autocorrelation between weight (wt) and automatic transmission (am1). To address this, we can adjust the model as follows:

```{r echo=FALSE, message=FALSE}
interactModel <-lm(mpg ~ wt + qsec + am + wt:am, data=mtcars)
summary(interactModel)
```

#Appendix 1
