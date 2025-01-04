analysis_challenger
================
2025-01-04

## Loading libraries

``` r
library(tidyverse)
library(ggplot2)
```

## Loading data

``` r
data <- read.csv("../data/shuttle.csv", header = TRUE)
```

### Adding frequency of O-rings failure

``` r
data$Frequency <- data$Malfunction / data$Count
datarm <- data %>%
  filter(Malfunction > 0)
```

### Scatter plot of Temperature vs Frequency of failure

``` r
# Add a new column to identify the dataset
data$Source <- "All Data"
datarm$Source <- "Filtered Data"

# Combine the two datasets into one
combined_data <- bind_rows(data, datarm)

# Create the plot with facet_wrap to display both datasets side by side
ggplot(combined_data, aes(x = Temperature, y = Frequency)) +
  geom_point() +
  labs(title = "Temperature vs O-ring Failure Frequency",
       x = "Temperature (°F)",
       y = "Failure Frequency") +
  theme_minimal() +
  facet_wrap(~ Source, scales = "free_y")  # Display two panels with separate y-axis for each dataset
```

![](analysis_challenger_files/figure-gfm/scatter%20plot-1.png)<!-- -->

From these graph we can see that for the non filtered data there seem to
be a trend regarding temperature which is imperceptible with the
filtered data.

## Logistic regression only with temperature

``` r
logmodel <- glm(Frequency ~ Temperature, 
                data = data, 
                family = binomial(link = "logit"))
```

    ## Warning in eval(family$initialize): nombre de succès non entier dans un glm binomial !

``` r
logmodelrm <- glm(Frequency ~ Temperature, 
                data = datarm, 
                family = binomial(link = "logit"))
```

    ## Warning in eval(family$initialize): nombre de succès non entier dans un glm binomial !

``` r
summary(logmodel)
```

    ## 
    ## Call:
    ## glm(formula = Frequency ~ Temperature, family = binomial(link = "logit"), 
    ##     data = data)
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)    5.085      7.477    0.68     0.50
    ## Temperature   -0.116      0.115   -1.00     0.32
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 4.0384  on 22  degrees of freedom
    ## Residual deviance: 3.0144  on 21  degrees of freedom
    ## AIC: 7.203
    ## 
    ## Number of Fisher Scoring iterations: 6

``` r
summary(logmodelrm)
```

    ## 
    ## Call:
    ## glm(formula = Frequency ~ Temperature, family = binomial(link = "logit"), 
    ##     data = datarm)
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)
    ## (Intercept) -1.38953    7.82796   -0.18     0.86
    ## Temperature  0.00142    0.12192    0.01     0.99
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 0.22245  on 6  degrees of freedom
    ## Residual deviance: 0.22231  on 5  degrees of freedom
    ## AIC: 7.376
    ## 
    ## Number of Fisher Scoring iterations: 4

In either of these data set (filtered vs unfiltered) temperature doesn’t
show significance with a P value = 0.5 for the unfiltered data and 0.86
for the filtered data, at first this could suggest that removing data
doesn’t have an impact in the significance of the temperature. However
in this analysis the pressure which is part of this dataset has not been
taken into account

## Logistic regression with temperature and pressure

``` r
model_2 <- glm(cbind(Malfunction, Count - Malfunction) ~ Temperature + Pressure, 
               data = data, 
               family = binomial(link = "logit"))

model_2rm <- glm(cbind(Malfunction, Count - Malfunction) ~ Temperature + Pressure, 
               data = datarm, 
               family = binomial(link = "logit"))

summary(model_2)
```

    ## 
    ## Call:
    ## glm(formula = cbind(Malfunction, Count - Malfunction) ~ Temperature + 
    ##     Pressure, family = binomial(link = "logit"), data = data)
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)  
    ## (Intercept)  2.52019    3.48678    0.72    0.470  
    ## Temperature -0.09830    0.04489   -2.19    0.029 *
    ## Pressure     0.00848    0.00768    1.11    0.269  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 24.230  on 22  degrees of freedom
    ## Residual deviance: 16.546  on 20  degrees of freedom
    ## AIC: 36.11
    ## 
    ## Number of Fisher Scoring iterations: 5

