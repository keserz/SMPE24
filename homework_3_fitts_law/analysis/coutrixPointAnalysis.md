PointingXP
================
Fouad Gasmi
02/01/2025

## Loading libraries

``` r
library(ggplot2)
library(dplyr)
```

## Loading data

``` r
meanMTdf <- read.csv("../data/20211117_1527_MeanMT.csv", header=T)
rawdf <- read.csv("../data/20211117_1527_RawData.csv", header=T)
```

## Plotting the data together with the linear regression

``` r
ggplot(meanMTdf, aes(ID, MT)) +
  geom_point() +
  geom_smooth(method='lm')
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](coutrixPointAnalysis_files/figure-gfm/Plotting%20the%20MT%20data%20together%20with%20the%20linear%20regression-1.png)<!-- -->
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
    ##    Min     1Q Median     3Q    Max 
    ## -381.4 -276.3   58.7  144.7  539.6 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept)   1108.4      351.1   3.158    0.016 *
    ## ID             111.0      107.5   1.032    0.336  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 320.5 on 7 degrees of freedom
    ## Multiple R-squared:  0.1321, Adjusted R-squared:  0.008146 
    ## F-statistic: 1.066 on 1 and 7 DF,  p-value: 0.3363

As Coutrix mentionned the ID doesn’t seem to have an impact on the
movement time and the results are different than the one provided by the
[experimental
software](http://ergo.human.cornell.edu/FittsLaw/FittsLaw.html) website,
MT = 904.22+27.17\*log2(A/W+1)with R2=0.008146 vs MT = 1001.293 +
140.589 × log(A/W + 1) with R2 = 0.218. The lack of impact of the ID on
the movement time could be explained by the fact that the target were
too small in order to apply fitts law here, additionally the R2 =
0.008146 in Coutrix result is the adjusted R squared while the webpage
provides the Multiple R squared value, but it still differs here, 0.1321
vs 0.218.

## Loading data raw data given by Cornell University Ergonomics website

``` r
rawdf <- read.csv("../data/20211117_1527_RawData.csv", header=T)
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

![](coutrixPointAnalysis_files/figure-gfm/Plotting%20the%20raw%20data%20together%20with%20the%20linear%20regression-1.png)<!-- -->

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
    ##     Min      1Q  Median      3Q     Max 
    ## -367.35 -275.12   34.85  139.45  541.87 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)  
    ## (Intercept)   1001.3      339.5   2.949   0.0214 *
    ## newID          140.6      100.7   1.396   0.2053  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 304.2 on 7 degrees of freedom
    ## Multiple R-squared:  0.2178, Adjusted R-squared:  0.1061 
    ## F-statistic: 1.949 on 1 and 7 DF,  p-value: 0.2053

By using this analysis, we get the same result as the webpage reported
in Coutrix journal.md MT = 1001.293 + 140.589 × log(A/W + 1) with R2 =
0.218.
