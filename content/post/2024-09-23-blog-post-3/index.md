---
title: Blog Post 3
author: Package Build
date: '2024-09-23'
slug: blog-post-3
categories: []
tags: []
---

```{r setup chunk, echo=FALSE}
## Loading required package: carData
```

```
## Loading required package: ggplot2
```

```
## Loading required package: lattice
```

```
## Warning: package 'CVXR' was built under R version 4.3.3
```

```
## Warning in .recacheSubclasses(def@className, def, env): undefined subclass
## "pcorMatrix" of class "ConstVal"; definition not updated
```

```
## Warning in .recacheSubclasses(def@className, def, env): undefined subclass
## "pcorMatrix" of class "ConstValListORExpr"; definition not updated
```

```
## Warning in .recacheSubclasses(def@className, def, env): undefined subclass
## "pcorMatrix" of class "ConstValORExpr"; definition not updated
```

```
## Warning in .recacheSubclasses(def@className, def, env): undefined subclass
## "pcorMatrix" of class "ConstValORNULL"; definition not updated
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
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.3     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ lubridate 1.9.2     ✔ tibble    3.2.1
## ✔ purrr     1.0.2     ✔ tidyr     1.3.0
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ tidyr::expand()    masks Matrix::expand()
## ✖ dplyr::filter()    masks stats::filter()
## ✖ dplyr::id()        masks CVXR::id()
## ✖ purrr::is_vector() masks CVXR::is_vector()
## ✖ dplyr::lag()       masks stats::lag()
## ✖ purrr::lift()      masks caret::lift()
## ✖ tidyr::pack()      masks Matrix::pack()
## ✖ dplyr::recode()    masks car::recode()
## ✖ purrr::some()      masks car::some()
## ✖ tidyr::unpack()    masks Matrix::unpack()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```


```
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
```
<img src="{{< blogdown/postref >}}index_files/figure-html/2024-polling-averages-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/2024-polling-averages-2.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/2024-polling-averages-3.png" width="672" />

