quickSort_IC_analysis
================
2025-01-02

``` r
knitr::opts_chunk$set(echo = TRUE)
```

## Introduction

The goal of this analysis is to explore how the runtime of the QuickSort
algorithm varies in function of the size for 3 different type of
algorithms: Sequential, Parralel and Built in. My prymary focus is to
compute the confidence interval for the runtime data with varying size.
Plotting these interval allows to assess the untertainty in the mean
runtime and to compare the 3 types of algorithms.

## loading libraries

``` r
library(dplyr)
library(ggplot2)
```

## Loading the data

``` r
data <- read.csv("../data/measurements_03_47.csv", header = TRUE)
```

The dataset contains 3 columns:  
- Size : Which is the size of the input for the algorithm here array
length i believe.  
- Type : The algorithm used (e.g., sequential, parralel, built in)  
- Time : The runtime for each size and type

As stated before, the goal is to compute the mean runtime and the
confidence interval for each combination of size and type to get some
insight concerning the uncertainty in runtime measurements.

## Compute mean, standard deviation, and confidence intervals

``` r
df_sum <- data %>%
  group_by(Type, Size) %>%
  summarise(
    mean = mean(Time),
    sd = sd(Time),
    n = n(),
    se = sd / sqrt(n),
    .groups = "drop"
  )
```

The first step is to group the data by type and size to ensure the
computation separately for each type and size combination

    df_sum <- data %>%
      group_by(Type, Size) %>%

The mean is given by where n = the number of observation for each
combinations $$
\text{mean} = \frac{1}{n} \sum_{i=1}^{n} \text{Time}_i
$$

    mean = mean(Time)

then we compute the standart deviation which provide a measure of the
variability in runtime for each combination $$
\text{sd} = \sqrt{\frac{1}{n-1} \sum_{i=1}^{n} (x_i - \text{mean})^2}
$$

    sd = sd(Time)

the standart error of the mean providing a measure of the uncertainty in
the estimation of the mean $$
\text{se} = \frac{\text{sd}}{\sqrt{n}}
$$

    se = sd / sqrt(n)

## Verifiying normality

``` r
# Histogram of the data to check normality
ggplot(data, aes(x = Time)) + 
  geom_histogram(binwidth = 0.01, fill = "blue", color = "black") + 
  labs(title = "Histogram of Runtime Data", x = "Runtime", y = "Frequency") +
  theme_minimal()
```

![](quickSort_IC_analysis_files/figure-gfm/checking%20normality-1.png)<!-- -->

``` r
# Q-Q plot to check for normality
ggplot(data, aes(sample = Time)) +
  geom_qq() +
  geom_qq_line() +
  labs(title = "Q-Q Plot for Normality Check") +
  theme_minimal()
```

![](quickSort_IC_analysis_files/figure-gfm/checking%20normality-2.png)<!-- -->

From these plots we can say that the distribution do not follow a normal
distribution which mean that we cannot use the CLT or the student test
to compute the confidence intervals

## Quantile based confidence interval

This approach is usefull when the distribution is not defined or unsure.
It does not assume any particular distribution for the data and uses the
empirical distribution instead.

``` r
df_sum <- df_sum %>%
  rowwise() %>%
  mutate(
    lower = quantile(data$Time[data$Type == Type & data$Size == Size], 0.025),
    upper = quantile(data$Time[data$Type == Type & data$Size == Size], 0.975)
  )
```

## Ploting the confidence interval

``` r
# Plot the data with the confidence interval
ggplot(df_sum, aes(x = Size, y = mean, color = Type)) +
  geom_point() +
  geom_errorbar(aes(ymin = lower, ymax = upper), width = 0.1) +
  geom_line() +
  scale_x_log10() +
  
  labs(title = "95% Confidence Interval (Quantiles) with Log10 Scale", x = "Size", y = "Mean Time") +
  theme_minimal()
```

![](quickSort_IC_analysis_files/figure-gfm/ploting%20CI-1.png)<!-- -->

The log scale was applied to the x-axis to better visualize the data, as
the ‘Size’ variable spans a wide range of values, from 100 to 1,000,000.
This transformation helps compress the scale and makes it easier to
observe patterns and trends across the values, particularly when there
is a large disparity between the smallest and largest sizes.
