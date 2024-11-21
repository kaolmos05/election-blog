---
title: 'Post-Election Reflection '
author: Kelly Olmos
date: '2024-11-18'
slug: post-election-reflection
categories: []
tags: []
---


``` r
library(censable)
library(geofacet)
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
library(readstata13)
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
## ✖ dplyr::filter()     masks stats::filter()
## ✖ dplyr::group_rows() masks kableExtra::group_rows()
## ✖ dplyr::lag()        masks stats::lag()
## ✖ purrr::map()        masks maps::map()
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
library(geofacet)
library(glmnet)
```

```
## Loading required package: Matrix
## 
## Attaching package: 'Matrix'
## 
## The following objects are masked from 'package:tidyr':
## 
##     expand, pack, unpack
## 
## Loaded glmnet 4.1-8
```

``` r
library(usmap)
library(ggpubr)
library(ggthemes)
library(haven)
library(kableExtra)
library(maps)
library(mgcv)
library(mgcViz)
library(RColorBrewer)
library(scales)
library(sf)
library(spData)
library(stargazer)
library(tidygeocoder)
library(tidyverse)
library(tigris)
library(tmap)
library(tmaptools)
library(viridis)
library(caret)
```

```
## Loading required package: lattice
## 
## Attaching package: 'lattice'
## 
## The following object is masked from 'package:mgcViz':
## 
##     qq
## 
## 
## Attaching package: 'caret'
## 
## The following object is masked from 'package:purrr':
## 
##     lift
```

``` r
library(broom)
library(knitr)
library(dplyr)
library(Metrics)
```

```
## 
## Attaching package: 'Metrics'
## 
## The following objects are masked from 'package:caret':
## 
##     precision, recall
```

### My Model and its Predictions

