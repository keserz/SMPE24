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
meanMTdf <- read.csv("../data/mt_data.csv", header=T)
```

## Plotting the data together with the linear regression

``` r
ggplot(meanMTdf, aes(ID, MT)) +
  geom_point() +
  geom_smooth(method='lm')
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](pointingAnalysis_1_2_4_16_32_64_files/figure-gfm/Plotting%20the%20MT%20data%20together%20with%20the%20linear%20regression-1.png)<!-- -->
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
    ## -273.89  -86.56  -56.89  139.78  185.11 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)   904.22     200.08   4.519  0.00273 **
    ## ID             27.17      48.06   0.565  0.58953   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 166.5 on 7 degrees of freedom
    ## Multiple R-squared:  0.04366,    Adjusted R-squared:  -0.09296 
    ## F-statistic: 0.3195 on 1 and 7 DF,  p-value: 0.5895

From the replication of the experiment we can see that the ID doesn’t
seem to have an impact on the movement time, which correspond to the
finding of Celine Coutrix Additionally, the R²=0.44 i found here is the
same as the R² = 0.044 reported by the [experimental
software](http://ergo.human.cornell.edu/FittsLaw/FittsLaw.html). However
the a and b seem to differ, in my result i find a MT =
904.22+27.17\*log2(A/W+1) vs MT = 892.715 + 29.187log(A/W + 1) provided
by the webpage

## Loading data raw data given by Cornell University Ergonomics website

``` r
rawdf <- read.csv("../data/raw_data.csv", header=T)
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

![](pointingAnalysis_1_2_4_16_32_64_files/figure-gfm/Plotting%20the%20raw%20data%20together%20with%20the%20linear%20regression-1.png)<!-- -->

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
    ## -273.22  -88.09  -56.02  138.11  185.58 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)   
    ## (Intercept)   892.71     219.68   4.064  0.00479 **
    ## newID          29.19      51.66   0.565  0.58969   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 166.3 on 7 degrees of freedom
    ## Multiple R-squared:  0.04362,    Adjusted R-squared:  -0.09301 
    ## F-statistic: 0.3193 on 1 and 7 DF,  p-value: 0.5897

By using this analysis, we get the same MT = 982.71+29.19 \* log2(A/W+1)
with R2 = 0.44. The ID doesn’t seem to have an impact on the mean time
movement, but that can be explained by the fact that the task was too
difficult to begin with. The width of the target of 1, 2 and 4 pixel
were too small to be comfortably reached by the participant. The data
seem to suggest this idea, first of all 32 error out of 45 observation
were made which eliminate around 71% of total observation.
