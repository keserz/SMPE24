# Critique of the [initial document](https://github.com/keserz/SMPE24/blob/main/homework_5_challenger/exo5_en.pdf)

## Strengths

### Context
- The introduction contextualize the analysis of the Challenger disaster providing motivation for this analysis.
- The study focuses on temperature and pressure regarding the O-rings which was the key point raised by discussion between Morton Thiokol and NASA according to the introduction.

### Data
- This analysis uses real data with temperature, pressure and malfunction records allowing for an empirical analysis instead of simulated data

### Graphical analysis
- The study doesn't solely relies on the number and provide a graphical analysis to explore patterns in the data

### Framework
- Logistic regression seems like a suitable choice to determine binary outcome of the O-rings success or failure which provide interpretable probabilities towards the outcome


## Weakness

### Data
- in the data the lowest temperature record was 53°F which isn't representative of the 31°F at take-off
- The removal of zero malfunction data lacks justification and could lead to some bias by excluding reliable take-off

### Hypothesis of independence
- The study seems to assume that each O-rings success or failure is independent and doesn't seem to be concerned by a factor that could affect all of the O-rings which can lead to an underestimation of the risk.

### Statistical interpretation
- The study seems to assume that since the temperature shows no significant impact on the probability of failure of the O-rings then the temperature has no influence on it, which is an issue in itself because at best it only shows that this study failed to show an impact, not that it doesn't exist.


## Improvement
- an improved analysis can be found [here](https://github.com/keserz/SMPE24/blob/main/homework_5_challenger/analysis/analysis_challenger.md)


