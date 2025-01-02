In the original journal several issue was found regarding the experimental explanation provided by the researcher [found on this link](https://gricad-gitlab.univ-grenoble-alpes.fr/coutrixc/m2r_pointingxp/-/blob/main/journal.md?ref_type=heads)

**Below is a list of several issues**:

- Experimental setup
    - tools used
        - no mention of the device specification or model used to track the target acquisition
        - no mention of the display specification or model and resolution used
    - no mention of the distance between eyes and display
    - no mention of the lighting setup

- Demographic
    - Only one participant, outliers exist in most if not all experiments
    - No mention about the characteristic of the participant
        - age, gender, expertise with the pointing device, vision impairment, mobility impairment, reaction time at the moment of data collection

- Experimental task
    - what is the shape and size of the target
    - what is the unit of the "*width*" ?
    - what is the unit of the "*distance*" ?
    - what were the instructions the participant had to follow ?

Since websites may not remain accessible indefinitely, we risk losing crucial information about the participant's task. Without this, we wouldn’t know details like the shape of the target, its size (and unit), or the distance (and unit) to be covered.

- Order effect
An order effect may have influenced the results of this experiment, which is another reason why a single participant is insufficient to draw any meaningful conclusions.

- Data
The data does not align with the description provided in the author’s journal. In the journal, the author states: "*I ran the experiment from the above webpage with widths of 1, 2, and 4, and distances of 16, 32, and 64*." However, the data instead shows distances of 8, 16, and 32.

##### Explanation of the difference found by Céline Coutrix regarding the R2 provided by the webpage and her results
In the pointingAnalysis file from Coutrix's experiment, she mentionned that there was a difference between the R2 found in her experiment(R2 = 0.008146) and the R2 provided by the webpage (R2=0.218). This is explained by the fact that the webpage uses a different formula to computer the 

The results provided by Coutrix where the following : 

```
Residuals:
   Min     1Q Median     3Q    Max 
-381.4 -276.3   58.7  144.7  539.6 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)  
(Intercept)   1108.4      351.1   3.158    0.016 *
ID             111.0      107.5   1.032    0.336  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 320.5 on 7 degrees of freedom
Multiple R-squared:  0.1321,	Adjusted R-squared:  0.008146 
F-statistic: 1.066 on 1 and 7 DF,  p-value: 0.3363
```

Coutrix reported the value of the Adjusted R squared of 0.008146 while the webpage reports the multiple R squared, but even in this case we still have a difference since the multiple r squared value here is 0.1321 which is different than 0.218.

This issue lies in the fact that the website provides the mean MT data with rounded values therefor, and ID of 1.5 suddenly becomes and ID of 2 which can significantly impact the result of the linear model. When we perform the analysis from the raw data of Coutrix, we found the same value provided by the website

```
Residuals:
    Min      1Q  Median      3Q     Max 
-367.35 -275.12   34.85  139.45  541.87 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)  
(Intercept)   1001.3      339.5   2.949   0.0214 *
newID          140.6      100.7   1.396   0.2053  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 304.2 on 7 degrees of freedom
Multiple R-squared:  0.2178,	Adjusted R-squared:  0.1061 
F-statistic: 1.949 on 1 and 7 DF,  p-value: 0.2053
```

And in this case, the data provided by the webpage is the same as the result computed :  _MT_ = 1001.3 + 140.6 x log(A/W+1) with R2 = 0.218

This emphazise the need to make the computation from the raw data instead of using pre processed data since we don't know what type of transformation the data went through before they were provided


