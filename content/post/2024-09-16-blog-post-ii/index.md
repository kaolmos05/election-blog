---
title: Blog Post II
author: Package Build
date: '2024-09-16'
slug: blog-post-ii
categories: []
tags: []
---
#' @title GOV 1347: Week 2 (Economics) Laboratory Session
#' @author Matthew E. Dardet
#' @date September 10, 2024

####----------------------------------------------------------#
#### Preamble
####----------------------------------------------------------#
Date: September, 16, 2024 

Can we predict election outcomes using only the state of
the economy? If so, how well?


```r
# Load libraries.
## install via `install.packages("name")`
library(car)
```

```
## Loading required package: carData
```

```r
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.3     ✔ readr     2.1.4
## ✔ forcats   1.0.0     ✔ stringr   1.5.0
## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
## ✔ purrr     1.0.2
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ✖ dplyr::recode() masks car::recode()
## ✖ purrr::some()   masks car::some()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```



## set working directory here
# setwd("~")

####----------------------------------------------------------#
#### Read, merge, and process data.
####----------------------------------------------------------#

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
## Rows: 387 Columns: 14
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## dbl (14): year, quarter, GDP, GDP_growth_quarterly, RDPI, RDPI_growth_quarte...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Rows: 310 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): Quarter
## dbl (10): Year, Gross domestic product, Gross national product, Disposable p...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
## Joining with `by = join_by(year)`
## Joining with `by = join_by(year)`
```

####----------------------------------------------------------#
#### Understanding the relationship between economy and vote share. 
####----------------------------------------------------------#

```
## [1] 0.4336956
```

```
## 
## Call:
## lm(formula = pv2p ~ GDP_growth_quarterly, data = d_inc_econ)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -6.7666 -3.3847 -0.7697  2.9121  8.8809 
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           51.2580     1.1399  44.968   <2e-16 ***
## GDP_growth_quarterly   0.2739     0.1380   1.985   0.0636 .  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.834 on 17 degrees of freedom
## Multiple R-squared:  0.1881,	Adjusted R-squared:  0.1403 
## F-statistic: 3.938 on 1 and 17 DF,  p-value: 0.06358
```

```
## 
## Call:
## lm(formula = pv2p ~ GDP_growth_quarterly, data = d_inc_econ_2)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
## -6.237 -4.160  0.450  1.904  8.728 
## 
## Coefficients:
##                      Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           49.3751     1.4163  34.862   <2e-16 ***
## GDP_growth_quarterly   0.7366     0.2655   2.774   0.0135 *  
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 4.463 on 16 degrees of freedom
## Multiple R-squared:  0.3248,	Adjusted R-squared:  0.2826 
## F-statistic: 7.697 on 1 and 16 DF,  p-value: 0.01354
```

```
## Warning: The following aesthetics were dropped during statistical transformation: label
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

```
## Warning: The following aesthetics were dropped during statistical transformation: label
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-2.png" width="672" />

```
## [1] 0.3248066
```

```
## [1] 0.282607
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-3.png" width="672" />

```
## [1] 17.7027
```

```
## [1] 4.207458
```

```
##        1 
## 28.75101
```

```
## # A tibble: 1 × 1
##    pv2p
##   <dbl>
## 1  47.7
```

```
##        pv2p
## 1 -18.97913
```
The 


```r
d_inc_econ_unemployment<- d_inc_econ |>
  select(unemployment, pv2p, year)

correlation <- cor(d_inc_econ_unemployment$unemployment, d_inc_econ_unemployment$pv2p)

ggplot(d_inc_econ_unemployment, aes(x = unemployment, y = pv2p)) +
  geom_point() +
  labs(
    title = "Relationship between Unemployment and Incumbent Vote Share",
    x = "Unemployment Rate (%)",
    y = "Incumbent Party's National Popular Vote Share (%)"
  ) +
  theme_bw()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

```r
reg_econ_3 <- lm(pv2p ~ unemployment, data = d_inc_econ_unemployment)

