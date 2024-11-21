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

Below is a heat map with the error metrics visualized. The bias in my model did not disproportionately overestimate the Democratic two-party vote share. In four states: Arizona, Georgia, Nevada, and North Carolina it did overestimate the Democratic two party vote share. However, my model did overestimate Trump's winning margin in Michigan, Pennsylvania, and Wisconsin although the bias for Trump was less on average. I found this interesting because the states were I overestimated Trump's winning margin were also the ones that were hypothesized by political pundits to go for Harris. Michigan and Wisconsin provided hope for Harris because they used to make up the blue wall. I knew immediately that my model was predicting Arizona wrong because polling data showed that out of the seven battleground states, Arizona was one of the least likely states to go for Harris.

To investigate why there is no direction in bias for my model I will go back to my OLS regression results and explore which of my variables might have affected my model. 

``` r
state_level_analysis_long <- state_level_analysis |>
  pivot_longer(cols = c(mse, rmse, mae, bias), names_to = "Metric", values_to = "Value")

#https://r-charts.com/correlation/heat-map-ggplot2/
   
ggplot(state_level_analysis_long, aes(state, Metric, fill = Value)) +
  geom_tile() +
  geom_text(aes(label = round(Value, 3)), color = "black") +
  scale_fill_gradient(low = "darkseagreen1", high = "darkseagreen4") +
  labs(
    title = "State-Level Analysis of Metrics",
    x = "State",
    y = "Metric",
    fill = "Value"
  ) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />
I created a confusion matrix. The confusion matrix shows that I did not predict Harris would win any state that Trump actually won. It is easier to understand the errors because Trump had a clean sweep of the battleground states and my model only predicted he would win 5 which means that the error comes from predicting a Harris win in Arizona and Georgia where Trump won. 

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

I used a combination of demographics, polling data, and economic indicators to predict the electoral college results and the democratic two-party vote share in the battleground states with a OLS regression. One of the things that immediately stands out to me is that I may have made my model too complex. Although my model did not do a terrible job predicting the outcome of the election, I recognize from a statistical perspective that there were too many variables and some of them are not independent of one another which led to multicollinearity. For example, I have race and education level variables which are likely correlated in my linear regression. Additionally, I had a high r-squared value, around the high nineties which suggests that my model may be overfitting. 

In terms of the components of my model, I knew including demographic variables was a risk because the Kim & Zilinsky article we read a couple of weeks ago claims that five key demographic attributes can predict a voter's choice about 68% of the time. I only use three of those key attributes so it may be even lower for my model in terms of its predicting capacity. Post-election exit polls show that Trump made gains with most demographic groups which indicates that demographic attributes may not have great predicting capacity in this election. To test this out I removed race variables from my model and this is how the update model performed below. 


``` r
updates <- read_csv("updates.csv")
```

```
## Rows: 7 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): state
## dbl (2): update 1, update 2
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
update_one <- my_predictions |>
  left_join(updates,  by = c("state"))

update_one
```

```
## # A tibble: 7 × 7
##   state           D2pv my_prediction Winner pred_Winner `update 1` `update 2`
##   <chr>          <dbl>         <dbl> <fct>  <fct>            <dbl>      <dbl>
## 1 Arizona         47.2          50.2 Trump  Harris            48.6       46.7
## 2 Georgia         48.9          50.0 Trump  Harris            48.7       49.6
## 3 Michigan        49.3          49.0 Trump  Trump             49.2       49.2
## 4 Nevada          48.4          49.7 Trump  Trump             47.6       48.7
## 5 North Carolina  48.3          49.3 Trump  Trump             47.9       47.9
## 6 Pennsylvania    49.0          48.9 Trump  Trump             48.3       49.6
## 7 Wisconsin       49.5          48.6 Trump  Trump             48.3       49.6
```

``` r
Mse1 <- mse(update_one$D2pv, update_one$'update 1')
Mse1
```

```
## [1] 0.6708597
```

``` r
Rmse1 <- rmse(update_one$D2pv, update_one$'update 1')
Rmse1 
```

```
## [1] 0.8190603
```

``` r
Mae1 <- mae(update_one$D2pv, update_one$'update 1')
Mae1
```

```
## [1] 0.686095
```

``` r
Bias1 <- bias(update_one$D2pv, update_one$'update 1')
Bias1
```

```
## [1] 0.2835597
```

``` r
accuracy1 <- data.frame(
  metric = c("MSE", "RMSE", "MAE", "Bias"),
  value = c(Mse1, Rmse1, Mae1, Bias1)
)

