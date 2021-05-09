# Innovation at Colleges 

## Background
Innovation is the act of bringing new ideas, methods, or things. Why innovation is critical? It is important because technological advances have played a key role in facilitating radically improved standards of living. In particular, scientific-technological innovations play an increasingly prominent role in the growth of leading industrial economies. I'm inspired by the college environment to be involved in entrepreneurship and commercialization, and I'm curious to learn about how different colleges have different innovation cultures. 

## Business Question 
Therefore, this leads to my business question of: 
__The Importance of Exposure to Innovation: What impact the culture of innovation in college?__

This question is meaningful because it can help educators and the government to figure out factors that foster innovation, thereby providing a better environment to generate more innovation. 

## Data Question 
__How did parents’ income, students’ income after graduation, college ranking (elite, selective, nonseletive) influence the college’s rate of inventors?__

## List of Data Source 
To answer the above question, I utlize the Opportunity Insights website (https://opportunityinsights.org/data/). There are two major dataset that I use: 
1. [mrc_table2.csv: The Baseline Cross-Sectional Estimates of Child and Parent Income Distributions by College](https://github.com/sophiaxuu/decision-analytics/blob/main/mini-proj2/mrc_table2.csv)
2. [table3.csv: Innovation Rates by College](https://github.com/sophiaxuu/decision-analytics/blob/main/mini-proj2/table3.csv)

The table mrc_table2.csv (The Baseline Cross-Sectional Estimates of Child and Parent Income Distributions by College) reports our baseline estimates of parents’ and children’s income distributions by college. The values are calcualted at college-level as means over students in the 1980, 1981 and 1982 birth cohorts. When we first look at our data, we see that we can see the following information: 
* super_opeid: Institution OPEID / Cluster ID when combining multiple OPEIDs
* name: Name of college (or college group)
* tier: Selectivity and type combination (see Table 6 for more detailed descriptions of these groups)
* tier_name: Name of college tier
* par_mean: Mean parental income
* k_mean: Mean kid earnings

The tatble table3.csv (Innovation Rates by College) reports resents estimates of students’ patent rates by the college they attended. The table define the college each child attends as the institution the child attended for the greatest amount of time during the four calendar years in which the child turned 19-22. 

According to Opportunity Insights, they define an individual as an investor if he or she is listed on a patent application between 2001 and 2012 or grant between 1996 and 2014 (see Section II.B of the paper), and as a highly-cited inventor if he or she is among the 5% of inventors with the most patent citations by 2014 within his or her birth cohort. When we first look at the data, we see the followoing information: 
* super_opeid: Institution OPEID / Cluster ID when combining multiple OPEIDs
* count: Number of students
* inventor: Share of inventors among students

# Simple Linear Regression

![Figure1. Relationship between parent's income and share of inventors among students](https://github.com/sophiaxuu/decision-analytics/blob/main/mini-proj2/figure%201.png)

The above matrices lead to the hypothesis relationship: 
_The predicted share of investors = 0. 04118114 - 0.0631198 * students’ average parent income_ 

* R-squared is a measure of how well a linear regression model “fits” a dataset. It is the proportion of the variance in the response variable that can be explained by the predictor variable. A value of 0 indicates that the response variable cannot be explained by the predictor variable at all. A value of 1 indicates that the response variable can be perfectly explained without error by the predictor variable. In our table, the 0.18 value is not as good as I’ve expected, therefore there may exists a lot of outliners. 
* The residual standard deviation (or residual standard error) is also a measure used to assess how well a linear regression model fits the data. 

## Multi-variable Linear Regression 
Next, we will go beyond investigating only one independent variable, but many - how will variables including "Is_elite", "Is_selective", "nonselective", "par_mean", "k_mean", and "is_outliner" has influence on share of "investor"?

![figure 5. The Output Summary of Milt-Variable Linear Regression between share of inventors and school selectivity and parents' income](https://github.com/sophiaxuu/decision-analytics/blob/main/mini-proj2/figure%205.png)

![figure 6. Linear Regression between if the school is elite and share of inventors and parent's income](https://github.com/sophiaxuu/decision-analytics/blob/main/mini-proj2/figure%206.png)

Here, R2 value means R-squared is a measure of how well a linear regression model “fits” a dataset. Also commonly called the coefficient of determination. Our r-squares is 0.33, which is not as large as i want it to be. There may be further investigation needed to understand. 

The standard error gives in particular is an indication of the likely accuracy of the sample mean as compared with the population mean. The smaller the standard error, the less the spread and the more likely it is that any sample mean is close to the population mean. A small standard error is thus a Good Thing. 

The positive coefficient between is_elite and share_of_inventor shows that elite school does tend to have more students that files patents. 
The negative coefficient between par_mean and share_of_inventor shows that students with wealthy parents are less likely to be an inventor as students. 
__Share_of_inventor = 0.0371218 + 0.013103192 * is_elite -0.062772509 * par_mean__
 
In null hypothesis significance testing, the p-value is the probability of obtaining test results at least as extreme as the results actually observed, under the assumption that the null hypothesis is correct. In our study, the p-value of both is_elite and par_income is very small, which is good indication of prediction. 

For f-value, if you get a large f value (one that is bigger than the F critical value found in a table), it means something is significant. The F statistic compares the joint effect of all the variables together. Here the F = 103 is much large than the critical f value = 2, showing the significance of the two variables joint together. 

To justify my updated linear regression is better, I look at the F value and found that the second F value in “updated_prediction” F= 103 is larger than the F value = 50 in “aggregated_data”, meaning second regression is more accurate and significant. 


## Calculate Prediction and Identify Outliners 
Next, I want to investigate if there is any outliners in the data. To do so, I need to calculating prediction, then residul, then use Standard Error of Residual to find Outliers. 

1. To calculate the “predicted_share_of_inventor”, i use the above prediction equation Share_of_inventor = 0.0371218 + 0.013103192 * is_elite -0.062772509 * par_mean  
= $L$19 + $L$20 * G2 + $L$21 * H2 where L19 is intercept, L20 is coefficient of is_elite, and L21 is coefficient of par_mean 
The next step is to calculate the residual which is invetor_error = actual share of inventor - predicted share of inventor 
 
2. Next, we need to calculate the standard error of residual using =STEXY(known ys, known xs) = 0.012890078 according to the regression statistics 

3. Finally, Determine which of our data points are considered outliers based on our error and standard error of the residual calculations. We know that a data point is considered an outlier if the absolute value of the error is greater than 2 x the standard error of regression, so we can show if each value is an outlier with the Excel IF statement:
=IF(ABS(J2)>2*$N$24, “outlier”, “not outlier”)

The purpose of finding outliers is that, by analyzing our outlier data points, I can potentially identify causes of an outlier to improve current policy or methods to incentivize innovation. 

## Further Analysis and Application
We can conclude that we can predict the share of inventory in colleges based on if it's elite school and parent's average income level. 

__Share_of_inventor = 0.0371218 + 0.013103192 * is_elite -0.062772509 * par_mean__

The positive coefficient between is_elite and share_of_inventor shows that elite school does tend to have more students that files patents. This could be explained by the fact that elite schools tend to have more powerful research centers, faculties, and funding to conduct scientific research. Hence there is a positive correlation. 
The negative coefficient between par_mean and share_of_inventor shows that students with wealthy parents are less likely to be an inventor as students. This could be explained as wealthy people don’t need patents or innovation to increase social status, while people in lower-income levels are more motivated to innovate and improve their capital accumulation. 

With that correlation in mind, educational institutes that want to foster innovation could try to find students with middle parents' income, and provide better education as "elite school". The government will be able to better provide policies that encourage more innovation. 


Some possible areas to improve on will be: 
- I could potentially use another dataset that gives numeric ranking as a variable to measure school selectivity instead of grouping them 
- I could investigate and try to understand the causes of those outliers 
- I could look into what factors of elite school education is most closely related to fostering inventors 


# Data Analysis Steps 
1. I first Combine two datasets toghether using VLOOKUP with key = super_opeid as it appears in both table and is uniquely identifiable for each school. I use VLOOKUP four times to bring in different vairables from mrc_table2.xlsx to innovation_analysis.xlsx 
2. Next I do data cleaning by grouping group “highly selective private”, “highly selective public”, “ivy plus”, “other elite schools” as one group named “is_elite” using the function and grouping “selective private” and “selective private” and “is_selective” using the function. 
3. By filter, we find there is one N/A data, so I delete the row of Ohios State University and put it in “deleted” table. 
4. Next I perform simple linear regression with one independent variable - students' parent income "par_mean", and the dependent variable is the share of inventors among students - "inventor". I use SCATTERPLOT, and add linear regression line as followed.
5. Perform milti-vairable linear regression using the Excel Data Analysis ToolPack.
6. Calculate Prediction and Identify Outliners
