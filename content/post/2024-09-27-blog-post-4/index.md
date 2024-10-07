---
title: Blog Post 4
author: Package Build
date: '2024-10-01'
slug: blog-post-4
categories: []
tags: []
---


```
## Loading required package: carData
```

```
## Loading required package: ggplot2
```

```
## Loading required package: lattice
```

```
## 
## Attaching package: 'CVXR'
```

```
## The following object is masked from 'package:stats':
## 
##     power
```

```
## Loading required package: Matrix
```

```
## Loaded glmnet 4.1-8
```

```
## Linking to GEOS 3.11.0, GDAL 3.5.3, PROJ 9.1.0; sf_use_s2() is TRUE
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
## ✔ purrr     1.0.2     ✔ tidyr     1.3.1
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ tidyr::expand()     masks Matrix::expand()
## ✖ dplyr::filter()     masks stats::filter()
## ✖ dplyr::group_rows() masks kableExtra::group_rows()
## ✖ dplyr::id()         masks CVXR::id()
## ✖ purrr::is_vector()  masks CVXR::is_vector()
## ✖ dplyr::lag()        masks stats::lag()
## ✖ purrr::lift()       masks caret::lift()
## ✖ purrr::map()        masks maps::map()
## ✖ tidyr::pack()       masks Matrix::pack()
## ✖ dplyr::recode()     masks car::recode()
## ✖ purrr::some()       masks car::some()
## ✖ tidyr::unpack()     masks Matrix::unpack()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
## Loading required package: viridisLite
## 
## 
## Attaching package: 'viridis'
## 
## 
## The following object is masked from 'package:maps':
## 
##     unemp
```



```
## Rows: 38 Columns: 9
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): party, candidate
## dbl (3): year, pv, pv2p
## lgl (4): winner, incumbent, incumbent_party, prev_admin
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 959 Columns: 14
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): state
## dbl (13): year, D_pv, R_pv, D_pv2p, R_pv2p, D_pv_lag1, R_pv_lag1, D_pv2p_lag...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 387 Columns: 14
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (14): year, quarter, GDP, GDP_growth_quarterly, RDPI, RDPI_growth_quarte...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 7378 Columns: 9
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): state, party, candidate
## dbl  (4): year, weeks_left, days_left, poll_support
## lgl  (1): before_convention
## date (1): poll_date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 204564 Columns: 9
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): state, party, candidate
## dbl  (4): year, weeks_left, days_left, poll_support
## lgl  (1): before_convention
## date (1): poll_date
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## `summarise()` has grouped output by 'year', 'party'. You can override using the `.groups` argument.
```

```
##   reelect_president  N Percent
## 1             FALSE 12   66.67
## 2              TRUE  6   33.33
```

```
## Elections with At Least One Incumbent Running: 11
## Incumbent Victories: 7
## Percentage: 63.64
```

```
## # A tibble: 3 × 7
##    year candidate_DEM    candidate_REP    incumbent_DEM incumbent_REP winner_DEM
##   <dbl> <chr>            <chr>            <lgl>         <lgl>         <lgl>     
## 1  2004 Kerry, John      Bush, George W.  FALSE         TRUE          FALSE     
## 2  2012 Obama, Barack H. Romney, Mitt     TRUE          FALSE         TRUE      
## 3  2020 Biden, Joseph R. Trump, Donald J. FALSE         TRUE          TRUE      
## # ℹ 1 more variable: winner_REP <lgl>
```

```
##   reelect_party  N Percent
## 1         FALSE 10   55.56
## 2          TRUE  8   44.44
```

```
## prev_admin
## FALSE  TRUE 
## 72.22 27.78
```


