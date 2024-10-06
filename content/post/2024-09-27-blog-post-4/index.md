---
title: Blog Post 4
author: Package Build
date: '2024-10-01'
slug: blog-post-4
categories: []
tags: []
---

<img src="{{< blogdown/postref >}}index_files/figure-html/pork-analysis-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/pork-analysis-2.png" width="672" />


```
## 
## Call:
## lm(formula = dvoteswing_inc ~ dpct_grants * comp_state + as.factor(year), 
##     data = d_pork_county)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -27.7179  -2.8547  -0.0047   2.7889  23.2187 
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            -6.450079   0.084452 -76.376  < 2e-16 ***
## dpct_grants             0.004762   0.001036   4.595 4.35e-06 ***
## comp_state              0.152687   0.076143   2.005 0.044949 *  
## as.factor(year)1992     0.170688   0.115787   1.474 0.140458    
## as.factor(year)1996     6.345396   0.115509  54.934  < 2e-16 ***
## as.factor(year)2000    -2.049544   0.116215 -17.636  < 2e-16 ***
## as.factor(year)2004     8.407388   0.115576  72.743  < 2e-16 ***
## as.factor(year)2008     3.136792   0.116122  27.013  < 2e-16 ***
## dpct_grants:comp_state  0.006391   0.001764   3.623 0.000292 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.495 on 18455 degrees of freedom
##   (1 observation deleted due to missingness)
## Multiple R-squared:  0.4027,	Adjusted R-squared:  0.4025 
## F-statistic:  1556 on 8 and 18455 DF,  p-value: < 2.2e-16
```



```
## 
## Call:
## lm(formula = pv2p ~ GDP_growth_quarterly + incumbent + juneapp, 
##     data = subset(d_tfc_train, year < 2020))
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -4.411 -1.869 -1.422  2.360  5.396 
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           39.5646     7.4994   5.276  0.00051 ***
## GDP_growth_quarterly   0.7835     0.2454   3.193  0.01096 *  
## incumbent              3.6646     2.1840   1.678  0.12768    
## juneapp                0.1581     0.1655   0.956  0.36416    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3.675 on 9 degrees of freedom
##   (5 observations deleted due to missingness)
## Multiple R-squared:  0.5824,	Adjusted R-squared:  0.4432 
## F-statistic: 4.183 on 3 and 9 DF,  p-value: 0.04122
```

```
## 
## Call:
## lm(formula = pv2p ~ juneapp, data = subset(d_tfc_train, year < 
##     2024))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -5.8720 -3.4311 -0.5621  1.6282 10.9476 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 48.17874    8.90177   5.412 0.000157 ***
## juneapp      0.07262    0.20668   0.351 0.731394    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.012 on 12 degrees of freedom
##   (5 observations deleted due to missingness)
## Multiple R-squared:  0.01018,	Adjusted R-squared:  -0.0723 
## F-statistic: 0.1235 on 1 and 12 DF,  p-value: 0.7314
```



Table: <span id="tab:expert-predictions"></span>Table 1: Expert Predictions Summary by Year

| Year| Mean.Cook.Correct| Mean.Sabato.Correct|
|----:|-----------------:|-------------------:|
| 2004|         0.8235294|           0.9607843|
| 2008|         0.8039216|           0.7843137|
| 2012|         0.8627451|           0.9607843|
| 2016|         0.9019608|           0.8823529|
| 2020|         0.8823529|           0.9803922|


Table: <span id="tab:ensembling"></span>Table 2: State-Level Predictions

|State          |Prediction |Winner |
|:--------------|:----------|:------|
|Wisconsin      |52.277     |D      |
|Virginia       |54.681     |D      |
|Texas          |49.352     |R      |
|Pennsylvania   |50.596     |D      |
|Ohio           |46.866     |R      |
|North Carolina |49.832     |R      |
|New York       |61.19      |D      |
|New Hampshire  |53.933     |D      |
|Nevada         |50.815     |D      |
|Minnesota      |51.821     |D      |
|Michigan       |51.114     |D      |
|Georgia        |50.206     |D      |
|Florida        |50.004     |D      |
|California     |61.701     |D      |
|Arizona        |50.292     |D      |




