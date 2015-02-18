#Executive Summary
The goal of this analysis is to determine, using the dataset mtcars, whether the type of transmission (automatic vs manual) has an effect of MPG. There are two key questions: 
First, is manual or automatic transmission better for gas milage;
Second, what is the magnitude of this effect, if any.
The regression model demonstrates that cars with manual transmission have a higher MPG than automatic cars of the same weight and quarter-mile speed. 

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
The resulting plots are available as Figures 1-2 in Appendix 1. 
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
However, our earlier exploratory analysis indicates autocorrelation between weight (wt) and automatic transmission (am1) (Figure 2). To address this, we can adjust the model as follows:

```{r echo=FALSE, message=FALSE}
interactModel <-lm(mpg ~ wt + qsec + am + wt:am, data=mtcars)
summary(interactModel)
summary(interactModel)$coef
```
The adjusted R-squared for this model is .8804, meaning that it explains 88.04% of variation in MPG, making it superior to fullModel. Furthermore, all the variables show p-values < 0.05, indicating they have statistical significance within the model. 

The analysis of coeficients shows that independent of weight and speed (qsec), cars with manual transmission get 14.079 + (-4.141)*wt MPG than cars with automatic transmission.  

#Diagnostics & Residuals
The residuals vs. fitted plot shows no fixed pattern, indicating that the variables are behaving independently. Furthermore, the distribution of residuals indicates a good model fit, with no outliers (Figure 3). 

#Appendix 1
Figure 1. Boxplot of MPG vs transmission
```r
boxplot(mpg ~ am, xlab="Transmission (0 = Automatic, 1 = Manual)", ylab="MPG",
        main="Boxplot of MPG vs. Transmission")
```
Figure 2: Possible variable autocorrelation 
```r
ggplot(mtcars, aes(x=wt, y=mpg, group=am, color=am, height=3, width=3)) + geom_point() +  
scale_colour_discrete(labels=c("Automatic", "Manual")) + 
xlab("weight") + ggtitle("Scatter Plot of MPG vs. Weight by Transmission")
```
Figure 3: Plots of residuals
```r
par(mfrow = c(2, 2))
plot(amIntWtModel)
```
