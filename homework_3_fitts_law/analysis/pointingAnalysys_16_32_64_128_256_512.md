PointingXP
================
Fouad Gasmi
02/01/2025

## Loading libraries

``` r
library(ggplot2)
library(dplyr)
```

## Loading data pre processed by Cornell University Ergonomics website

``` r
meanMTdf <- read.csv("../data/mt_data2.csv", header=T)
```

## Plotting the data together with the linear regression

``` r
ggplot(meanMTdf, aes(ID, MT)) +
  geom_point() +
  geom_smooth(method='lm')
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](pointingAnalysys_16_32_64_128_256_512_files/figure-gfm/Plotting%20the%20MT%20data%20together%20with%20the%20linear%20regression-1.png)<!-- -->
\## Linear modelling

``` r
model <- lm(MT~ID, data = meanMTdf)
summary(model)
```

    ## 
    ## Call:
    ## lm(formula = MT ~ ID, data = meanMTdf)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -175.75  -40.88  -23.88   50.25  142.12 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)   178.50     111.80   1.597  0.15438   
    ## ID            172.12      34.23   5.028  0.00152 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 102.1 on 7 degrees of freedom
    ## Multiple R-squared:  0.7832, Adjusted R-squared:  0.7522 
    ## F-statistic: 25.28 on 1 and 7 DF,  p-value: 0.001516

In this experiment we can see that the ID seems to have an impact on the
movement time. However the R²=0.783 find here is different from the
R²=0.884 reported by the [experimental
software](http://ergo.human.cornell.edu/FittsLaw/FittsLaw.html). The
result provided by the website were : MT =132.970 + 180.536log(A/W + 1)
with RSquare = 0.884

## Loading data raw data given by Cornell University Ergonomics website

``` r
rawdf <- read.csv("../data/raw_data2.csv", header=T)
```

## Applying Fitt’s law formula to get the ID

``` r
rawdf$newID <- (log2((rawdf$A / rawdf$W) +1))
```

## Computing the mean MT for each unique combination of A, W, and ID

``` r
newmeanMTdf <- rawdf %>%
  group_by(A, W, newID) %>%
  summarise(mean_MT = mean(MT), .groups = "drop")
```

## Plotting the data together with the linear regression

``` r
ggplot(newmeanMTdf, aes(newID, mean_MT)) +
  geom_point() +
  geom_smooth(method='lm')
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](pointingAnalysys_16_32_64_128_256_512_files/figure-gfm/Plotting%20the%20raw%20data%20together%20with%20the%20linear%20regression-1.png)<!-- -->

## Linear modelling

``` r
model <- lm(mean_MT~newID, data = newmeanMTdf)
summary(model)
```

    ## 
    ## Call:
    ## lm(formula = mean_MT ~ newID, data = newmeanMTdf)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -72.33 -51.37 -34.59  31.89 131.97 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   132.97      83.25   1.597 0.154242    
    ## newID         180.54      24.69   7.311 0.000161 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 74.6 on 7 degrees of freedom
    ## Multiple R-squared:  0.8842, Adjusted R-squared:  0.8677 
    ## F-statistic: 53.46 on 1 and 7 DF,  p-value: 0.0001612

And now by using the raw data and applying the different operation, we
get the same result provided by the webpage MT = 132.970 +
180.536log(A/W + 1), RSquare = 0.884.
