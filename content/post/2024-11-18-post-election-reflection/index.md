---
title: 'Post Election Reflection '
author: Package Build
date: '2024-11-18'
slug: post-election-reflection
categories: []
tags: []
---

``` r
library(geofacet)
library(glmnet)
```

```
## Loading required package: Matrix
```

```
## Loaded glmnet 4.1-8
```

``` r
library(usmap)
library(ggpubr)
```

```
## Loading required package: ggplot2
```

``` r
library(ggthemes)
library(haven)
library(kableExtra)
library(maps)
library(mgcv)
```

```
## Loading required package: nlme
```

```
## This is mgcv 1.9-1. For overview type 'help("mgcv-package")'.
```

``` r
library(mgcViz)
```

```
## Loading required package: qgam
```

```
## Registered S3 method overwritten by 'GGally':
##   method from   
##   +.gg   ggplot2
```

```
## Registered S3 method overwritten by 'mgcViz':
##   method from  
##   +.gg   GGally
```

```
## 
## Attaching package: 'mgcViz'
```

```
## The following objects are masked from 'package:stats':
## 
##     qqline, qqnorm, qqplot
```

``` r
library(RColorBrewer)
library(scales)
library(sf)
```

```
## Linking to GEOS 3.11.0, GDAL 3.5.3, PROJ 9.1.0; sf_use_s2() is TRUE
```

``` r
library(spData)
```

```
## To access larger datasets in this package, install the spDataLarge
## package with: `install.packages('spDataLarge',
## repos='https://nowosad.github.io/drat/', type='source')`
```

``` r
library(stargazer)
```

```
## 
## Please cite as:
```

```
##  Hlavac, Marek (2022). stargazer: Well-Formatted Regression and Summary Statistics Tables.
```

```
##  R package version 5.2.3. https://CRAN.R-project.org/package=stargazer
```

``` r
library(tidygeocoder)
library(tidyverse)
```

```
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ lubridate 1.9.3     ✔ tibble    3.2.1
## ✔ purrr     1.0.2     ✔ tidyr     1.3.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ readr::col_factor() masks scales::col_factor()
## ✖ dplyr::collapse()   masks nlme::collapse()
## ✖ purrr::discard()    masks scales::discard()
## ✖ tidyr::expand()     masks Matrix::expand()
## ✖ dplyr::filter()     masks stats::filter()
## ✖ dplyr::group_rows() masks kableExtra::group_rows()
## ✖ dplyr::lag()        masks stats::lag()
## ✖ purrr::map()        masks maps::map()
## ✖ tidyr::pack()       masks Matrix::pack()
## ✖ tidyr::unpack()     masks Matrix::unpack()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library(tigris)
```

```
## To enable caching of data, set `options(tigris_use_cache = TRUE)`
## in your R script or .Rprofile.
```

``` r
library(tmap)
```

```
## Breaking News: tmap 3.x is retiring. Please test v4, e.g. with
## remotes::install_github('r-tmap/tmap')
```

``` r
library(tmaptools)
library(viridis)
```

```
## Loading required package: viridisLite
## 
## Attaching package: 'viridis'
## 
## The following object is masked from 'package:scales':
## 
##     viridis_pal
## 
## The following object is masked from 'package:maps':
## 
##     unemp
```

``` r
library(broom)
library(knitr)
library(dplyr)
library(Metrics)
```



``` r
state_pres_vote <- read_csv("state_votes_pres_2024.csv")
```

```
## New names:
## Rows: 52 Columns: 42
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (42): FIPS, Geographic Name, Geographic Subtype, Total Vote, Kamala D. H...
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `0` -> `0...31`
## • `0` -> `0...32`
## • `0` -> `0...33`
## • `0` -> `0...34`
## • `0` -> `0...35`
## • `0` -> `0...36`
## • `0` -> `0...37`
## • `0` -> `0...38`
## • `0` -> `0...39`
## • `0` -> `0...40`
## • `0` -> `0...41`
## • `0` -> `0...42`
```

``` r
as.numeric("Donald J. Trump")
```

```
## Warning: NAs introduced by coercion
```

```
## [1] NA
```

``` r
as.numeric("Kamala D. Harris")
```

```
## Warning: NAs introduced by coercion
```

```
## [1] NA
```

``` r
state_pres_vote <- state_pres_vote |>
  select(state = "Geographic Name", Trump = "Donald J. Trump", Harris = "Kamala D. Harris") |>
  mutate(
    Trump = as.numeric(Trump),
    Harris = as.numeric(Harris),
    D2pv = Harris/(Trump + Harris)
  ) |> 
  drop_na()
```

```
## Warning: There were 2 warnings in `mutate()`.
## The first warning was:
## ℹ In argument: `Trump = as.numeric(Trump)`.
## Caused by warning:
## ! NAs introduced by coercion
## ℹ Run `dplyr::last_dplyr_warnings()` to see the 1 remaining warning.
```

``` r
my_predictions <- read_csv("my_predictions.csv")
```

```
## Rows: 7 Columns: 2
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): state
## dbl (1): my_prediction
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
my_predictions <- my_predictions|>
  left_join(state_pres_vote, by = c("state")) |>
  select(state, D2pv, my_prediction) |> 
  filter(state %in% c("Arizona", "North Carolina", "Nevada", "Pennsylvania", "Michigan", "Wisconsin", "Georgia"))

my_predictions
```

```
## # A tibble: 7 × 3
##   state           D2pv my_prediction
##   <chr>          <dbl>         <dbl>
## 1 Arizona        0.472          50.2
## 2 Georgia        0.489          50.0
## 3 Michigan       0.493          49.0
## 4 Nevada         0.484          49.7
## 5 North Carolina 0.483          49.3
## 6 Pennsylvania   0.490          48.9
## 7 Wisconsin      0.495          48.6
```

``` r
Mse <- mse(my_predictions$D2pv, my_predictions$my_prediction)
Mse
```

```
## [1] 2393.423
```

``` r
Rmse <- rmse(my_predictions$D2pv, my_predictions$my_prediction)
Rmse 
```

```
## [1] 48.92262
```

``` r
Mae <- mae(my_predictions$D2pv, my_predictions$my_prediction)
Mae
```

```
## [1] 48.91922
```

``` r
Bias <- bias(my_predictions$D2pv, my_predictions$my_prediction)
Bias
```

```
## [1] -48.91922
```

