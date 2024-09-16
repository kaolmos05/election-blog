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

```r
# Load popular vote data. 
d_popvote <- read_csv("popvote_1948-2020.csv")
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
```

```r
# Load economic data from FRED: https://fred.stlouisfed.org. 
# Variables, units, & ranges: 
# GDP, billions $, 1947-2024
# GDP_growth_quarterly, %
# RDPI, $, 1959-2024
# RDPI_growth_quarterly, %
# CPI, $ index, 1947-2024
# unemployment, %, 1948-2024
# sp500_, $, 1927-2024 
d_fred <- read_csv("fred_econ.csv")
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

```r
# Load economic data from the BEA: https://apps.bea.gov/iTable/?reqid=19&step=2&isuri=1&categories=survey#eyJhcHBpZCI6MTksInN0ZXBzIjpbMSwyLDMsM10sImRhdGEiOltbImNhdGVnb3JpZXMiLCJTdXJ2ZXkiXSxbIk5JUEFfVGFibGVfTGlzdCIsIjI2NCJdLFsiRmlyc3RfWWVhciIsIjE5NDciXSxbIkxhc3RfWWVhciIsIjIwMjQiXSxbIlNjYWxlIiwiMCJdLFsiU2VyaWVzIiwiUSJdXX0=.
# GDP, 1947-2024 (all)
# GNP
# RDPI
# Personal consumption expenditures
# Goods
# Durable goods
# Nondurable goods
# Services 
# Population (midperiod, thousands)
d_bea <- read_csv("bea_econ.csv") |> 
  rename(year = "Year",
         quarter = "Quarter", 
         gdp = "Gross domestic product", 
         gnp = "Gross national product", 
         dpi = "Disposable personal income", 
         consumption = "Personal consumption expenditures", 
         goods = "Goods", 
         durables = "Durable goods", 
         nondurables = "Nondurable goods", 
         services = "Services", 
         pop = "Population (midperiod, thousands)")
```

```
## Rows: 310 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): Quarter
## dbl (10): Year, Gross domestic product, Gross national product, Disposable p...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
# Filter and merge data. 
d_inc_econ <- d_popvote |> 
  filter(incumbent_party == TRUE) |> 
  select(year, pv, pv2p, winner) |> 
  left_join(d_fred |> filter(quarter == 2)) |> 
  left_join(d_bea |> filter(quarter == "Q2") |> select(year, dpi))
```

```
## Joining with `by = join_by(year)`
## Joining with `by = join_by(year)`
```

```r
  # N.B. two different sources of data to use, FRED & BEA. 
  # We are using second-quarter data since that is the latest 2024 release. 
  # Feel free to experiment with different data/combinations!
```

####----------------------------------------------------------#
#### Understanding the relationship between economy and vote share. 
####----------------------------------------------------------#

```r
# Create scatterplot to visualize relationship between Q2 GDP growth and 
# incumbent vote share. 
d_inc_econ |> 
  ggplot(aes(x = GDP_growth_quarterly, y = pv2p, label = year)) + 
  geom_text() + 
  geom_hline(yintercept = 50, lty = 2) + 
  geom_vline(xintercept = 0.01, lty = 2) +
  labs(x = "Second Quarter GDP Growth (%)", 
       y = "Incumbent Party's National Popular Vote Share") + 
  theme_bw()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

```r
# Remove 2020 from plot.
d_inc_econ_2 <- d_inc_econ |>
  filter(year != 2020)

d_inc_econ_2 |> 
  ggplot(aes(x = GDP_growth_quarterly, y = pv2p, label = year)) + 
  geom_text() + 
  geom_hline(yintercept = 50, lty = 2) + 
  geom_vline(xintercept = 0.01, lty = 2) + 
  labs(x = "Second Quarter GDP Growth (%)", 
       y = "Incumbent Party's National Popular Vote Share") + 
  theme_bw()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-2.png" width="672" />

```r
# Compute correlations between Q2 GDP growth and incumbent vote 2-party vote share.
cor(d_inc_econ$GDP_growth_quarterly, 
    d_inc_econ$pv2p)
```

```
## [1] 0.4336956
```

```r
cor(d_inc_econ_2$GDP_growth_quarterly, 
    d_inc_econ_2$pv2p)
```

```
## [1] 0.569918
```

```r
# Fit bivariate OLS. 
reg_econ <- lm(pv2p ~ GDP_growth_quarterly, 
               data = d_inc_econ)
reg_econ |> summary()
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

```r
reg_econ_2 <- lm(pv2p ~ GDP_growth_quarterly, 
                         data = d_inc_econ_2)
reg_econ_2 |> summary()
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

```r
# Can add bivariate regression lines to our scatterplots. 
d_inc_econ |> 
  ggplot(aes(x = GDP_growth_quarterly, y = pv2p, label = year)) + 
  geom_text() + 
  geom_smooth(method = "lm", formula = y ~ x) +
  geom_hline(yintercept = 50, lty = 2) + 
  geom_vline(xintercept = 0.01, lty = 2) + 
  labs(x = "Second Quarter GDP Growth (%)", 
       y = "Incumbent Party's National Popular Vote Share", 
       title = "Y = 51.25 + 0.274 * X") + 
  theme_bw() + 
  theme(plot.title = element_text(size = 18))