``` r
summary(model_2rm)
```

    ## 
    ## Call:
    ## glm(formula = cbind(Malfunction, Count - Malfunction) ~ Temperature + 
    ##     Pressure, family = binomial(link = "logit"), data = datarm)
    ## 
    ## Coefficients:
    ##             Estimate Std. Error z value Pr(>|z|)
    ## (Intercept) -2.25231    4.03615   -0.56     0.58
    ## Temperature  0.00724    0.05196    0.14     0.89
    ## Pressure     0.00273    0.00816    0.33     0.74
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 1.3347  on 6  degrees of freedom
    ## Residual deviance: 1.2162  on 4  degrees of freedom
    ## AIC: 20.78
    ## 
    ## Number of Fisher Scoring iterations: 4

When we apply the logistic regression considering temperature and
pressure, the analysis reveals that temperature has a significant impact
on the probability of O-ring failure with the full dataset with a p
value = 0.029. The negative coefficient suggest that as temperature
increases, the probability of failure decreases.

However when the same model is applied to the filtered dataset,
temperature doesn’t show any significance. This shift suggest that
removing datas reduced the model’s ability to capture meaningfull
information leading to a loss of critical information.

So far we’ve tried to include temperature and pressure and since the
original dataset doesn’t include any other variable, the only thing left
we could explore is the interaction between these two variables.

## logistic regression of temperature \* pressure

``` r
model_3 <- glm(cbind(Malfunction, Count - Malfunction) ~ Temperature * Pressure, 
               data = data, 
               family = binomial(link = "logit"))

model_3rm <- glm(cbind(Malfunction, Count - Malfunction) ~ Temperature * Pressure, 
               data = datarm, 
               family = binomial(link = "logit"))

summary(model_3)
```

    ## 
    ## Call:
    ## glm(formula = cbind(Malfunction, Count - Malfunction) ~ Temperature * 
    ##     Pressure, family = binomial(link = "logit"), data = data)
    ## 
    ## Coefficients:
    ##                       Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)          -21.76555   46.28561   -0.47     0.64
    ## Temperature            0.25246    0.66299    0.38     0.70
    ## Pressure               0.13086    0.23264    0.56     0.57
    ## Temperature:Pressure  -0.00177    0.00334   -0.53     0.60
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 24.230  on 22  degrees of freedom
    ## Residual deviance: 16.257  on 19  degrees of freedom
    ## AIC: 37.82
    ## 
    ## Number of Fisher Scoring iterations: 6

``` r
summary(model_3rm)
```

    ## 
    ## Call:
    ## glm(formula = cbind(Malfunction, Count - Malfunction) ~ Temperature * 
    ##     Pressure, family = binomial(link = "logit"), data = datarm)
    ## 
    ## Coefficients: (1 not defined because of singularities)
    ##                      Estimate Std. Error z value Pr(>|z|)
    ## (Intercept)          -2.25231    4.03615   -0.56     0.58
    ## Temperature           0.00724    0.05196    0.14     0.89
    ## Pressure              0.00273    0.00816    0.33     0.74
    ## Temperature:Pressure       NA         NA      NA       NA
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 1.3347  on 6  degrees of freedom
    ## Residual deviance: 1.2162  on 4  degrees of freedom
    ## AIC: 20.78
    ## 
    ## Number of Fisher Scoring iterations: 4

In the full dataset, neither temperature nor pressure shows significance
toward the probability of O-ring failure, nor does the interaction
between these two variables. In the filtered dataset, the situation
becomes even more uncertain, as the model couldn’t estimate the
interaction term, likely due to the small sample size. Overall,
incorporating the interaction between temperature and pressure seems to
overcomplicate the model, reducing its predictive power compared to the
second model. Therefore, the analysis of O-ring failure estimation at
31°F will focus on the second model, where temperature and pressure are
considered independently.

## Estimation of O-rings failure with a temperature of 31°F

``` r
temperature_value <- 31

mean_pressure <- mean(data$Pressure, na.rm = TRUE)
mean_pressurerm <- mean(datarm$Pressure, na.rm = TRUE)

# Since the original report doesn't mention the pressure at take-off, we will use the mean pressure
new_data <- data.frame(Temperature = temperature_value, Pressure = mean_pressure)
new_datarm <- data.frame(Temperature = temperature_value, Pressure = mean_pressurerm)


predicted_prob_m2 <- predict(model_2, newdata = new_data, type = "response")
predicted_prob_m2rm <- predict(model_2rm, newdata = new_datarm, type = "response")


cat("(full dataset) Estimated probability of malfunction at 31°F:", predicted_prob_m2, "\n")
```

    ## (full dataset) Estimated probability of malfunction at 31°F: 0.6822

