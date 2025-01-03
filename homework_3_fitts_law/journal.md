# Laboratory Notebook for a user pointing experiment

## Project overview
The following introduction is copied from the [work of Céline Coutrix](https://gricad-gitlab.univ-grenoble-alpes.fr/coutrixc/m2r_pointingxp/-/tree/main?ref_type=heads) who explored the relationship between movement time, target distance and target width in the context of Fitt's law :

*Fitts described 1954 the relationship between the distance to a target, its width, and the time needed to acquire it [[Fitts, 1954](http://www2.psychology.uiowa.edu/faculty/mordkoff/InfoProc/pdfs/Fitts%201954.pdf)].*

*To acquire a target, e.g., to move the mouse cursor and click on a file to select it, Fitts' law describes how the distance between the start point and the target (_A_: amplitude of the movement), and the size of the target (_W_: width of the target) impacts the index of difficulty of the task (_ID_) [[MacKenzie and Buxton, 1992](http://www.billbuxton.com/fitts92.html)].*

*The time (_MT_: movement time) needed for a user to acquire a target is linearly correlated to _ID_:*

*> _ID_ = log2(_A_/_W_ + 1)*

*The time (_MT_: movement time) needed for a user to acquire a target is linearly correlated to _ID_:*

*> _MT_ = a + b × _ID_*

*A large part of Human-Computer Interaction research since then builds on top of Fitts' law.*

*This project aims at finding the values of the _a_ and _b_ parameters. This document contains my attempts to experimentally find _a_ and _b_ parameters.*

In my project, I aim to replicate the experiment conducted by Céline Coutrix, which experimentally determined the values of the parameters a and b. The following document outlines my approach and findings in replicating their work.

## General Organization
### data/
This folder contains 2 experiment performed by the same participant on the [pointing experiment from Ergonomics Web at Cornell University](http://ergo.human.cornell.edu/FittsLaw/FittsLaw.html). The date of the experiment was the 2nd january 2025.
#### First experiment
- `raw_data`, i.e. the raw data  as returned from the experimental software. 
- `mt_data`, i.e. the processed mean movement times as returned from the experimental software.
#### Second experiment
- `raw_data2`, i.e. the raw data  as returned from the experimental software. 
- `mt_data2`, i.e. the processed mean movement times as returned from the experimental software.
#### Coutrix experiment
- `20211117_1527_RawData`, i.e. the raw data  as returned from the experimental software. 
- `20211117_1527_MeanMT`, i.e. the processed mean movement times as returned from the experimental software. 

## Experimental reports

### 2025-01-02

#### Experimental task
The experiment was performed on the same [experimental software](http://ergo.human.cornell.edu/FittsLaw/FittsLaw.html) used by Céline Coutrix.
1. The first text field is used to enter the width of the target, separated with ','.
2. The second text field is used to enter the distances between the target, separated with ','.
3. The third text field is used to enter the number of trials, but the actual number of trials collected will be one less (n-1). This is because the first trial at a new distance cannot be used, as it follows a different distance from the previous one.

#### Controlled variables
- The reaction time of the participant was 263.2 ms, this was measured from an average of 10 trials on the [human benchmark reaction time](https://humanbenchmark.com/tests/reactiontime/).

- The participant had been using a computer mouse for more than 20 years

- The model of the computer mouse was a [Logitech 502X PLUS](https://www.logitechg.com/fr-fr/products/gaming-mice/g502-x-plus-wireless-lightforce.html?srsltid=AfmBOopUFiXgl7CPpopGOrWxwxIFE9uOmLBkcRoQCGT9pruOHUWPeZRi) set to 2400 DPI

- The monitor on which the experiment was displayed was an IPS BENQ GW2765 with a resolution of 2560x1440 pixels, 4ms response time and a refresh rate of 60hz

#### Independent variables
- Width of the target:
    - First experiment : 1, 2, 4 pixels
    - Second experiment : 16, 32, 64 pixels

- Distance between target:
    - First experiment : 16, 32, 64 pixels
    - Second experiment : 124, 256, 512 pixels

- Number of trials:
    - First experiment : 6 trials, for a total of 45 observations
    - Second experiment : 10 trials for a total of 81 observations

#### Data collected
The webpage gave the following results:
##### First experiment:
- 32 errors out of 45 observations where made
- A fitts modelling in the form of _MT_ = 892.715 + 29.187log(A/W + 1) with R2 = 0.044
- The [mean movement time table](./data/mt_data.csv) is provided in the [data folder](./data/)
    
##### Second experiment:
- 3 errors out of 81 observations where made
- A fitts modelling in the form of _MT_ = MT = 132.970 + 180.536log(A/W + 1) with R2 = 0.884
- The [mean movement time table](./data/mt_data2.csv) is provided in the [data folder](./data/)

#### Data analysis
This analysis is available in the [analysis folder](./analysis/) (markdown file).
- `pointingAnalysis1`,  [analysis of experiment 1](./analysis/pointingAnalysis_1_2_4_16_32_64.md)
- `pointingAnalysis2`,  [analysis of experiment 2](./analysis/pointingAnalysys_16_32_64_128_256_512.md)
- `coutrixPointingAnalysis`,  [analysis of Coutrix's experiment](./analysis/coutrixPointAnalysis.md)