```
## Rows: 1251 Columns: 7
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (3): state_abb, state_year_type, state_year_type2
## dbl (4): year, elxn_year, grant_mil, state_incvote_avglast3
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

<img src="{{< blogdown/postref >}}index_files/figure-html/pork-analysis-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/pork-analysis-2.png" width="672" />

```
## Rows: 18465 Columns: 16
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): state, county, state_abb
## dbl (13): year, state_FIPS, county_FIPS, dvoteswing_inc, dpct_grants, dpc_in...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

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
## lm(formula = dvoteswing_inc ~ dpct_grants * comp_state + as.factor(year) + 
##     dpc_income + inc_ad_diff + inc_campaign_diff + dhousevote_inc + 
##     iraq_cas2004 + iraq_cas2008 + dpct_popl, data = d_pork_county)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -27.321  -2.848  -0.025   2.728  22.994 
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)            -6.523210   0.084963 -76.777  < 2e-16 ***
## dpct_grants             0.003954   0.001043   3.792 0.000150 ***
## comp_state              0.155418   0.077223   2.013 0.044173 *  
## as.factor(year)1992    -0.156389   0.120591  -1.297 0.194699    
## as.factor(year)1996     6.230500   0.119533  52.124  < 2e-16 ***
## as.factor(year)2000    -2.000293   0.118588 -16.868  < 2e-16 ***
## as.factor(year)2004     8.248378   0.119371  69.099  < 2e-16 ***
## as.factor(year)2008     3.574248   0.124060  28.811  < 2e-16 ***
## dpc_income              0.134285   0.022326   6.015 1.84e-09 ***
## inc_ad_diff             0.061345   0.010851   5.654 1.60e-08 ***
## inc_campaign_diff       0.161845   0.013166  12.292  < 2e-16 ***
## dhousevote_inc          0.012093   0.001952   6.196 5.91e-10 ***
## iraq_cas2004           -0.153092   0.069585  -2.200 0.027816 *  
## iraq_cas2008           -0.164783   0.021677  -7.602 3.07e-14 ***
## dpct_popl               2.103344   0.530292   3.966 7.33e-05 ***
## dpct_grants:comp_state  0.006411   0.001781   3.600 0.000319 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.452 on 17943 degrees of freedom
##   (506 observations deleted due to missingness)
## Multiple R-squared:  0.4199,	Adjusted R-squared:  0.4194 
## F-statistic: 865.9 on 15 and 17943 DF,  p-value: < 2.2e-16
```

```
## Warning in left_join(inner_join(mutate(d_state_vote, state_abb = state.abb[match(d_state_vote$state, : Detected an unexpected many-to-many relationship between `x` and `y`.
## ℹ Row 1 of `x` matches multiple rows in `y`.
## ℹ Row 19 of `y` matches multiple rows in `x`.
## ℹ If a many-to-many relationship is expected, set `relationship =
##   "many-to-many"` to silence this warning.
```

```
## 
## Call:
## lm(formula = change_inc_pv2p ~ is_comp * change_grant_mil + as.factor(year), 
##     data = d_pork_state_model)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -136.740   -6.628    0.341    7.176   64.748 
## 
## Coefficients:
##                          Estimate Std. Error t value Pr(>|t|)    
## (Intercept)                9.6346     3.6317   2.653  0.00842 ** 
## is_comp                   -0.4004     4.1498  -0.096  0.92319    
## change_grant_mil           0.1138     0.1051   1.082  0.28001    
## as.factor(year)1992        6.8952     6.7168   1.027  0.30548    
## as.factor(year)1996      -21.3789     5.2732  -4.054 6.46e-05 ***
## as.factor(year)2000        3.5773     5.6260   0.636  0.52537    
## as.factor(year)2004      -30.1619     5.4753  -5.509 7.96e-08 ***
## as.factor(year)2008        1.0850     4.8627   0.223  0.82360    
## is_comp:change_grant_mil  -0.1027     0.1643  -0.625  0.53246    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 23.43 on 291 degrees of freedom
##   (50 observations deleted due to missingness)
## Multiple R-squared:  0.2675,	Adjusted R-squared:  0.2474 
## F-statistic: 13.29 on 8 and 291 DF,  p-value: 2.299e-16
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


