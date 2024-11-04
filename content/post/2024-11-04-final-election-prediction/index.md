---
title: 'Final Election Prediction '
author: 'Kelly Olmos' 
date: '2024-11-04'
slug: 'final-election-prediction'
categories: []
tags: []
---

In this blog post, I will present my final election prediction and go over the methodology for this prediction.

### Methodology

I used a singular regression to make my final prediction. My linear regression has 14 predictors to model the democratic two party vote share. My model predicts the democratic two party vote share for each state independently if there is polling data for that state. The other states are not considered battleground states and therefore I can use the lagged vote shares to reliably predict which candidate will win that state based on past presidential elections. 

latest_pollav_DEM
D_pv2p_lag1
D_pv2p_lag2
Q2_gdp_growth
White 
Black 
Hispanic
American_indian
Asian_pacific_islander
Less_than_college
College
Age_18_to_29
Age_30_to_64
Age_65_75plus

I can divide my predictors into three main categories: fundamentals, polling data, and demographics. My fundamental predictor is the second quarter GDP growth for each state. In my week 7 blog post, I used the gdp of each state because I did not have consolidated data for GDP growth. Following a class discussion, it makes more sense to use GDP growth because that is a better indicator of economic performance than gdp which some states like California will always have a greater gdp than smaller states who may be performing relatively similar. Additionally, Alan Abramowitz’s time for change model uses GDP growth as one of the handful of indicators to forecast the presidential election outcome. 

The next important set of data used in this model is polling data, it takes the most up to date state level polls as of November 1st to predict which candidate will win each state. Polling data aggregates and measures how voters at the time feel about each candidate. By including state level polls, I can include how voters currently feel about the candidates and since it is so close to the election might capture how they will up voting. 

The rest of my predictors are age, race, and education. Although demographics are the best indicators of how someone will vote, they do provide valuable information regarding turnout. For example, young voters are known for turning out at disproportionately lower rates than older age groups. Additionally, if a voter has attained higher education they are more likely to turnout out to vote. For this reason, I find demographic data valuable and worth including in this regression model. 

I originally had more predictors in my regression model. I had more categories for age groups and level of educational attainment as well as the unemployment rate but ultimately had to simplify my model because the high number of predictors, especially those that are correlated, caused high multicollinearity. My model is already overfitting but prior to the removal of those other variables and condensing into fewer categories, the multicollinearity was higher. The unemployment rate and GDP growth turned out to be highly correlated so in the end I opted for just the GDP growth. There are a handful of other predictors that could be included in this model but I need to have a way to quantify these predictors and have state by state data for 2024. There are other events and predictors that are hard to include and quantify in models. For example, the Dobbs decision is predicted to increase women turnout but how can you quantify the effect of an event like this one? Additionally, the data for campaign spending on ads on a state level basis was hard to find but I think could’ve been very interesting to include. It’s also hard to quantify the impact of ads because not only do campaigns run ads but so do independent expenditures. However, at one point creating a really complex model with too many variables might not say much because you could run into the issue I had and still have, multicollinearity. 

Why did I use linear regression as opposed to ensemble models or machine learning? In the end I opted for the model that made the most sense to me considering my skill level and understanding of statistics. The linear regression model was appealing to me because it is simple and easy to understand. It is straightforward to see how each coefficient impacts that model. 

On that note, here is a table that shows the summary of the regression model. Only two variables were statistically significant with this regression model: latest_pollav_DEM and D_pv2p_lag2. I recognize that the Adjusted R-squared is 0.9661, which is very high but I have reduced to what I believe are the core predictors in terms of demographics, and polling data, and economic indicators. 

