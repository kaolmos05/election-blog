---
title: Blog Post 3
author: Package Build
date: '2024-09-23'
slug: blog-post-3
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

```r
####----------------------------------------------------------#
#### Visualizing poll variation over time.
####----------------------------------------------------------#

# Plot 1. Polling Averages Over 2020 Election Cycle with and Without Event Labels. 
d_pollav_natl |> 
  filter(year == 2020) |> 
  ggplot(aes(x = poll_date, y = poll_support, color = party)) +
  geom_point(size = 1) + 
  geom_line() + 
  scale_x_date(date_labels = "%b %d") + 
  scale_color_manual(values = c("dodgerblue4", "firebrick1")) +
  labs(x = "Date",
       y = "Average Poll Approval", 
       title = "Polling Averages by Date, 2020") + 
  theme_classic()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

```r
# Plot 2. RNC and DNC Bumps in 2020. 
d_pollav_natl |> 
  filter(year == 2020) |> 
  ggplot(aes(x = poll_date, y = poll_support, color = party)) +
  geom_rect(xmin = as.Date("2020-08-17"), xmax = as.Date("2020-08-20"), ymin = 47.5, ymax = 100, alpha = 0.1, color = NA, fill = "grey") + 
  annotate("text", x = as.Date("2020-08-07"), y = 51.5, label = "DNC", size = 4) + 
  geom_rect(xmin = as.Date("2020-08-24"), xmax = as.Date("2020-08-27"), ymin = 0, ymax = 46, alpha = 0.1, color = NA, fill = "grey") +
  annotate("text", x = as.Date("2020-09-04"), y = 45, label = "RNC", size = 4) +
  geom_point(size = 1) + 
  geom_line() + 
  scale_x_date(date_labels = "%b %d") + 
  scale_color_manual(values = c("dodgerblue4", "firebrick1")) +
  labs(x = "Date",
       y = "Average Poll Approval", 
       title = "Polling Averages by Date, 2020 (with Conference Dates)") + 
  theme_classic()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-2.png" width="672" />

```r
# Plot 3. Adding in Extra Dates of Interest for 2020 —— "game changers"? 
d_pollav_natl |> 
  filter(year == 2020) |> 
  ggplot(aes(x = poll_date, y = poll_support, color = party)) +
  geom_rect(xmin = as.Date("2020-08-17"), xmax = as.Date("2020-08-20"), ymin = 47.5, ymax = 100, alpha = 0.1, color = NA, fill = "grey") + 
  annotate("text", x = as.Date("2020-08-07"), y = 51.5, label = "DNC", size = 4) +
  geom_rect(xmin = as.Date("2020-08-24"), xmax = as.Date("2020-08-27"), ymin = 0, ymax = 47.2, alpha = 0.1, color = NA, fill = "grey") +
  annotate("text", x = as.Date("2020-09-04"), y = 45, label = "RNC", size = 4) +
  geom_rect(xmin = as.Date("2020-10-02"), xmax = as.Date("2020-10-12"), ymin = 0, ymax = 42.7, alpha = 0.05, color = NA, fill = "grey") +
  
  geom_point(size = 1) + 
  geom_line() + 
  
  geom_segment(x = as.Date("2020-03-12"), xend = as.Date("2020-03-12"), y = 0, yend = 44.8, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-03-12"), y = 42.5, label = "COVID \n Market Crash", size = 3) +
  geom_segment(x = as.Date("2020-04-08"), xend = as.Date("2020-04-08"), y = 49, yend = 100, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-03-25"), y = 51.3, label = "Bernie Ends Run", size = 3) +
  geom_segment(x = as.Date("2020-04-16"), xend = as.Date("2020-04-16"), y = 0, yend = 44, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-04-16"), y = 44.7, label = "22 mil \n Unemployment", size = 3) +
  geom_segment(x = as.Date("2020-05-27"), xend = as.Date("2020-05-27"), y = 0, yend = 43, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-06-05"), y = 44, label = "100k COVID Dead, \n George Floyd", size = 3) +
  
  geom_segment(x = as.Date("2020-07-14"), xend = as.Date("2020-07-14"), y = 0, yend = 50.3, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-06-19"), y = 47.5, label = "Moderna Announces", size = 3) +
  
  geom_segment(x = as.Date("2020-09-29"), xend = as.Date("2020-09-29"), y = 50, yend = 100, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-9-12"), y = 49.5, label = "Pres. Debate", size = 3) +
  geom_segment(x = as.Date("2020-10-07"), xend = as.Date("2020-10-07"), y = 51.7, yend = 100, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-10-17"), y = 50.3, label = "VP Debate", size = 3) +
  geom_segment(x = as.Date("2020-10-22"), xend = as.Date("2020-10-22"), y = 52, yend = 100, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-10-30"), y = 51.5, label = "Pres. Debate", size = 3) +
  annotate("text", x = as.Date("2020-10-15"), y = 43.7, label = "Trump Has COVID", size = 3) +
  geom_segment(x = as.Date("2020-09-18"), xend = as.Date("2020-09-18"), y = 50, yend = 100, linetype = "dashed", alpha = 0.4, color = "grey") +
  annotate("text", x = as.Date("2020-09-03"), y = 51.5, label = "RBG Passes", size = 3) +
  
  scale_x_date(date_labels = "%b %d") + 
  scale_color_manual(values = c("dodgerblue4", "firebrick1")) +
  labs(x = "Date",
       y = "Average Poll Approval", 
       title = "Polling Averages by Date, 2020 (with Game Changers?)") + 
  theme_classic()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-3.png" width="672" />

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-2.png" width="672" />

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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-2.png" width="672" />

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

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-3.png" width="672" />

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
## [1] 4.642786
```

```
## [1] 5.578409
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-4.png" width="672" />

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

## Warning: number of rows of result is not a multiple of vector length (arg 1)
```

```
##      s1
```


# Ensemble 1: Predict based on unweighted (or equally weighted) ensemble model between polls and fundamentals models. 
(unweighted.ensemble.pred <- (polls.pred + fund.pred)/2)

# Ensemble 2: Weight based on polls mattering closer to November. (Nate Silver)
election_day_2024 <- "2024-11-05"
today <- "2024-09-18"
days_left <- as.numeric(as.Date(election_day_2024) - as.Date(today))

(poll_model_weight <- 1- (1/sqrt(days_left)))
(fund_model_weight <- 1/sqrt(days_left))

(ensemble.2.pred <- polls.pred * poll_model_weight + fund.pred * fund_model_weight)  

# Ensemble 3. Weight based on fundamentals mattering closer to November. (Gelman & King, 1993)
(poll_model_weight <- 1/sqrt(days_left))
(fund_model_weight <- 1-(1/sqrt(days_left)))

(ensemble.3.pred <- polls.pred * poll_model_weight + fund.pred * fund_model_weight)