<img src="{{< blogdown/postref >}}index_files/figure-html/more-polling-averages-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/more-polling-averages-2.png" width="672" />

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
```

```
## 
## Call:
## lm(formula = pv2p ~ nov_poll, data = subset(d_poll_nov, party == 
##     "DEM"))
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.0155 -2.4353 -0.3752  1.4026  5.8014 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  14.2936     7.1693   1.994 0.069416 .  
## nov_poll      0.7856     0.1608   4.885 0.000376 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.968 on 12 degrees of freedom
## Multiple R-squared:  0.6654,	Adjusted R-squared:  0.6375 
## F-statistic: 23.86 on 1 and 12 DF,  p-value: 0.0003756
```

```
## 
## Call:
## lm(formula = pv2p ~ nov_poll, data = d_poll_nov)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -4.6190 -1.6523 -0.5808  1.3629  6.0220 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 17.92577    4.15543   4.314 0.000205 ***
## nov_poll     0.70787    0.09099   7.780 2.97e-08 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.75 on 26 degrees of freedom
## Multiple R-squared:  0.6995,	Adjusted R-squared:  0.6879 
## F-statistic: 60.52 on 1 and 26 DF,  p-value: 2.974e-08
```

```
## `summarise()` has grouped output by 'year', 'party'. You can override using the
## `.groups` argument.
```

```
## 
## Call:
## lm(formula = paste0("pv2p ~ ", paste0("poll_weeks_left_", 0:30, 
##     collapse = " + ")), data = d_poll_weeks_train)
## 
## Residuals:
## ALL 28 residuals are 0: no residual degrees of freedom!
## 
## Coefficients: (4 not defined because of singularities)
##                    Estimate Std. Error t value Pr(>|t|)
## (Intercept)        28.25534        NaN     NaN      NaN
## poll_weeks_left_0   3.24113        NaN     NaN      NaN
## poll_weeks_left_1   0.02516        NaN     NaN      NaN
## poll_weeks_left_2  -8.87360        NaN     NaN      NaN
## poll_weeks_left_3   7.91455        NaN     NaN      NaN
## poll_weeks_left_4   0.74573        NaN     NaN      NaN
## poll_weeks_left_5   1.41567        NaN     NaN      NaN
## poll_weeks_left_6  -4.58444        NaN     NaN      NaN
## poll_weeks_left_7   4.63361        NaN     NaN      NaN
## poll_weeks_left_8  -0.95121        NaN     NaN      NaN
## poll_weeks_left_9  -1.55307        NaN     NaN      NaN
## poll_weeks_left_10 -1.38062        NaN     NaN      NaN
## poll_weeks_left_11  1.74881        NaN     NaN      NaN
## poll_weeks_left_12 -1.28871        NaN     NaN      NaN
## poll_weeks_left_13 -0.08482        NaN     NaN      NaN
## poll_weeks_left_14  0.87498        NaN     NaN      NaN
## poll_weeks_left_15 -0.16310        NaN     NaN      NaN
## poll_weeks_left_16 -0.34501        NaN     NaN      NaN
## poll_weeks_left_17 -0.38689        NaN     NaN      NaN
## poll_weeks_left_18 -0.06281        NaN     NaN      NaN
## poll_weeks_left_19 -0.17204        NaN     NaN      NaN
## poll_weeks_left_20  1.52230        NaN     NaN      NaN
## poll_weeks_left_21 -0.72487        NaN     NaN      NaN
## poll_weeks_left_22 -2.76531        NaN     NaN      NaN
## poll_weeks_left_23  4.90361        NaN     NaN      NaN
## poll_weeks_left_24 -2.04431        NaN     NaN      NaN
## poll_weeks_left_25 -0.76078        NaN     NaN      NaN
## poll_weeks_left_26 -0.47860        NaN     NaN      NaN
## poll_weeks_left_27       NA         NA      NA       NA
## poll_weeks_left_28       NA         NA      NA       NA
## poll_weeks_left_29       NA         NA      NA       NA
## poll_weeks_left_30       NA         NA      NA       NA
## 
## Residual standard error: NaN on 0 degrees of freedom
## Multiple R-squared:      1,	Adjusted R-squared:    NaN 
## F-statistic:   NaN on 27 and 0 DF,  p-value: NA
```

<img src="{{< blogdown/postref >}}index_files/figure-html/regularization-1.png" width="672" />

```
## 32 x 1 sparse Matrix of class "dgCMatrix"
##                              s1
## (Intercept)        29.951147799
## poll_weeks_left_0   0.032163983
## poll_weeks_left_1   0.025440084
## poll_weeks_left_2   0.024404320
## poll_weeks_left_3   0.024688870
## poll_weeks_left_4   0.024695646
## poll_weeks_left_5   0.024725772
## poll_weeks_left_6   0.024080438
## poll_weeks_left_7   0.023636908
## poll_weeks_left_8   0.024487501
## poll_weeks_left_9   0.026498950
## poll_weeks_left_10  0.025642838
## poll_weeks_left_11  0.021361476
## poll_weeks_left_12  0.017386999
## poll_weeks_left_13  0.013378030
## poll_weeks_left_14  0.010078675
## poll_weeks_left_15  0.007248494
## poll_weeks_left_16  0.012943440
## poll_weeks_left_17  0.012879654
## poll_weeks_left_18  0.011157452
## poll_weeks_left_19  0.008302783
## poll_weeks_left_20  0.004012987
## poll_weeks_left_21  0.003350434
## poll_weeks_left_22  0.004458406
## poll_weeks_left_23  0.001019583
## poll_weeks_left_24 -0.002711193
## poll_weeks_left_25 -0.002447895
## poll_weeks_left_26  0.001121142
## poll_weeks_left_27  0.005975853
## poll_weeks_left_28  0.011623984
## poll_weeks_left_29  0.013833925
## poll_weeks_left_30  0.018964139
```

<img src="{{< blogdown/postref >}}index_files/figure-html/regularization-2.png" width="672" />

```
## 32 x 1 sparse Matrix of class "dgCMatrix"
##                             s1
## (Intercept)        24.57897724
## poll_weeks_left_0   0.50149421
## poll_weeks_left_1   .         
## poll_weeks_left_2   .         
## poll_weeks_left_3   .         
## poll_weeks_left_4   .         
## poll_weeks_left_5   0.08461518
## poll_weeks_left_6   .         
## poll_weeks_left_7   .         
## poll_weeks_left_8   .         
## poll_weeks_left_9   0.17064525
## poll_weeks_left_10  .         
## poll_weeks_left_11  .         
## poll_weeks_left_12  .         
## poll_weeks_left_13  .         
## poll_weeks_left_14  .         
## poll_weeks_left_15  0.01147512
## poll_weeks_left_16  .         
## poll_weeks_left_17  .         
## poll_weeks_left_18  0.23694416
## poll_weeks_left_19  .         
## poll_weeks_left_20  .         
## poll_weeks_left_21  .         
## poll_weeks_left_22  .         
## poll_weeks_left_23  .         
## poll_weeks_left_24  .         
## poll_weeks_left_25 -0.55693209
## poll_weeks_left_26  .         
## poll_weeks_left_27  .         
## poll_weeks_left_28  .         
## poll_weeks_left_29  .         
## poll_weeks_left_30  0.11120476
```

<img src="{{< blogdown/postref >}}index_files/figure-html/regularization-3.png" width="672" />

```
## Warning: Option grouped=FALSE enforced in cv.glmnet, since < 3 observations per
## fold