```
## 
## Call:
## lm(formula = D_pv2p ~ latest_pollav_DEM + D_pv2p_lag1 + D_pv2p_lag2 + 
##     q2_gdp_growth + white + black + american_indian + asian_pacific_islander + 
##     less_than_college + college + hispanic + age_18_to_29 + age_30_to_64 + 
##     age_65_75plus, data = d.train)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.3485 -0.5579 -0.0639  0.9712  2.3936 
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            -13.61715   27.71331  -0.491 0.626076    
## latest_pollav_DEM        0.51656    0.09886   5.225 7.02e-06 ***
## D_pv2p_lag1              0.01105    0.13413   0.082 0.934789    
## D_pv2p_lag2              0.38902    0.09383   4.146 0.000189 ***
## q2_gdp_growth            0.11066    0.06893   1.605 0.116921    
## white                    0.09577    0.18095   0.529 0.599764    
## black                    0.12891    0.17985   0.717 0.478033    
## american_indian          0.27691    0.25134   1.102 0.277700    
## asian_pacific_islander   0.33104    0.31081   1.065 0.293729    
## less_than_college        0.05484    0.56135   0.098 0.922700    
## college                  0.50194    0.58136   0.863 0.393482    
## hispanic                 0.15132    0.10638   1.422 0.163290    
## age_18_to_29             0.61459    0.75485   0.814 0.420746    
## age_30_to_64            -0.38090    0.86165  -0.442 0.661018    
## age_65_75plus           -0.33132    0.64347  -0.515 0.609690    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.495 on 37 degrees of freedom
## Multiple R-squared:  0.9754,	Adjusted R-squared:  0.9661 
## F-statistic: 104.8 on 14 and 37 DF,  p-value: < 2.2e-16
```


### Model Validation 

To deal with the potential overfitting and multicollinearity, I decided to run an elastic net model on my training and test datasets. I opted for the elastic net model because it combines lasso and ridge regression. Essentially, the elastic net model adds penalties to regularize the model. I chose the alpha and lambda values by testing a couple and going with the one that minimizes the cross-validated mean squared error. The results for the battleground states are displayed in the table below. All swing states except Arizona went for Trump. However, it is important to note how close some states are. For example, in Georgia, the predicted two party vote share is 49.66829 which is very close to the 50% threshold for Harris. 


|state          | predicted_D_pv2p|predicted_winner |
|:--------------|----------------:|:----------------|
|Arizona        |         50.20252|Democrat         |
|Georgia        |         49.66760|Republican       |
|Michigan       |         49.12480|Republican       |
|Nevada         |         49.45751|Republican       |
|North Carolina |         48.90056|Republican       |
|Pennsylvania   |         48.74455|Republican       |
|Wisconsin      |         48.70884|Republican       |



### Final Prediction 
To explore further how close the election is and obtain some confidence intervals I ran 10,000 simulations and created 95% confidence intervals for each state with state-level data available. The results are displayed below. I made a table for the battleground states and all seven states could go for Trump or Harris based on the 95% confidence interval which reflects how close this election will be. 



|party      | total_electors|
|:----------|--------------:|
|Democrat   |            253|
|Republican |            285|


<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />


|state          | mean_dem| mean_rep| lower_dem| upper_dem| lower_rep| upper_rep|winner     |
|:--------------|--------:|--------:|---------:|---------:|---------:|---------:|:----------|
|Arizona        | 50.24998| 49.75002|  47.36775|  53.13222|  46.86778|  52.63225|Democrat   |
|Georgia        | 50.04388| 49.95612|  47.08478|  53.00298|  46.99702|  52.91522|Democrat   |
|Michigan       | 48.95665| 51.04335|  46.00299|  51.91031|  48.08969|  53.99701|Republican |
|Nevada         | 49.74023| 50.25977|  46.78517|  52.69529|  47.30471|  53.21483|Republican |
|North Carolina | 49.29332| 50.70668|  46.38625|  52.20039|  47.79961|  53.61375|Republican |
|Pennsylvania   | 48.92587| 51.07413|  46.01251|  51.83923|  48.16077|  53.98749|Republican |
|Wisconsin      | 48.62988| 51.37012|  45.67864|  51.58111|  48.41889|  54.32136|Republican |

### Discussion

There are limitations to running a linear regression to model the election outcome, especially considering that my model may be overfitting. For future iterations I would consider exploring other types of models because a linear regression aims to find a linear relationship among the predictors but that might not be the true relationship between these variables. 