kable(accuracy1)
```



|metric |     value|
|:------|---------:|
|MSE    | 0.6708597|
|RMSE   | 0.8190603|
|MAE    | 0.6860950|
|Bias   | 0.2835597|

``` r
state_level_analysis1 <- update_one |>
  group_by(state) |> 
  mutate(
    mse = mse(D2pv, `update 1`),
    rmse = rmse(D2pv, `update 1`),
    mae = mae(D2pv, `update 1`),
    bias = bias(D2pv, `update 1`)
  )

kable(state_level_analysis1)
```



|state          |     D2pv| my_prediction|Winner |pred_Winner | update 1| update 2|       mse|      rmse|       mae|       bias|
|:--------------|--------:|-------------:|:------|:-----------|--------:|--------:|---------:|---------:|---------:|----------:|
|Arizona        | 47.15253|      50.24998|Trump  |Harris      | 48.56140| 46.71684| 1.9849244| 1.4088735| 1.4088735| -1.4088735|
|Georgia        | 48.87845|      50.04388|Trump  |Harris      | 48.66703| 49.58014| 0.0446983| 0.2114197| 0.2114197|  0.2114197|
|Michigan       | 49.30156|      48.95665|Trump  |Trump       | 49.15558| 49.22085| 0.0213088| 0.1459752| 0.1459752|  0.1459752|
|Nevada         | 48.37829|      49.74023|Trump  |Trump       | 47.59146| 48.67112| 0.6190976| 0.7868276| 0.7868276|  0.7868276|
|North Carolina | 48.29889|      49.29332|Trump  |Trump       | 47.90145| 47.94593| 0.1579621| 0.3974445| 0.3974445|  0.3974445|
|Pennsylvania   | 48.98235|      48.92587|Trump  |Trump       | 48.33273| 49.55108| 0.4220015| 0.6496165| 0.6496165|  0.6496165|
|Wisconsin      | 49.53156|      48.62988|Trump  |Trump       | 48.32905| 49.61006| 1.4460254| 1.2025079| 1.2025079|  1.2025079|

``` r
state_level_analysis_long1 <- state_level_analysis1 |>
  pivot_longer(cols = c(mse, rmse, mae, bias), names_to = "Metric", values_to = "Value")

ggplot(state_level_analysis_long1, aes(state, Metric, fill = Value)) +
  geom_tile() +
  geom_text(aes(label = round(Value, 3)), color = "black") +
  scale_fill_gradient(low = "darkseagreen1", high = "darkseagreen4") +
  labs(
    title = "State-Level Analysis of Metrics",
    x = "State",
    y = "Metric",
    fill = "Value"
  ) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/no race vars-1.png" width="672" />
After removing the race variables from my model, it does perform better, accurately predicting a Trump victory in all battleground states, but has started to underestimate the Democratic two-party vote share in six out of the seven battleground states which I thought was interesting. The model once again overestimates the Harris margin which makes me wonder what about the variables and Arizona is so unique that it is not being captured by the variables in my model. I predict it has to do with the quality of polls in Arizona potentially overestimating Harris support in Arizona. The graphic below shows that Arizona substantially shifted to the right. 


``` r
# Read 2024 results datasets. 
d_state_2024 <- read_csv("state_votes_pres_2024.csv")[-1, 1:6]
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
d_county_2024 <- read_csv("county_votes_pres_2024.csv")[-1, 1:6]
```

```
## New names:
## Rows: 3193 Columns: 42
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
d_county_2020 <- read_csv("county_votes_pres_2020.csv")[-1, 1:6]
```