summary(reg_econ_3)
```

```
## 
## Call:
## lm(formula = pv2p ~ unemployment, data = d_inc_econ_unemployment)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -7.9906 -2.6616 -0.9256  2.4016  9.9399 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   53.6260     3.4614  15.493 1.85e-11 ***
## unemployment  -0.3117     0.5475  -0.569    0.577    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.314 on 17 degrees of freedom
## Multiple R-squared:  0.01872,	Adjusted R-squared:  -0.03901 
## F-statistic: 0.3243 on 1 and 17 DF,  p-value: 0.5765
```

```r
d_inc_econ_unemployment |> 
  ggplot(aes(x = unemployment, y = pv2p, label = year)) + 
  geom_text() + 
  geom_smooth(method = "lm", formula = y ~ x) +
  geom_hline(yintercept = 50, lty = 2) + 
  geom_vline(xintercept = 0.01, lty = 2) + 
  labs(x = "Unemployment Rate (%)", 
       y = "Incumbent Party's National Popular Vote Share", 
       title = "Y = 53.6260 + -0.3117 * X") + 
  theme_bw() 
```

```
## Warning: The following aesthetics were dropped during statistical transformation: label
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-2.png" width="672" />

```r
correlation <- cor(d_inc_econ$RDPI_growth_quarterly, d_inc_econ$pv2p)

reg_econ_4 <- lm(pv2p ~ RDPI_growth_quarterly, data = d_inc_econ)

summary(reg_econ_4)
```

```
## 
## Call:
## lm(formula = pv2p ~ RDPI_growth_quarterly, data = d_inc_econ)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -7.2308 -2.9971 -0.8344  2.4629  9.9337 
## 
## Coefficients:
##                       Estimate Std. Error t value Pr(>|t|)    
## (Intercept)           51.97420    1.49322  34.807   <2e-16 ***
## RDPI_growth_quarterly -0.02831    0.12446  -0.227    0.823    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 5.357 on 17 degrees of freedom
## Multiple R-squared:  0.003033,	Adjusted R-squared:  -0.05561 
## F-statistic: 0.05172 on 1 and 17 DF,  p-value: 0.8228
```

```r
d_inc_econ |> 
  ggplot(aes(x = RDPI_growth_quarterly, y = pv2p, label = year)) + 
  geom_text() + 
  geom_smooth(method = "lm", formula = y ~ x) +
  geom_hline(yintercept = 50, lty = 2) + 
  geom_vline(xintercept = 0.01, lty = 2) + 
  labs(x = "RDPI Growth Q2 (%)", 
       y = "Incumbent Party's National Popular Vote Share") + 
  theme_bw() 
```

```
## Warning: The following aesthetics were dropped during statistical transformation: label
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />


####----------------------------------------------------------#
#### Predicting 2024 results using simple economy model. 
####----------------------------------------------------------#
# Sequester 2024 data.


```r
# Sequester 2024 data.
GDP_new <- d_fred |> 
  filter(year == 2024 & quarter == 2) |> 
  select(GDP_growth_quarterly, RDPI_growth_quarterly, unemployment)

# Predict.
predict(reg_econ_2, GDP_new)
```

```
##        1 
## 51.58486
```

```r
predict(reg_econ_3, GDP_new)
```

```
##        1 
## 52.37907
```

```r
predict(reg_econ_4, GDP_new)
```

```
##       1 
## 51.9459
```

```r
# Predict uncertainty.
predict(reg_econ_2, GDP_new, interval = "prediction")
```

```
##        fit      lwr     upr
## 1 51.58486 41.85982 61.3099
```

```r
predict(reg_econ_3, GDP_new, interval = "prediction")
```

```
##        fit      lwr      upr
## 1 52.37907 40.66441 64.09372
```

```r
predict(reg_econ_4, GDP_new, interval = "prediction")
```

```
##       fit      lwr      upr
## 1 51.9459 40.25082 63.64097
```