I ran a state-level ordinary least squares regression. For the non-battleground states, I used the lagged vote shares to predict which candidate would win that particular state. I used polling data, fundamentals (Q2 gdp growth, and demographics (age, race, and education level) to predict which candidate would win in each state. Since there was not enough polling data for all states, I focused on the ones that did–battleground states. I ran an elastic net on my OLS regression model to deal with multicollinearity. Some variables like race and education level may be correlated. Additionally I ran 10,000 simulations to create 95% confidence intervals. 

The total list of predictors in my OLS regression is: latest_pollav_DEM, D_pv2p_lag1,  D_pv2p_lag2 +, q2_gdp_growth, white, black, american_indian, asian_pacific_islander, less_than_college, college, hispanic, age_18_to_29, age_30_to_64, age_65_75plus. 

My model predicted a Trump victory with 285 electoral college votes and 258 electoral college votes for Harris. My model inaccurately predicted that Georgia and Arizona would go for Harris. 



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
state_pres_vote <- state_pres_vote |>
  select(state = "Geographic Name", Trump = "Donald J. Trump", Harris = "Kamala D. Harris") |>
  mutate(
    Trump = as.numeric(Trump),
    Harris = as.numeric(Harris),
    D2pv = (Harris/(Trump + Harris))*100,
    Winner = as.factor(ifelse(D2pv > 50, "Harris", "Trump"))
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
  mutate(
    pred_Winner = as.factor(ifelse(my_prediction > 50, "Harris", "Trump"))
  ) |>
  left_join(state_pres_vote, by = c("state")) |>
  select(state, D2pv, my_prediction, Winner, pred_Winner) |> 
  filter(state %in% c("Arizona", "North Carolina", "Nevada", "Pennsylvania", "Michigan", "Wisconsin", "Georgia"))


kable(my_predictions)
```



|state          |     D2pv| my_prediction|Winner |pred_Winner |
|:--------------|--------:|-------------:|:------|:-----------|
|Arizona        | 47.15253|      50.24998|Trump  |Harris      |
|Georgia        | 48.87845|      50.04388|Trump  |Harris      |
|Michigan       | 49.30156|      48.95665|Trump  |Trump       |
|Nevada         | 48.37829|      49.74023|Trump  |Trump       |
|North Carolina | 48.29889|      49.29332|Trump  |Trump       |
|Pennsylvania   | 48.98235|      48.92587|Trump  |Trump       |
|Wisconsin      | 49.53156|      48.62988|Trump  |Trump       |
In the bar chart below, I have plotted my predicted Democratic two-party vote share compared to actual Democratic two-party vote share in the battleground states. 


``` r
bar_chart <- my_predictions |>
  select(state, D2pv, my_prediction) |>
  pivot_longer(cols = c(D2pv, my_prediction), 
               names_to = "type", 
               values_to = "D2pv_value")

ggplot(bar_chart, aes(x = state, y = D2pv_value, fill = type)) +
  geom_bar(stat = "identity", position = position_dodge(width = 0.8)) +
  scale_fill_manual(values = c("D2pv" = "darkgreen", "my_prediction" = "red"), 
                    labels = c("D2pv (%)", "Predicted D2pv (%)")) +
  labs(
    title = "Predicted vs Actual Democratic Two-Party Vote Share",
    x = "State",
    y = "D2pv",
    fill = "Key"
  ) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/bar chart-1.png" width="672" />

### Accuracy of my Model 

To quantify the accuracy of my model, I calculated the MSE, RMSE, MAE, and Bias. The findings are presented in the table below. 

The average squared difference between predicted and actual Democratic vote shares is 2.104.

The average error is approximately 1.45 percentage points.

The average absolute error between my predicted and actual Democratic vote share is 1.13 percentage points

My bias score indicates that I overestimated the Democratic party vote share by 0.76 points on average. 


``` r
Mse <- mse(my_predictions$D2pv, my_predictions$my_prediction)
Mse
```

```
## [1] 2.104484
```

``` r
Rmse <- rmse(my_predictions$D2pv, my_predictions$my_prediction)
Rmse 
```

```
## [1] 1.450684
```

``` r
Mae <- mae(my_predictions$D2pv, my_predictions$my_prediction)
Mae
```

```
## [1] 1.131759
```

``` r
Bias <- bias(my_predictions$D2pv, my_predictions$my_prediction)
Bias
```

```
## [1] -0.759456
```

``` r
accuracy <- data.frame(
  metric = c("MSE", "RMSE", "MAE", "Bias"),
  value = c(Mse, Rmse, Mae, Bias)
)
kable(accuracy)
```



|metric |     value|
|:------|---------:|
|MSE    |  2.104484|
|RMSE   |  1.450684|
|MAE    |  1.131759|
|Bias   | -0.759456|

I performed a state-level analysis to test how well my model worked in certain states. My model performed the worst in Arizona where I overestimated the Democratic party vote share by a little over three percentage points. The model performed the best in Pennsylvania where the MSE value was 0.003. 


``` r
state_level_analysis <- my_predictions |>
  group_by(state) |> 
  mutate(
    mse = mse(D2pv, my_prediction),
    rmse = rmse(D2pv, my_prediction),
    mae = mae(D2pv, my_prediction),
    bias = bias(D2pv, my_prediction)
  )

kable(state_level_analysis)
```



|state          |     D2pv| my_prediction|Winner |pred_Winner |       mse|      rmse|       mae|       bias|
|:--------------|--------:|-------------:|:------|:-----------|---------:|---------:|---------:|----------:|
|Arizona        | 47.15253|      50.24998|Trump  |Harris      | 9.5942180| 3.0974535| 3.0974535| -3.0974535|
|Georgia        | 48.87845|      50.04388|Trump  |Harris      | 1.3582278| 1.1654303| 1.1654303| -1.1654303|
|Michigan       | 49.30156|      48.95665|Trump  |Trump       | 0.1189596| 0.3449052| 0.3449052|  0.3449052|
|Nevada         | 48.37829|      49.74023|Trump  |Trump       | 1.8548872| 1.3619424| 1.3619424| -1.3619424|
|North Carolina | 48.29889|      49.29332|Trump  |Trump       | 0.9888821| 0.9944255| 0.9944255| -0.9944255|
|Pennsylvania   | 48.98235|      48.92587|Trump  |Trump       | 0.0031896| 0.0564765| 0.0564765|  0.0564765|
|Wisconsin      | 49.53156|      48.62988|Trump  |Trump       | 0.8130231| 0.9016779| 0.9016779|  0.9016779|

I create a confusion matrix. The confusion matrix shows that I did not predict Harris would win any state that Trump actually won. It is easier to understand the errors because Trump had a clean sweep of the battleground states and my model only predicted he would win 5 which means that the error comes from predicting a Harris win in Arizona and Georgia where Trump won. 

``` r
table("Actual" = my_predictions$Winner, 
      "Prediction" = my_predictions$pred_Winner)
```

```
##         Prediction
## Actual   Harris Trump
##   Harris      0     0
##   Trump       2     5
```

``` r
confusion_matrix <- confusionMatrix(data= my_predictions$Winner, reference = my_predictions$pred_Winner)
```
### Reflection