``` r
cat("(filtered dataset) Estimated probability of malfunction at 31°F:", predicted_prob_m2rm, "\n")
```

    ## (filtered dataset) Estimated probability of malfunction at 31°F: 0.1764

The logistic regression model (temperature + pressure) on the full
dataset shows a high probability of O-ring malfunction at 31°F of 68%.
Which is consistent with this model showing the significance of the
temperature on O-ring failure. However in the filtered dataset, the
model showed no significance of the temperature and now shows a
probability of failure of 17%. But we have to remember that any
successfull launch was removed leading to a model having a strong bias
toward failure.

## Final plot

``` r
# Generate a sequence of temperatures from 30°F to 90°F
tempv <- seq(from = 30, to = 90, by = 0.5)

# Use the mean pressure from the data for prediction
mean_pressure <- mean(data$Pressure, na.rm = TRUE)

# Create a new data frame for predictions with the sequence of temperatures and mean pressure
new_data <- data.frame(Temperature = tempv, Pressure = mean_pressure)

# Predict probabilities using logmodel, model_2, and model_3
predicted_prob_logmodel <- predict(logmodel, newdata = new_data, type = "response")
predicted_prob_model_2 <- predict(model_2, newdata = new_data, type = "response")
predicted_prob_model_3 <- predict(model_3, newdata = new_data, type = "response")

predicted_prob_logmodelrm <- predict(logmodelrm, newdata = new_data, type = "response")
predicted_prob_model_2rm <- predict(model_2rm, newdata = new_data, type = "response")
predicted_prob_model_3rm <- predict(model_3rm, newdata = new_data, type = "response")
```

    ## Warning in predict.lm(object, newdata, se.fit, scale = 1, type = if (type == : prediction from rank-deficient fit;
    ## attr(*, "non-estim") has doubtful cases

``` r
# Create a new data frame with predicted probabilities for all models
predicted_data <- data.frame(
  Temperature = rep(tempv, 3),
  PredictedProbability = c(predicted_prob_logmodel, predicted_prob_model_2, predicted_prob_model_3),
  Model = rep(c("logmodel", "model_2", "model_3"), each = length(tempv))
)

predicted_datarm <- data.frame(
  Temperature = rep(tempv, 3),
  PredictedProbability = c(predicted_prob_logmodelrm, predicted_prob_model_2rm, predicted_prob_model_3rm),
  Model = rep(c("logmodel", "model_2", "model_3"), each = length(tempv))
)

# Create the plot
ggplot() +
  # Predicted probabilities as lines for each model
  geom_line(data = predicted_data, aes(x = Temperature, y = PredictedProbability, color = Model), size = 1) +
  # Observed probabilities (Malfunction / Count) as red points
  geom_point(data = data, aes(x = Temperature, y = Malfunction / Count), color = "red", shape = 16) +
  labs(title = "Predicted vs Observed Probability of O-ring Malfunction",
       x = "Temperature (°F)", y = "Probability of Malfunction") +
  theme_minimal() +
  scale_y_continuous(labels = scales::percent) +
  scale_color_manual(values = c("blue", "green", "purple")) +
  theme(legend.position = "top")
```

![](analysis_challenger_files/figure-gfm/confidence%20intervals-1.png)<!-- -->

``` r
# Create the plot
ggplot() +
  # Predicted probabilities as lines for each model
  geom_line(data = predicted_datarm, aes(x = Temperature, y = PredictedProbability, color = Model), size = 1) +
  # Observed probabilities (Malfunction / Count) as red points
  geom_point(data = data, aes(x = Temperature, y = Malfunction / Count), color = "red", shape = 16) +
  labs(title = "Predicted vs Observed Probability of O-ring Malfunction",
       x = "Temperature (°F)", y = "Probability of Malfunction") +
  theme_minimal() +
  scale_y_continuous(labels = scales::percent) +
  scale_color_manual(values = c("blue", "green", "purple")) +
  theme(legend.position = "top")
```

![](analysis_challenger_files/figure-gfm/confidence%20intervals-2.png)<!-- -->

logmodel = Temperature alone model_2 = Temperature + Pressure model_3 =
Temperature \* pressure