```
## New names:
## Rows: 474 Columns: 26
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (15): Office, Abbreviation, Rating, PluralityParty, RepCandidate, RepSta... dbl
## (6): ...1, Cycle, RepVotesMajorPercent, DemVotesMajorPercent, ThirdVote... num
## (3): RepVotes, DemVotes, PluralityVotes lgl (2): Raw, Incumbent.Party
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 306 Columns: 3
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (1): state dbl (2): year, rating
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```

```
## 
##  0  1 
##  9 42
```

```
## [1] "Florida"        "Georgia"        "Iowa"           "Minnesota"     
## [5] "New Hampshire"  "New Mexico"     "North Carolina" "Ohio"          
## [9] "Texas"
```

```
## # A tibble: 9 × 4
##   state          cook_rating sabato_rating rating_match
##   <chr>                <dbl>         <dbl>        <dbl>
## 1 Florida                  4             5            0
## 2 Georgia                  4             3            0
## 3 Iowa                     4             5            0
## 4 Minnesota                3             2            0
## 5 New Hampshire            3             2            0
## 6 New Mexico               1             2            0
## 7 North Carolina           4             3            0
## 8 Ohio                     4             5            0
## 9 Texas                    4             5            0
```

```
##   cook_correct sabato_correct 
##      0.8823529      0.9803922
```

```
## [1] "Florida"        "Georgia"        "Iowa"           "North Carolina"
## [5] "Ohio"           "Texas"
```

```
## [1] "North Carolina"
```

```
## # A tibble: 7 × 1
##   state         
##   <chr>         
## 1 Arizona       
## 2 Georgia       
## 3 Michigan      
## 4 Nevada        
## 5 North Carolina
## 6 Pennsylvania  
## 7 Wisconsin
```

```
## # A tibble: 5 × 2
##    year mean_match_rate
##   <dbl>           <dbl>
## 1  2004           0.647
## 2  2008           0.608
## 3  2012           0.765
## 4  2016           0.804
## 5  2020           0.824
```

```
##   cook_correct sabato_correct 
##      0.8549020      0.9137255
```



Table: <span id="tab:expert-predictions"></span>Table 1: Expert Predictions Summary by Year

| Year| Mean.Cook.Correct| Mean.Sabato.Correct|
|----:|-----------------:|-------------------:|
| 2004|         0.8235294|           0.9607843|
| 2008|         0.8039216|           0.7843137|
| 2012|         0.8627451|           0.9607843|
| 2016|         0.9019608|           0.8823529|
| 2020|         0.8823529|           0.9803922|


```
## [1]  7 36
```

```
## Warning: Option grouped=FALSE enforced in cv.glmnet, since < 3 observations per
## fold
```

```
## Warning in cbind2(1, newx): number of rows of result is not a multiple of
## vector length (arg 1)
```

```
##      s1
```

```
## Rows: 387 Columns: 14
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (14): year, quarter, GDP, GDP_growth_quarterly, RDPI, RDPI_growth_quarte...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```
## Warning: Option grouped=FALSE enforced in cv.glmnet, since < 3 observations per
## fold
## Warning: number of rows of result is not a multiple of vector length (arg 1)
```

```
##      s1
```

```
## Warning: Option grouped=FALSE enforced in cv.glmnet, since < 3 observations per
## fold
## Warning: number of rows of result is not a multiple of vector length (arg 1)
```

```
##      s1
```

```
##      s1
```

```
## [1] 0.8556624
```

```
## [1] 0.1443376
```

```
##      s1
```

```
## [1] 0.1443376
```

```
## [1] 0.8556624
```

```
##      s1
```

```
##             state   pred winner
## 1       Wisconsin 52.277      D
## 2        Virginia 54.681      D
## 3           Texas 49.352      R
## 4    Pennsylvania 50.596      D
## 5            Ohio 46.866      R
## 6  North Carolina 49.832      R
## 7        New York  61.19      D
## 8   New Hampshire 53.933      D
## 9          Nevada 50.815      D
## 10      Minnesota 51.821      D
## 11       Michigan 51.114      D
## 12        Georgia 50.206      D
## 13        Florida 50.004      D
## 14     California 61.701      D
## 15        Arizona 50.292      D
```



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