```

```
## Warning: The following aesthetics were dropped during statistical transformation: label
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-3.png" width="672" />

```r
d_inc_econ_2 |> 
  ggplot(aes(x = GDP_growth_quarterly, y = pv2p, label = year)) + 
  geom_text() + 
  geom_smooth(method = "lm", formula = y ~ x) +
  geom_hline(yintercept = 50, lty = 2) + 
  geom_vline(xintercept = 0.01, lty = 2) + 
  labs(x = "Second Quarter GDP Growth (%)", 
       y = "Incumbent Party's National Popular Vote Share", 
       title = "Y = 49.38 + 0.737 * X") + 
  theme_bw() + 
  theme(plot.title = element_text(size = 18))
```

```
## Warning: The following aesthetics were dropped during statistical transformation: label
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-4.png" width="672" />

```r
# Evaluate the in-sample fit of your preferred model.
# R2.
summary(reg_econ_2)$r.squared
```

```
## [1] 0.3248066
```

```r
summary(reg_econ_2)$adj.r.squared
```

```
## [1] 0.282607
```

```r
# Predicted and actual comparisons.
plot(d_inc_econ$year, 
     d_inc_econ$pv2p, 
     type="l",
     main="True Y (Line), Predicted Y (Dot) for Each Year")
points(d_inc_econ$year, predict(reg_econ_2, d_inc_econ))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-5.png" width="672" />

```r
# Residuals and regression innards. 
plot(reg_econ_2)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-6.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-7.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-8.png" width="672" /><img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-9.png" width="672" />

```r
# MSE.
hist(reg_econ_2$model$pv2p - reg_econ_2$fitted.values, 
     main = "Histogram of True Y - Predicted Y")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-10.png" width="672" />

```r
mse <- mean((reg_econ_2$model$pv2p - reg_econ_2$fitted.values)^2)
mse
```

```
## [1] 17.7027
```

```r
sqrt(mse)
```

```
## [1] 4.207458
```

```r
# Model Testing: Leave-One-Out
(out_samp_pred <- predict(reg_econ_2, d_inc_econ[d_inc_econ$year == 2020,]))
```

```
##        1 
## 28.75101
```

```r
(out_samp_truth <- d_inc_econ |> filter(year == 2020) |> select(pv2p))
```

```
## # A tibble: 1 × 1
##    pv2p
##   <dbl>
## 1  47.7
```

```r
out_samp_pred - out_samp_truth # Dangers of fundamentals-only model!
```

```
##        pv2p
## 1 -18.97913
```

```r
# https://www.nytimes.com/2020/07/30/business/economy/q2-gdp-coronavirus-economy.html

# Model Testing: Cross-Validation (One Run)
years_out_samp <- sample(d_inc_econ_2$year, 9) 
mod <- lm(pv2p ~ GDP_growth_quarterly, 
          d_inc_econ_2[!(d_inc_econ_2$year %in% years_out_samp),])
out_samp_pred <- predict(mod, d_inc_econ_2[d_inc_econ_2$year %in% years_out_samp,])
out_samp_truth <- d_inc_econ_2$pv2p[d_inc_econ_2$year %in% years_out_samp]
mean(out_samp_pred - out_samp_truth)
```

```
## [1] 2.040077
```

```r
# Model Testing: Cross-Validation (1000 Runs)
out_samp_errors <- sapply(1:1000, function(i) {
  years_out_samp <- sample(d_inc_econ_2$year, 9) 
  mod <- lm(pv2p ~ GDP_growth_quarterly, 
            d_inc_econ_2[!(d_inc_econ_2$year %in% years_out_samp),])
  out_samp_pred <- predict(mod, d_inc_econ_2[d_inc_econ_2$year %in% years_out_samp,])
  out_samp_truth <- d_inc_econ_2$pv2p[d_inc_econ_2$year %in% years_out_samp]
  mean(out_samp_pred - out_samp_truth)
})

mean(abs(out_samp_errors))
```

```
## [1] 1.817551
```

```r
hist(out_samp_errors,
     xlab = "",
     main = "Mean Out-of-Sample Residual\n(1000 Runs of Cross-Validation)")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-11.png" width="672" />

####----------------------------------------------------------#
#### Predicting 2024 results using simple economy model. 
####----------------------------------------------------------#
# Sequester 2024 data.


```r
# Sequester 2024 data.
GDP_new <- d_fred |> 
  filter(year == 2024 & quarter == 2) |> 
  select(GDP_growth_quarterly)

# Predict.
predict(reg_econ_2, GDP_new)
```

```
##        1 
## 51.58486
```

```r
# Predict uncertainty.
predict(reg_econ_2, GDP_new, interval = "prediction")
```

```
##        fit      lwr     upr
## 1 51.58486 41.85982 61.3099
```
