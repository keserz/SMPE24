# Analysis of JMV's graph

The images we're provided from (JMV Slides)[https://github.com/alegrand/SMPE/blob/master/sessions/2022_10_Grenoble/02_Intro-Visu.pdf] (p29-46)

*disclaimer: i'm going to play the idiot on the room and assume that i have no technical or expertise knowledge about the domain of these graphics*

*impr = improvements*

## Total brand sales in Europe

![graph of Car sales of audi, BMW and Mercedes-Benz](https://github.com/keserz/SMPE24/blob/main/homework_2_JMV/img/car_brand_sales.png)

**Checklist**
- [ ] Graphical objects
    - [ ] Graphical objects are readable on screen, on printed version (B/W), on video
        - issue: the blue and red line may have the same gray level when printed in black and white
        - impr: 3 types of lines could have been used: a continuous line, a dashed line and a doted line
    - [ ] Graphical axis are well identified and labelled
        - issue: we can guess that the X axis represent years and the Y axis represent number of sales (based on the title) but it's not clear since it's not defined
        - impr: define the axis

- [ ] Annotations
    - [ ] Axis are labbeled by quantites
    - [ ] Labels of the axis are clear, and self contained
    - [ ] Units are indicated on the axis
    - [ ] Origin is (0, 0), if not it should be clearly justified
    - There is no label on the axis and we have to guess to understand the units, however we don't really know what was being sold, is it only cars, accessories, components or every sales each brand made ? That being said i think we can give them a pass for the origin not starting a (0, 0) most people should understand that these brands existed before 1997 but also not in years 0 (X-axis) and that the number of sales is the continuity from previous year hence not starting at 0 on the Y-axis

- [ ] Information
    - [ ] The graphic gives a relevant information to the reader
        - issue: Simply knowing that one brand sold more than another doesn’t give us meaningful insight. As mentioned, we lack critical details about what was sold, the revenue generated, or the remaining inventory. For instance, in 2013, Audi might have sold 749,999 small parts and just one engine, BMW could have sold around 640,000 cars but still hold a million in stock, while Mercedes might have sold out entirely. These scenarios would significantly alter the story told by the graph.
        - impr: Specify what was sold, and consider using a ratio on the Y-axis (such as sales-to-production)

**Everything else seems to checkout**

## Monthly number of objects in earth orbit by object type
![](https://github.com/keserz/SMPE24/blob/main/homework_2_JMV/img/object_orbit.png)

**Checklist**
- [ ] Graphical objects
    - [ ] Graphical objects are readable on screen, on printed version (B/W), on video
        - issue : graphical objects are not readable on a printed B/W version
        - impr : if we stick to their visualization, i'm not sure this issue can be solved since there is many curve and adding some motifs could make it confusing
    - [ ] Grids help the reader
        - i would argue that the grid actually disturbs the user, reducing the grid opacity would make the reading easier for the user in my opinion
    - [ ] Curves cross without ambiguity
        - some curves do not cross without ambiguity, for example i can't figure out if spacecraft objects in earth orbit started in 1963 or 1957, same type of issue with mission-related debris.
- [ ] Annotations
    - [ ] Origin is (0, 0), if not it should be clearly justified
        - Although the x axis (years) doesn't start at 0, the authors should explain somewhere why they start at 1956, maybe something like "analysis of object in earth orbit started in 1956"
- [ ] Information
    - [ ] Curves are on the same scale
        - The total object curve is thicker than all the other curves

**My 2 cents**
The graph is a little confusing during the 1956 - 1966 period because there is a lot of curves on top of each others, maybe a "stack" representation could leverage this issue (drawing example below). I think that we also lack some additional information like an explanation of why was there a sudden spike in fragmentation debris in the year 2007. Other spike could also be explained with a little tag (example below).


## Demandeurs d'emplois
![](https://github.com/keserz/SMPE24/blob/main/homework_2_JMV/img/pole_emploi.png)

**Checklist**
- [ ] Graphical objects
    - [ ] Graphical objects are readable on screen, on printed version (B/W), on video
        - issue : graphical objects are not readable on a printed B/W version
        - impr : different dashed length line could solve this issue
- [ ] Annotation
    - [ ] Origin is (0, 0), if not it should be clearly justified
        - X-axis
            - issue : The title of the graph says from 1996 to 2017, however the origin of the graph seems to start in 2011, we can see that there is a "zooming interaction effect" bellow the x axis but there isn't any information as to why it starts in 2011.
        - Y-axis
            - issue : the axis doesn't start from 0.
        - in this graph the information portrayed seems to show that there is a large gap between category ABC and A however if we scale this graph between 0 and 6 millions, we would see very little difference between these two lines
    - [ ] Each curve has a legend
        - issue : Each curve has indeed a legend but we have no information as to what they mean, what is category A and category ABC, is category A engulfed in category ABC ? are they completely distinct categories ? why would we want to compare them ? so many questions ...
- [ ] Context
    - issue : as mentioned previously we don't know anything as to what the categories mean, additionally the title says that these are the people who register at pole emploi at the end of the month, why would we want to only portray citizen looking for a job only at the end of the month ? Do they mean that these are the number of people still registered by the end of the month or that registered at the end of the month ? also, when is the "end of the month" starts ? is it the 20th, the 25th, the 29th ? did they pick the 25th for january then the 20th for february and so on or is it the same day for each month ?