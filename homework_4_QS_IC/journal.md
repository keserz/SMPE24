# 95% Confidence Interval Analysis

This analysis calculates the **95% confidence intervals** using quantiles for the `Time` variable, grouped by `Type` and `Size`. The data is visualized with a **log10 scale** applied to the `Size` axis for better visualization of the wide range of values, from 100 to 1,000,000. The plot illustrates the mean `Time` for each group along with the confidence intervals.

## Methodology

- The **2.5th** and **97.5th** percentiles of the `Time` data are computed for each combination of `Type` and `Size` to define the **95% confidence intervals**.
- A **log10 scale** is used for the `Size` variable to compress the large range.

## Plot

Below is the plot representing the **95% confidence intervals**:

![95% Confidence Interval Plot](https://github.com/keserz/SMPE24/blob/main/homework_4_QS_IC/analysis/CI_plot.png)

## R Markdown

For detailed code and further analysis, refer to the corresponding [R Markdown file](https://github.com/keserz/SMPE24/blob/main/homework_4_QS_IC/analysis/quickSort_IC_analysis.md).