## Warning: Option grouped=FALSE enforced in cv.glmnet, since < 3 observations per
## fold

## Warning: Option grouped=FALSE enforced in cv.glmnet, since < 3 observations per
## fold
```

```
## [1] 9.575001
```

```
## [1] 3.61215
```

```
## [1] 4.021314
```

<img src="{{< blogdown/postref >}}index_files/figure-html/regularization-4.png" width="672" />

```
## [1]  7 36
```

```
## Warning: Option grouped=FALSE enforced in cv.glmnet, since < 3 observations per
## fold
```

```
##            s1
## [1,] 51.79268
## [2,] 50.65879
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
## # A tibble: 24 × 60
##     year quarter   GDP GDP_growth_quarterly  RDPI RDPI_growth_quarterly   CPI
##    <dbl>   <dbl> <dbl>                <dbl> <dbl>                 <dbl> <dbl>
##  1  1976       2 1852.                  3   4675.                   2.3  56.4
##  2  1976       2 1852.                  3   4675.                   2.3  56.4
##  3  1980       2 2797.                 -8   5144.                  -3.5  81.7
##  4  1980       2 2797.                 -8   5144.                  -3.5  81.7
##  5  1984       2 4010.                  7.1 5981.                   6.6 104. 
##  6  1984       2 4010.                  7.1 5981.                   6.6 104. 
##  7  1988       2 5190.                  5.4 6847.                   4.7 118. 
##  8  1988       2 5190.                  5.4 6847.                   4.7 118. 
##  9  1992       2 6471.                  4.4 7569.                   3.8 140. 
## 10  1992       2 6471.                  4.4 7569.                   3.8 140. 
## # ℹ 14 more rows
## # ℹ 53 more variables: unemployment <dbl>, sp500_open <dbl>, sp500_high <dbl>,
## #   sp500_low <dbl>, sp500_close <dbl>, sp500_adj_close <dbl>,
## #   sp500_volume <dbl>, party <chr>, poll_weeks_left_0 <dbl>,
## #   poll_weeks_left_1 <dbl>, poll_weeks_left_2 <dbl>, poll_weeks_left_3 <dbl>,
## #   poll_weeks_left_4 <dbl>, poll_weeks_left_5 <dbl>, poll_weeks_left_6 <dbl>,
## #   poll_weeks_left_7 <dbl>, poll_weeks_left_8 <dbl>, …
```

```
##      GDP GDP_growth_quarterly RDPI RDPI_growth_quarterly CPI unemployment
##      sp500_close incumbent gdp_growth_x_incumbent rdpi_growth_quarterly
##      cpi_x_incumbent unemployment_x_incumbent sp500_x_incumbent pv2p_lag1
##      pv2p_lag2
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
## numeric(0)
```

```
## [1] 0.8556624
```

```
## [1] 0.1443376
```

```
## numeric(0)
```

```
## numeric(0)
```

```
## [1] 0.1443376
```

```
## [1] 0.8556624
```

```
## numeric(0)
```

```
## numeric(0)
```