```
## Rows: 3156 Columns: 42
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (42): FIPS, Geographic Name, Geographic Subtype, Total Vote, Joseph R. B...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
# Process 2024 state and county-level data. 
d_state_2024 <- d_state_2024 |> 
  mutate(FIPS = as.numeric(FIPS), 
         votes_trump = as.numeric(`Donald J. Trump`), 
         votes_harris = as.numeric(`Kamala D. Harris`), 
         votes = as.numeric(`Total Vote`), 
         trump_pv = votes_trump/votes, 
         harris_pv = votes_harris/votes, 
         trump_2pv = votes_trump/(votes_trump + votes_harris), 
         harris_2pv = votes_harris/(votes_trump + votes_harris)) |> 
  mutate(winner = case_when(votes_trump > votes_harris ~ "REP", 
                            .default = "DEM")) |> 
  select(FIPS, `Geographic Name`, `Geographic Subtype`, votes_trump, votes_harris, votes, 
         winner, trump_pv, harris_pv, trump_2pv, harris_2pv)

d_county_2024 <- d_county_2024 |>
  mutate(FIPS = as.numeric(FIPS),
         votes_trump = as.numeric(`Donald J. Trump`), 
         votes_harris = as.numeric(`Kamala D. Harris`), 
         votes = as.numeric(`Total Vote`), 
         trump_pv = votes_trump/votes, 
         harris_pv = votes_harris/votes, 
         trump_2pv = votes_trump/(votes_trump + votes_harris), 
         harris_2pv = votes_harris/(votes_trump + votes_harris)) |> 
  mutate(winner = case_when(votes_trump > votes_harris ~ "REP", 
                            .default = "DEM")) |> 
  select(FIPS, `Geographic Name`, `Geographic Subtype`, votes_trump, votes_harris, votes, 
         winner, trump_pv, harris_pv, trump_2pv, harris_2pv)

d_county_2020 <- d_county_2020 |> 
  mutate(FIPS = as.numeric(FIPS),
         votes_trump_2020 = as.numeric(`Donald J. Trump`), 
         votes_biden_2020 = as.numeric(`Joseph R. Biden Jr.`), 
         votes_2020 = as.numeric(`Total Vote`), 
         trump_pv_2020 = votes_trump_2020/votes_2020, 
         biden_pv_2020 = votes_biden_2020/votes_2020, 
         trump_2pv_2020 = votes_trump_2020/(votes_trump_2020 + votes_biden_2020), 
         biden_2pv_2020 = votes_biden_2020/(votes_trump_2020 + votes_biden_2020)) |> 
  mutate(winner_2020 = case_when(votes_trump_2020 > votes_biden_2020 ~ "REP", 
                            .default = "DEM")) |> 
  select(FIPS, `Geographic Name`, `Geographic Subtype`, votes_trump_2020, votes_biden_2020, votes_2020, 
         winner_2020, trump_pv_2020, biden_pv_2020, trump_2pv_2020, biden_2pv_2020)
counties_2024 <- counties(cb = TRUE, resolution = "5m", year = 2023) |> 
  shift_geometry() |> 
  mutate(GEOID = as.numeric(GEOID)) |> 
  left_join(d_county_2024, by = c("GEOID" = "FIPS")) |> 
  left_join(d_county_2020, by = c("GEOID" = "FIPS")) |>
  mutate(shift = (trump_pv - trump_pv_2020) * 100, 
         shift_dir = case_when(shift > 0 ~ "REP", 
                               shift < 0 ~ "DEM", 
                               TRUE ~ "No Change"),
         centroid = st_centroid(geometry), 
         centroid_long = st_coordinates(centroid)[,1],
         centroid_lat = st_coordinates(centroid)[,2],
         scale_factor = 1e4, 
         end_long = centroid_long + scale_factor * shift,
         end_lat = centroid_lat + scale_factor * shift) |>
  drop_na()
```

