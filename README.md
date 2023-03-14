# MechaCar_Statistical_Analysis
## Overview of Project 
In this project, we are using R to perfrom statistical analysis.
## Results
## Linear Regression to Predict MPG
In order to predict the MPG from vehicle length, weight, spoiler angle, ground clearance and AWD, a multiple linear regression was performed after importing the data.
The steps are shown in the code below:

library(dplyr)

mecha <-read.csv('MechaCar_mpg.csv')

head(mecha)

lr_model <-lm(mpg ~ vehicle_length + vehicle_weight + spoiler_angle + ground_clearance + AWD, data = mecha)

summary(lr_model)

Here are the results of the model:

![fig1](/img/lm.png?raw=true "Linear Regression")

-	Which variables/coefficients provided a non-random amount of variance to the mpg values in the dataset?

The results indicate that beside the intercept, vehicle length and ground clearance have a non-random amount of variance to the mpg (top red box)

-	Is the slope of the linear model considered to be zero? 

According to the model, given the estimated coefficient of each variable (cyan box) and the Pr(>|t|) values, the slope is not zero. In addition the R-squared of 0.7149 (bottom red box) shows strong linear relationship

-	Does this linear model predict mpg of MechaCar prototypes effectively? 

The R-squared of 0.7149 (bottom red box) shows strong linear relationship
## Summary Statistics on Suspension Coils
To test if the manufacturing process is consistent across production lots, mean, median, variance and standard deviation for the entire population and for each manufacture lot were calculated. 

1.	Calculating mean, median, variance and standard deviation of total data set:

sus <-read.csv("Suspension_Coil.csv")

total_summary <- sus %>%summarize(Mean = mean(PSI),Median = median(PSI), Variance = sd(PSI)**2, SD = sd(PSI))

The results can be observed in the following figure:

![fig2](/img/sus_summary.png?raw=true "Stat. Summary")

2.	Calculating mean, median, variance and standard deviation for each lot:

lot_summary <- sus%>%group_by(Manufacturing_Lot)%>%summarize(Mean = mean(PSI),Median = median(PSI), Variance = sd(PSI)**2, SD = sd(PSI))

The results can be observed in the following figure:

![fig3](/img/lot_summary.png?raw=true "Lot Stat. Summary")

-	The design specifications for the MechaCar suspension coils dictate that the variance of the suspension coils must not exceed 100 pounds per square inch. Does the current manufacturing data meet this design specification for all manufacturing lots in total and each lot individually? 

According to the data, for lot1 (variance ~0.98 PSI^2) and 2 (variance ~7.5 PSI^2) variance is below 100 PSI^2 but lot 3 shows variance of 170 PSI^2 which violates the design specifications.

## T-Tests on Suspension Coils
To investigate if all manufacturing lots is statistically different from the population mean of 1500 PSI, the following t.test was performed:

t.test(sus$PSI,mu=1500)

The result (depicted below, red box) gives p-value of 0.06 which is larger than 0.05 meaning that we cannot reject the null hypothesis, i.e. the mean for all lots is not significantly different from 1500 PSI.

![fig4](/img/ttest_all.png?raw=true "T-test")

To investigate if all manufacturing lots are statistically different from the population mean of 1500 PSI, the following t.tests was performed:

t.test(subset(sus,Manufacturing_Lot == "Lot1")$PSI,mu=1500)

t.test(subset(sus,Manufacturing_Lot == "Lot2")$PSI,mu=1500)

t.test(subset(sus,Manufacturing_Lot == "Lot3")$PSI,mu=1500)

The result (depicted below, red box) shows that for Lot1 and Lo3, the p-values are 1 and 0.6 which are both larger than 0.05 so null hypothesis cannot be rejected meaning that their mean values is not significantly different from 1500 PSI. However, for Lot3 the p-value is 0.41 which is smaller than 0.05 and null hypothesis can be rejected indicating that the average is significantly different from 1500 PSI.

![fig5](/img/ttest_lot.png?raw=true "Lot T-test")


## Study Design: MechaCar vs Competition

To compare MechaCar with a hypothetical competator:

•	What metric or metrics are you going to test?

I can think of several metrics that can be compared among MechaCar and the competitors: cost, city and highway mpg, safety rate, fuel efficiency, battery life and exhaust pollution (CO2 emission which I will use in the following analysis).

•	What is the null hypothesis or alternative hypothesis?

For each comparison a null and an alternative hypothesis can be considered: for instance: 
Null hypothesis: cars produced by MechaCar and a competitor emit the same amount of CO2 by burning one gallon of premium gas. 
Alternative hypothesis: cars produced by MechaCar and a competitor emit different amount of CO2 by burning one gallon of premium gas.

•	What statistical test would you use to test the hypothesis? 

To test the null hypothesis, I will perform a 2 sample Ttest using the data for CO2 emission of MechaCar data and the competitor.

•	What data is needed to run the statistical test?

To do the test, I would need the CO2 emission of MechaCar data and the competitor
