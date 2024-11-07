# Analysis of JMV's graph

The images we're provided from (JMV Slides)[https://github.com/alegrand/SMPE/blob/master/sessions/2022_10_Grenoble/02_Intro-Visu.pdf] (p29-46)

*disclaimer: i'm going to play the idiot on the room and assume that i have no technical or expertise knowledge about the domain of these graphics*

*impr = improvements*

## Total brand sales in Europe

![graph of Car sales of audi, BMW and Mercedes-Benz](https://github.com/keserz/SMPE24/blob/main/homework_2_JMV/img/car_brand_sales.png)

**Checklist**
- [x] Data

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