```
## 
  |                                                                            
  |                                                                      |   0%
  |                                                                            
  |                                                                      |   1%
  |                                                                            
  |=                                                                     |   1%
  |                                                                            
  |=                                                                     |   2%
  |                                                                            
  |==                                                                    |   2%
  |                                                                            
  |==                                                                    |   3%
  |                                                                            
  |===                                                                   |   4%
  |                                                                            
  |===                                                                   |   5%
  |                                                                            
  |====                                                                  |   5%
  |                                                                            
  |====                                                                  |   6%
  |                                                                            
  |=====                                                                 |   7%
  |                                                                            
  |=====                                                                 |   8%
  |                                                                            
  |======                                                                |   8%
  |                                                                            
  |======                                                                |   9%
  |                                                                            
  |=======                                                               |  10%
  |                                                                            
  |========                                                              |  11%
  |                                                                            
  |========                                                              |  12%
  |                                                                            
  |=========                                                             |  13%
  |                                                                            
  |==========                                                            |  14%
  |                                                                            
  |==========                                                            |  15%
  |                                                                            
  |===========                                                           |  15%
  |                                                                            
  |===========                                                           |  16%
  |                                                                            
  |============                                                          |  17%
  |                                                                            
  |=============                                                         |  18%
  |                                                                            
  |=============                                                         |  19%
  |                                                                            
  |==============                                                        |  20%
  |                                                                            
  |===============                                                       |  21%
  |                                                                            
  |===============                                                       |  22%
  |                                                                            
  |================                                                      |  22%
  |                                                                            
  |================                                                      |  23%
  |                                                                            
  |=================                                                     |  24%
  |                                                                            
  |=================                                                     |  25%
  |                                                                            
  |==================                                                    |  25%
  |                                                                            
  |==================                                                    |  26%
  |                                                                            
  |===================                                                   |  27%
  |                                                                            
  |===================                                                   |  28%
  |                                                                            
  |====================                                                  |  28%
  |                                                                            
  |====================                                                  |  29%
  |                                                                            
  |=====================                                                 |  29%
  |                                                                            
  |=====================                                                 |  30%
  |                                                                            
  |=====================                                                 |  31%
  |                                                                            
  |======================                                                |  31%
  |                                                                            
  |======================                                                |  32%
  |                                                                            
  |=======================                                               |  32%
  |                                                                            
  |=======================                                               |  33%
  |                                                                            
  |========================                                              |  34%
  |                                                                            
  |=========================                                             |  35%
  |                                                                            
  |=========================                                             |  36%
  |                                                                            
  |==========================                                            |  37%
  |                                                                            
  |==========================                                            |  38%
  |                                                                            
  |===========================                                           |  38%
  |                                                                            
  |===========================                                           |  39%
  |                                                                            
  |============================                                          |  39%
  |                                                                            
  |============================                                          |  40%
  |                                                                            
  |=============================                                         |  41%
  |                                                                            
  |=============================                                         |  42%
  |                                                                            
  |==============================                                        |  43%
  |                                                                            
  |===============================                                       |  44%
  |                                                                            
  |===============================                                       |  45%
  |                                                                            
  |================================                                      |  45%
  |                                                                            
  |================================                                      |  46%
  |                                                                            
  |=================================                                     |  47%
  |                                                                            
  |==================================                                    |  48%
  |                                                                            
  |==================================                                    |  49%
  |                                                                            
  |===================================                                   |  50%
  |                                                                            
  |====================================                                  |  51%
  |                                                                            
  |====================================                                  |  52%
  |                                                                            
  |=====================================                                 |  52%
  |                                                                            
  |=====================================                                 |  53%
  |                                                                            
  |======================================                                |  54%
  |                                                                            
  |======================================                                |  55%
  |                                                                            
  |=======================================                               |  55%
  |                                                                            
  |=======================================                               |  56%
  |                                                                            
  |========================================                              |  57%
  |                                                                            
  |=========================================                             |  58%
  |                                                                            
  |=========================================                             |  59%
  |                                                                            
  |==========================================                            |  60%
  |                                                                            
  |==========================================                            |  61%
  |                                                                            
  |===========================================                           |  61%
  |                                                                            
  |===========================================                           |  62%
  |                                                                            
  |============================================                          |  62%
  |                                                                            
  |============================================                          |  63%
  |                                                                            
  |=============================================                         |  64%
  |                                                                            
  |=============================================                         |  65%
  |                                                                            
  |==============================================                        |  65%
  |                                                                            
  |==============================================                        |  66%
  |                                                                            
  |===============================================                       |  67%
  |                                                                            
  |===============================================                       |  68%
  |                                                                            
  |================================================                      |  68%
  |                                                                            
  |================================================                      |  69%
  |                                                                            
  |=================================================                     |  69%
  |                                                                            
  |=================================================                     |  70%
  |                                                                            
  |==================================================                    |  71%
  |                                                                            
  |==================================================                    |  72%
  |                                                                            
  |===================================================                   |  73%
  |                                                                            
  |====================================================                  |  74%
  |                                                                            
  |====================================================                  |  75%
  |                                                                            
  |=====================================================                 |  75%
  |                                                                            
  |=====================================================                 |  76%
  |                                                                            
  |======================================================                |  76%
  |                                                                            
  |======================================================                |  77%
  |                                                                            
  |======================================================                |  78%
  |                                                                            
  |=======================================================               |  78%
  |                                                                            
  |=======================================================               |  79%
  |                                                                            
  |========================================================              |  80%
  |                                                                            
  |=========================================================             |  81%
  |                                                                            
  |=========================================================             |  82%
  |                                                                            
  |==========================================================            |  82%
  |                                                                            
  |==========================================================            |  83%
  |                                                                            
  |==========================================================            |  84%
  |                                                                            
  |===========================================================           |  84%
  |                                                                            
  |===========================================================           |  85%
  |                                                                            
  |============================================================          |  85%
  |                                                                            
  |============================================================          |  86%
  |                                                                            
  |=============================================================         |  87%
  |                                                                            
  |==============================================================        |  88%
  |                                                                            
  |==============================================================        |  89%
  |                                                                            
  |===============================================================       |  90%
  |                                                                            
  |===============================================================       |  91%
  |                                                                            
  |================================================================      |  91%
  |                                                                            
  |================================================================      |  92%
  |                                                                            
  |=================================================================     |  92%
  |                                                                            
  |=================================================================     |  93%
  |                                                                            
  |==================================================================    |  94%
  |                                                                            
  |==================================================================    |  95%
  |                                                                            
  |===================================================================   |  96%
  |                                                                            
  |====================================================================  |  97%
  |                                                                            
  |====================================================================  |  98%
  |                                                                            
  |===================================================================== |  98%
  |                                                                            
  |===================================================================== |  99%
  |                                                                            
  |======================================================================|  99%
  |                                                                            
  |======================================================================| 100%
```

``` r
county_pop_2024 <- read_csv("PopulationEstimates.csv") |> 
  mutate(FIPStxt = as.numeric(FIPStxt)) |>
  select(FIPStxt, POP_ESTIMATE_2023)
```

```
## Rows: 3283 Columns: 68
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (3): FIPStxt, State, Area_Name
## dbl (24): Rural_Urban_Continuum_Code_2013, Rural_Urban_Continuum_Code_2023, ...
## num (41): CENSUS_2020_POP, ESTIMATES_BASE_2020, POP_ESTIMATE_2020, POP_ESTIM...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
counties_2024 <- counties_2024 |> 
  left_join(county_pop_2024, by = c("GEOID" = "FIPStxt"))
counties_2024 |> 
  filter(STATE_NAME == "Arizona") |> 
  ggplot() +
  geom_sf(fill = "gray95", color = "darkgrey") +  # Base map
  geom_text(aes(x = centroid_long, y = centroid_lat-1.5e4, label = NAME),
            size = 2,  # Adjust size as needed
            color = "black", hjust = 0.5, vjust = -0.5) + 
  geom_curve(aes(x = centroid_long, 
                 y = centroid_lat,
                 xend = end_long, 
                 yend = end_lat,
                 color = shift_dir),
             arrow = arrow(length = unit(0.1, "cm"), type = "closed"),  # Smaller arrowhead
             curvature = 0.2,  # Add a slight curve to each arrow
             size = 0.3) +
  scale_color_manual(values = c("DEM" = "blue", "REP" = "red")) +
  theme_void() +
  labs(title = "Presidential Voting Shifts by County in Arizona",
       subtitle = "Democratic vs. Republican Gains")
```

```
## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
## ℹ Please use `linewidth` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />
I also looked back at my original OLS regression model and decided to select only the variables that were statistically significant. Interestingly, it was all of my race variables and the variable containing the latest polling data. I ran the same model again with these variables and this was the outcome (shown below).


``` r
updates <- read_csv("updates.csv")
```

```
## Rows: 7 Columns: 3
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): state
## dbl (2): update 1, update 2
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
update_one <- my_predictions |>
  left_join(updates,  by = c("state"))

update_one
```

```
## # A tibble: 7 × 7
##   state           D2pv my_prediction Winner pred_Winner `update 1` `update 2`
##   <chr>          <dbl>         <dbl> <fct>  <fct>            <dbl>      <dbl>
## 1 Arizona         47.2          50.2 Trump  Harris            48.6       46.7
## 2 Georgia         48.9          50.0 Trump  Harris            48.7       49.6
## 3 Michigan        49.3          49.0 Trump  Trump             49.2       49.2
## 4 Nevada          48.4          49.7 Trump  Trump             47.6       48.7
## 5 North Carolina  48.3          49.3 Trump  Trump             47.9       47.9
## 6 Pennsylvania    49.0          48.9 Trump  Trump             48.3       49.6
## 7 Wisconsin       49.5          48.6 Trump  Trump             48.3       49.6
```

``` r
Mse2 <- mse(update_one$D2pv, update_one$'update 2')
Mse2
```

```
## [1] 0.1755229
```

``` r
Rmse2 <- rmse(update_one$D2pv, update_one$'update 2')
Rmse2
```

```
## [1] 0.4189546
```

``` r
Mae2 <- mae(update_one$D2pv, update_one$'update 2')
Mae2
```

```
## [1] 0.3587306
```

``` r
Bias2 <- bias(update_one$D2pv, update_one$'update 2')
Bias2
```

```
## [1] -0.1103432
```

``` r
accuracy2 <- data.frame(
  metric = c("MSE", "RMSE", "MAE", "Bias"),
  value = c(Mse2, Rmse2, Mae2, Bias2)
)

kable(accuracy2)
```



|metric |      value|
|:------|----------:|
|MSE    |  0.1755229|
|RMSE   |  0.4189546|
|MAE    |  0.3587306|
|Bias   | -0.1103432|

``` r
state_level_analysis2 <- update_one |>
  group_by(state) |> 
  mutate(
    mse = mse(D2pv, `update 2`),
    rmse = rmse(D2pv, `update 2`),
    mae = mae(D2pv, `update 2`),
    bias = bias(D2pv, `update 2`)
  )

kable(state_level_analysis2)
```



|state          |     D2pv| my_prediction|Winner |pred_Winner | update 1| update 2|       mse|      rmse|       mae|       bias|
|:--------------|--------:|-------------:|:------|:-----------|--------:|--------:|---------:|---------:|---------:|----------:|
|Arizona        | 47.15253|      50.24998|Trump  |Harris      | 48.56140| 46.71684| 0.1898228| 0.4356865| 0.4356865|  0.4356865|
|Georgia        | 48.87845|      50.04388|Trump  |Harris      | 48.66703| 49.58014| 0.4923693| 0.7016903| 0.7016903| -0.7016903|
|Michigan       | 49.30156|      48.95665|Trump  |Trump       | 49.15558| 49.22085| 0.0065133| 0.0807052| 0.0807052|  0.0807052|
|Nevada         | 48.37829|      49.74023|Trump  |Trump       | 47.59146| 48.67112| 0.0857508| 0.2928324| 0.2928324| -0.2928324|
|North Carolina | 48.29889|      49.29332|Trump  |Trump       | 47.90145| 47.94593| 0.1245839| 0.3529645| 0.3529645|  0.3529645|
|Pennsylvania   | 48.98235|      48.92587|Trump  |Trump       | 48.33273| 49.55108| 0.3234578| 0.5687335| 0.5687335| -0.5687335|
|Wisconsin      | 49.53156|      48.62988|Trump  |Trump       | 48.32905| 49.61006| 0.0061626| 0.0785021| 0.0785021| -0.0785021|

``` r
state_level_analysis_long2 <- state_level_analysis2 |>
  pivot_longer(cols = c(mse, rmse, mae, bias), names_to = "Metric", values_to = "Value")

ggplot(state_level_analysis_long2, aes(state, Metric, fill = Value)) +
  geom_tile() +
  geom_text(aes(label = round(Value, 3)), color = "black") +
  scale_fill_gradient(low = "darkseagreen1", high = "darkseagreen4") +
  labs(
    title = "State-Level Analysis of Metrics",
    x = "State",
    y = "Metric",
    fill = "Value"
  ) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />
When I only include the variables that were statistically significant, the model performs better than my original better and the one updates without race variables. The aggregate level MSE value is 0.17 which is close to 0, indicating very good accuracy. I think this is really interesting because the Kim & Zilinsky article did not show the demographic attributes had high predicting capacity. This latest iteration of my model shows that polling data and race variables predict this election outcome very well. 

What does this say about the Q2 GDP growth? This was my only economic variable in my model. I did consider including unemployment but in the end opted to exclude because it was showing multicollinearity with Q2 GDP growth. When I added Q2 GDP growth to both of the updated models it increased the Democratic two-party vote share to the extent where it flipped states. 

### Where To Go From Here

In a 








++ interaction variables



