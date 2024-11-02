---
title: "Blog Post 7"
author: "Kelly Olmos"
date: '2024-10-21'
slug: "blog-post-7"
categories: []
tags: []
---



``` r
#' @title GOV 1347: Week 7 (Ground Game) Laboratory Session
#' @author Matthew E. Dardet
#' @date October 15, 2024

####----------------------------------------------------------#
#### Preamble
####----------------------------------------------------------#

# Load libraries.
## install via `install.packages("name")`
library(geofacet)
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
library(broom)
library(knitr)

## set working directory here
# setwd("~")
```


``` r
####----------------------------------------------------------#
#### Read, merge, and process data.
####----------------------------------------------------------#

# Read popular vote datasets. 
d_popvote <- read_csv("popvote_1948_2020.csv")
```

```
## Rows: 40 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): party, candidate
## dbl (5): year, pv, pv2p, deminc, juneapp
## lgl (4): winner, incumbent, incumbent_party, prev_admin
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
d_state_popvote <- read_csv("state_popvote_1948_2020.csv")
```

```
## Rows: 959 Columns: 18
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): state
## dbl (17): year, D_pv, R_pv, D_pv2p, R_pv2p, votes_D, votes_R, total_votes, t...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
d_state_popvote[d_state_popvote$state == "District of Columbia",]$state <- "District Of Columbia"

# Read elector distribution dataset. 
d_ec <- read_csv("corrected_ec_1948_2024.csv")
```

```
## Rows: 1010 Columns: 4
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (2): state, stateab
## dbl (2): year, electors
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
# Read polling data. 
d_polls <- read_csv("national_polls_1968-2024.csv")
```

```
## Rows: 7436 Columns: 9
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

``` r
d_state_polls <- read_csv("state_polls_1968-2024.csv")
```

```
## Rows: 205678 Columns: 9
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

``` r
# Read turnout data. 
d_turnout <- read_csv("state_turnout_1980_2022.csv")
```

```
## Rows: 1144 Columns: 15
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (5): state, vep_turnout, vep_highest_office, vap_highest_office, noncitizen
## dbl (1): year
## num (9): total_ballots, highest_office_ballots, vep, vap, prison, probation,...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
# Read county turnout. 
d_county_turnout <- read_csv("county_turnout.csv")
```

```
## New names:
## Rows: 21848 Columns: 12
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (5): state, state_po, county_name, candidate, party dbl (7): ...1, year,
## county_fips, candidatevotes, totalvotes, POPESTIMATE202...
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```

``` r
# Read state-level demographics.
d_state_demog <- read_csv("demographics.csv")
```

```
## New names:
## Rows: 663 Columns: 44
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (1): state dbl (43): ...1, year, total_pop, white, black, american_indian,
## asian_pacifi...
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```

``` r
# Read county demographics. 
d_county_demog <- read_csv("county_demographics.csv")
```

```
## New names:
## Rows: 3223 Columns: 45
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (2): COUNTY, STATE dbl (43): ...1, year, total_pop, white, black,
## american_indian, asian_pacifi...
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```

``` r
# Read campaign events datasets. 
d_campaign_events <- read_csv("campaigns_2016_2024.csv")[,-1]
```

```
## New names:
## Rows: 902 Columns: 7
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (4): state, city, candidate, Event.Type dbl (2): ...1, year date (1): date
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```



``` r
####----------------------------------------------------------#
#### Binomial simulations for election prediction. 
####----------------------------------------------------------#


# Merge popular vote and polling data. 
d <- d_state_popvote |> 
  inner_join(d_state_polls |> filter(weeks_left == 3)) |> 
  mutate(state_abb = state.abb[match(state, state.name)])
```

```
## Joining with `by = join_by(year, state)`
```

``` r
# Generate state-specific univariate poll-based forecasts with linear model.
state_forecast <- list()
state_forecast_outputs <- data.frame()
```


``` r
#linear regressions for each state, predicting Dem party vote share based on polling data
for (s in unique(d$state_abb)) {
  # Democrat model.
  state_forecast[[s]]$dat_D <- d |> filter(state_abb == s, party == "DEM")
  state_forecast[[s]]$mod_D <- lm(D_pv ~ poll_support, 
                                  state_forecast[[s]]$dat_D)
  
  # Republican model.
  state_forecast[[s]]$dat_R <- d |> filter(state_abb == s, party == "REP")
  state_forecast[[s]]$mod_R <- lm(R_pv ~ poll_support, 
                                  state_forecast[[s]]$dat_R)
  
  if (nrow(state_forecast[[s]]$dat_R) > 2) {
    # Save state-level model estimates. 
    state_forecast_outputs <- rbind(state_forecast_outputs, 
                                    rbind(cbind.data.frame(
                                      intercept = summary(state_forecast[[s]]$mod_D)$coefficients[1,1], 
                                      intercept_se = summary(state_forecast[[s]]$mod_D)$coefficients[1,2],
                                      slope = summary(state_forecast[[s]]$mod_D)$coefficients[2,1], 
                                      state_abb = s, 
                                      party = "DEM"), 
                                    rbind(cbind.data.frame(
                                     intercept = summary(state_forecast[[s]]$mod_R)$coefficients[1,1],
                                     intercept_se = summary(state_forecast[[s]]$mod_R)$coefficients[1,2],
                                     slope = summary(state_forecast[[s]]$mod_R)$coefficients[2,1],
                                     state_abb = s,
                                     party = "REP"
                                    ))))
  }
}
```


``` r
# Make graphs of polls in different states/parties at different levels of strength/significance of outcome. 
state_forecast_trends <- state_forecast_outputs |> 
  mutate(`0` = intercept, 
         `25` = intercept + slope*25, 
         `50` = intercept + slope*50, 
         `75` = intercept + slope*75, 
         `100` = intercept + slope*100) |>
  select(-intercept, -slope) |> 
  gather(x, y, -party, -state_abb, -intercept_se) |> 
  mutate(x = as.numeric(x))

ggplot(state_forecast_trends, aes(x=x, y=y, ymin=y-intercept_se, ymax=y+intercept_se)) + 
  facet_geo(~ state_abb) +
  geom_line(aes(color = party)) + 
  geom_ribbon(aes(fill = party), alpha=0.5, color=NA) +
  coord_cartesian(ylim=c(0, 100)) +
  scale_color_manual(values = c("blue", "red")) +
  scale_fill_manual(values = c("blue", "red")) +
  xlab("Hypothetical Poll Support") +
  ylab("Predicted Voteshare\n(pv = A + B * poll)") +
  ggtitle("") +
  theme_bw()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

``` r
# Q: What's wrong with this map? 
# A: (1.) no polls in some states
#    (2.) very high variance for some states (Nevada)/negative slopes for others (Mississippi)
#    (3.) y is not always in the [0, 100] range
```


``` r
#probabilistic state forecast w/ binomial lm 
state_forecast_trends |>
  filter(state_abb == "CA" | state_abb == "FL")|>
  ggplot(aes(x=x, y=y, ymin=y-intercept_se, ymax=y+intercept_se)) + 
  facet_wrap(~ state_abb) +
  geom_line(aes(color = party)) + 
  geom_hline(yintercept = 100, lty = 3) +
  geom_hline(yintercept = 0, lty = 3) + 
  geom_ribbon(aes(fill = party), alpha=0.5, color=NA) +
  ## N.B. You can, in fact, combine *different* data and aesthetics
  ##       in one ggplot; but this usually needs to come at the end 
  ##       and you must explicitly override all previous aesthetics
  geom_text(data = d |> filter(state_abb == "CA", party=="DEM"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  geom_text(data = d |> filter(state_abb == "CA", party=="REP"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  geom_text(data = d |> filter(state_abb == "FL", party=="DEM"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  geom_text(data = d |> filter(state_abb == "FL", party=="REP"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  scale_color_manual(values = c("blue", "red")) +
  scale_fill_manual(values = c("blue", "red")) +
  xlab("Hypothetical Poll Support") +
  ylab("Predicted Voteshare\n(pv = A + B * poll)") +
  theme_bw()
```



``` r
# Merge turnout data into main dataset. 
d <- d |> 
  left_join(d_turnout, by = c("state", "year")) |> 
  filter(year >= 1980) # Filter to when turnout dataset begins. 

# Generate probabilistic univariate poll-based state forecasts. 
state_glm_forecast <- list()
state_glm_forecast_outputs <- data.frame()
for (s in unique(d$state_abb)) {
  # Democrat model. 
  state_glm_forecast[[s]]$dat_D <- d |> filter(state_abb == s, party == "DEM")
  state_glm_forecast[[s]]$mod_D <- glm(cbind(votes_D, vep - votes_D) ~ poll_support, # Cbind(N Success, N Total) for Binomial Model 
                                      state_glm_forecast[[s]]$dat_D, 
                                      family = binomial(link = "logit"))
  
  # Republican model. 
  state_glm_forecast[[s]]$dat_R <- d |> filter(state_abb == s, party == "REP")
  state_glm_forecast[[s]]$mod_R <- glm(cbind(votes_R, vep - votes_R) ~ poll_support, 
                                      state_glm_forecast[[s]]$dat_R, 
                                      family = binomial(link = "logit"))
  
  if (nrow(state_glm_forecast[[s]]$dat_R) > 2) {
    for (hypo_avg_poll in seq(from = 0, to = 100, by = 10)) { 
      # Democrat prediction. 
      D_pred_vote_prob <- predict(state_glm_forecast[[s]]$mod_D, 
                                  newdata = data.frame(poll_support = hypo_avg_poll), se = TRUE, type = "response")
      D_pred_qt <- qt(0.975, df = df.residual(state_glm_forecast[[s]]$mod_D)) # Used in the prediction interval formula. 
      
      # Republican prediction. 
      R_pred_vote_prob <- predict(state_glm_forecast[[s]]$mod_R, 
                                  newdata = data.frame(poll_support = hypo_avg_poll), se = TRUE, type = "response")
      R_pred_qt <- qt(0.975, df = df.residual(state_glm_forecast[[s]]$mod_R)) # Used in the prediction interval formula.
      
      # Save predictions. 
      state_glm_forecast_outputs <- rbind(state_glm_forecast_outputs, 
                                          cbind.data.frame(x = hypo_avg_poll,
                                                           y = D_pred_vote_prob$fit*100,
                                                           ymin = (D_pred_vote_prob$fit - D_pred_qt*D_pred_vote_prob$se.fit)*100,
                                                           ymax = (D_pred_vote_prob$fit + D_pred_qt*D_pred_vote_prob$se.fit)*100,
                                                           state_abb = s, 
                                                           party = "DEM"),
                                          cbind.data.frame(x = hypo_avg_poll,
                                                           y = R_pred_vote_prob$fit*100,
                                                           ymin = (R_pred_vote_prob$fit - R_pred_qt*R_pred_vote_prob$se.fit)*100,
                                                           ymax = (R_pred_vote_prob$fit + R_pred_qt*R_pred_vote_prob$se.fit)*100,
                                                           state_abb = s, 
                                                           party = "REP"))
    }
  }
}

# Make graphs of polls in different states/parties at different levels of strength/significance of outcome. 
ggplot(state_glm_forecast_outputs, aes(x=x, y=y, ymin=ymin, ymax=ymax)) + 
  facet_geo(~ state_abb) +
  geom_line(aes(color = party)) + 
  geom_ribbon(aes(fill = party), alpha=0.5, color=NA) +
  coord_cartesian(ylim=c(0, 100)) +
  scale_color_manual(values = c("blue", "red")) +
  scale_fill_manual(values = c("blue", "red")) +
  xlab("Hypothetical Poll Support") +
  ylab('Probability of State-Eligible Voter Voting for Party') +
  theme_bw()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

``` r
state_glm_forecast_outputs |>
  filter(state_abb == "CA" | state_abb == "FL") |>
  ggplot(aes(x=x, y=y, ymin=ymin, ymax=ymax)) + 
  facet_wrap(~ state_abb) +
  geom_line(aes(color = party)) + 
  geom_ribbon(aes(fill = party), alpha=0.5, color=NA) +
  coord_cartesian(ylim=c(0, 100)) +
  geom_text(data = d |> filter(state_abb == "CA", party=="DEM"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  geom_text(data = d |> filter(state_abb == "CA", party=="REP"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  geom_text(data = d |> filter(state_abb == "FL", party=="DEM"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  geom_text(data = d |> filter(state_abb == "FL", party=="REP"), 
            aes(x = poll_support, y = D_pv, ymin = D_pv, ymax = D_pv, color = party, label = year), size=1.5) +
  scale_color_manual(values = c("blue", "red")) +
  scale_fill_manual(values = c("blue", "red")) +
  xlab("Hypothetical Poll Support") +
  ylab('Probability of\nState-Eligible Voter\nVoting for Party') +
  ggtitle("Binomial Logit") + 
  theme_bw() + 
  theme(axis.title.y = element_text(size=6.5))
```

```
## Warning in geom_text(data = filter(d, state_abb == "CA", party == "DEM"), :
## Ignoring unknown aesthetics: ymin and ymax
```

```
## Warning in geom_text(data = filter(d, state_abb == "CA", party == "REP"), :
## Ignoring unknown aesthetics: ymin and ymax
```

```
## Warning in geom_text(data = filter(d, state_abb == "FL", party == "DEM"), :
## Ignoring unknown aesthetics: ymin and ymax
```

```
## Warning in geom_text(data = filter(d, state_abb == "FL", party == "REP"), :
## Ignoring unknown aesthetics: ymin and ymax
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-2.png" width="672" />


``` r
# Simulating a distribution of potential election results in Pennsylvania for 2024. 
# First step. Let's use GAM (general additive model) to impute VEP in Pennsylvania for 2024 using historical VEP.

# Get historical eligible voting population in Pennsylvania. 
vep_PA_2020 <- as.integer(d_turnout$vep[d_turnout$state == "Pennsylvania" & d_turnout$year == 2020])
vep_PA <- d_turnout |> filter(state == "Pennsylvania") |> select(vep, year)

# Fit regression for 2024 VEP prediction. 
lm_vep_PA <- lm(vep ~ year, vep_PA)

plot(x = vep_PA$year, y = vep_PA$vep, xlab = "Year", ylab = "VEP", main = "Voting Eligible Population in Pennsylvania by Year")
abline(lm_vep_PA, col = "red")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

``` r
vep_PA_2024_ols <- predict(lm_vep_PA, newdata = data.frame(year = 2024)) |> as.numeric()

gam_vep_PA <- mgcv::gam(vep ~ s(year), data = vep_PA)
print(plot(getViz(gam_vep_PA)) + l_points() + l_fitLine(linetype = 3) + l_ciLine(colour = 2) + theme_get()) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-2.png" width="672" />

``` r
# Use generalized additive model (GAM) to predict 2024 VEP in Pennsylvania.
vep_PA_2024_gam <- predict(gam_vep_PA, newdata = data.frame(year = 2024)) |> as.numeric()

# Take weighted average of linear and GAM predictions for final prediction. 
vep_PA_2024 <- as.integer(0.75*vep_PA_2024_gam + 0.25*vep_PA_2024_ols)
vep_PA_2024
```

```
## [1] 10044706
```

``` r
# Split datasets by party. 
PA_D <- d |> filter(state == "Pennsylvania" & party == "DEM")
PA_R <- d |> filter(state == "Pennsylvania" & party == "REP")

# Fit Democrat and Republican models. 
PA_D_glm <- glm(cbind(votes_R, vep - votes_R) ~ poll_support, data = PA_D, family = binomial(link = "logit"))
PA_R_glm <- glm(cbind(votes_R, vep - votes_R) ~ poll_support, data = PA_R, family = binomial(link = "logit"))

# Get predicted draw probabilities for D and R. 
(PA_pollav_D <- d_state_polls$poll_support[d_state_polls$state == "Pennsylvania" & d_state_polls$weeks_left == 3 & d_state_polls$party == "DEM"] |> mean(na.rm = T))
```

```
## [1] 45.27291
```

``` r
(PA_pollav_R <- d_state_polls$poll_support[d_state_polls$state == "Pennsylvania" & d_state_polls$weeks_left == 3 & d_state_polls$party == "REP"] |> mean(na.rm = T))
```

```
## [1] 42.14185
```

``` r
(PA_sdpoll_D <- sd(d_state_polls$poll_support[d_state_polls$state == "Pennsylvania" & d_state_polls$weeks_left == 3 & d_state_polls$party == "DEM"] |> na.omit()))
```

```
## [1] 5.302223
```

``` r
(PA_sdpoll_R <- sd(d_state_polls$poll_support[d_state_polls$state == "Pennsylvania" & d_state_polls$weeks_left == 3 & d_state_polls$party == "REP"] |> na.omit()))
```

```
## [1] 5.069872
```

``` r
(prob_D_vote_PA_2024 <- predict(PA_D_glm, newdata = data.frame(poll_support = PA_pollav_D), se = TRUE, type = "response")[[1]] |> as.numeric())
```

```
## [1] 0.2706284
```

``` r
(prob_R_vote_PA_2024 <- predict(PA_R_glm, newdata = data.frame(poll_support = PA_pollav_R), se = TRUE, type = "response")[[1]] |> as.numeric())
```

```
## [1] 0.2716045
```


``` r
# Get predicted distribution of draws from the population. 
sim_D_votes_PA_2024 <- rbinom(n = 10000, size = vep_PA_2024, prob = prob_D_vote_PA_2024)
sim_R_votes_PA_2024 <- rbinom(n = 10000, size = vep_PA_2024, prob = prob_R_vote_PA_2024)

# Simulating a distribution of election results: Harris PA PV. 
hist(sim_D_votes_PA_2024, breaks = 100, col = "blue", main = "Predicted Turnout Draws for Harris \n from 10,000 Binomial Process Simulations")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

``` r
# Simulating a distribution of election results: Trump PA PV. 
hist(sim_R_votes_PA_2024, breaks = 100, col = "red", main = "Predicted Turnout Draws for Trump \n from 10,000 Binomial Process Simulations")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-2.png" width="672" />

``` r
# Simulating a distribution of election results: Trump win margin. 
sim_elxns_PA_2024 <- ((sim_R_votes_PA_2024-sim_D_votes_PA_2024)/(sim_D_votes_PA_2024 + sim_R_votes_PA_2024))*100
hist(sim_elxns_PA_2024, breaks = 100, col = "firebrick1", main = "Predicted Draws of Win Margin for Trump \n from 10,000 Binomial Process Simulations", xlim = c(0, 0.4))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-3.png" width="672" />

``` r
# Simulations incorporating prior for SD. 
sim_D_votes_PA_2024_2 <- rbinom(n = 10000, size = vep_PA_2024, prob = rnorm(10000, PA_pollav_D/100, PA_sdpoll_D/100))
sim_R_votes_PA_2024_2 <- rbinom(n = 10000, size = vep_PA_2024, prob = rnorm(10000, PA_pollav_R/100, PA_sdpoll_R/100))


sim_elxns_PA_2024_2 <- ((sim_R_votes_PA_2024_2-sim_D_votes_PA_2024_2)/(sim_D_votes_PA_2024_2 + sim_R_votes_PA_2024_2))*100
h <- hist(sim_elxns_PA_2024_2, breaks = 100, col = "firebrick1")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-4.png" width="672" />

``` r
cuts <- cut(h$breaks, c(-Inf, 0, Inf))
plot(h, yaxt = "n", bty = "n", xlab = "", ylab = "", main = "", xlim = c(-35, 35), col = c("blue", "red")[cuts], cex.axis=0.8)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-5.png" width="672" />


``` r
####----------------------------------------------------------#
#### Ground Game: Field offices and campaign events. 
####----------------------------------------------------------#

# Where should campaigns build field offices? 
fo_2012 <- read_csv("fieldoffice_2012_bycounty.csv")
```

```
## Rows: 3113 Columns: 16
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): state
## dbl (11): fips, obama12fo, romney12fo, normal, medage08, pop2008, medinc08, ...
## lgl  (4): battle, swing, core_dem, core_rep
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
lm_obama <- lm(obama12fo ~ romney12fo + 
                 swing + 
                 core_rep + 
                 swing:romney12fo + 
                 core_rep:romney12fo + 
                 battle + 
                 medage08 + 
                 pop2008 + 
                 pop2008^2 + 
                 medinc08 + 
                 black + 
                 hispanic + 
                 pc_less_hs00 + 
                 pc_degree00 + 
                 as.factor(state), 
               fo_2012)

lm_romney <- lm(romney12fo ~ 
                  obama12fo + 
                  swing + 
                  core_dem + 
                  swing:obama12fo + 
                  core_dem:obama12fo + 
                  battle + 
                  medage08 + 
                  pop2008 + 
                  pop2008^2 + 
                  medinc08 + 
                  black + 
                  hispanic + 
                  pc_less_hs00 + 
                  pc_degree00 + 
                  as.factor(state),
                  fo_2012)

stargazer(lm_obama, lm_romney, header=FALSE, type='latex', no.space = TRUE,
          column.sep.width = "3pt", font.size = "scriptsize", single.row = TRUE,
          keep = c(1:7, 62:66), omit.table.layout = "sn",
          title = "Placement of Field Offices (2012)")
```

```
## 
## \begin{table}[!htbp] \centering 
##   \caption{Placement of Field Offices (2012)} 
##   \label{} 
## \scriptsize 
## \begin{tabular}{@{\extracolsep{3pt}}lcc} 
## \\[-1.8ex]\hline 
## \hline \\[-1.8ex] 
##  & \multicolumn{2}{c}{\textit{Dependent variable:}} \\ 
## \cline{2-3} 
## \\[-1.8ex] & obama12fo & romney12fo \\ 
## \\[-1.8ex] & (1) & (2)\\ 
## \hline \\[-1.8ex] 
##  romney12fo & 2.546$^{***}$ (0.114) &  \\ 
##   obama12fo &  & 0.374$^{***}$ (0.020) \\ 
##   swing & 0.001 (0.055) & $-$0.012 (0.011) \\ 
##   core\_rep & 0.007 (0.061) &  \\ 
##   core\_dem &  & 0.004 (0.027) \\ 
##   battle & 0.541$^{***}$ (0.096) & 0.014 (0.042) \\ 
##   medage08 &  &  \\ 
##   romney12fo:swing & $-$0.765$^{***}$ (0.116) &  \\ 
##   romney12fo:core\_rep & $-$1.875$^{***}$ (0.131) &  \\ 
##   obama12fo:swing &  & $-$0.081$^{***}$ (0.020) \\ 
##   obama12fo:core\_dem &  & $-$0.164$^{***}$ (0.023) \\ 
##   Constant & $-$0.340$^{*}$ (0.196) & 0.001 (0.079) \\ 
##  \hline \\[-1.8ex] 
## \hline 
## \hline \\[-1.8ex] 
## \end{tabular} 
## \end{table}
```

``` r
# Effects of field offices on turnout and vote share. 
fo_dem <- read_csv("fieldoffice_2004-2012_dems.csv")
```

```
## Rows: 9339 Columns: 18
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): candidate, state
## dbl (15): year, fips, number_fo, dummy_fo, population, totalvote, demvote, d...
## lgl  (1): battle
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
ef_t <- lm(turnout_change ~ dummy_fo_change + battle + dummy_fo_change:battle + as.factor(state) + as.factor(year), fo_dem)

ef_d <- lm(dempct_change ~ dummy_fo_change + battle + dummy_fo_change:battle + as.factor(state) + as.factor(year), fo_dem)

stargazer(ef_t, ef_d, header=FALSE, type='latex', no.space = TRUE,
          column.sep.width = "3pt", font.size = "scriptsize", single.row = TRUE,
          keep = c(1:3, 53:54), keep.stat = c("n", "adj.rsq", "res.dev"),
          title = "Effect of DEM Field Offices on Turnout and DEM Vote Share (2004-2012)")
```

```
## 
## \begin{table}[!htbp] \centering 
##   \caption{Effect of DEM Field Offices on Turnout and DEM Vote Share (2004-2012)} 
##   \label{} 
## \scriptsize 
## \begin{tabular}{@{\extracolsep{3pt}}lcc} 
## \\[-1.8ex]\hline 
## \hline \\[-1.8ex] 
##  & \multicolumn{2}{c}{\textit{Dependent variable:}} \\ 
## \cline{2-3} 
## \\[-1.8ex] & turnout\_change & dempct\_change \\ 
## \\[-1.8ex] & (1) & (2)\\ 
## \hline \\[-1.8ex] 
##  dummy\_fo\_change & 0.004$^{***}$ (0.001) & 0.009$^{***}$ (0.002) \\ 
##   battle & 0.024$^{***}$ (0.002) & 0.043$^{***}$ (0.003) \\ 
##   as.factor(state)Arizona &  &  \\ 
##   dummy\_fo\_change:battle & $-$0.002 (0.002) & 0.007$^{**}$ (0.003) \\ 
##   Constant & 0.029$^{***}$ (0.002) & 0.022$^{***}$ (0.003) \\ 
##  \hline \\[-1.8ex] 
## Observations & 6,224 & 6,224 \\ 
## Adjusted R$^{2}$ & 0.419 & 0.469 \\ 
## \hline 
## \hline \\[-1.8ex] 
## \textit{Note:}  & \multicolumn{2}{r}{$^{*}$p$<$0.1; $^{**}$p$<$0.05; $^{***}$p$<$0.01} \\ 
## \end{tabular} 
## \end{table}
```

``` r
# Field Strategies of Obama, Romney, Clinton, and Trump in 2016. 
fo_add <- read_csv("fieldoffice_2012-2016_byaddress.csv")
```

```
## Rows: 1777 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (5): party, candidate, state, city, full_address
## dbl (3): year, latitude, longitude
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
# Visualizing campaign events. 
d_campaign_events$party[d_campaign_events$candidate %in% c("Trump / Pence", "Trump", "Pence", "Trump/Pence", "Vance")] <- "REP"
```

```
## Warning: Unknown or uninitialised column: `party`.
```

``` r
d_campaign_events$party[d_campaign_events$candidate %in% c("Biden / Harris", "Biden", "Harris", "Biden/Harris", "Walz", "Kaine", "Clinton", "Clinton / Kaine")] <- "DEM"
p.ev.1 <- d_campaign_events |> group_by(date, party) |> summarize(n_events = n(), year) |> filter(year == 2016) |> ggplot(aes(x = date, y = n_events, color = party)) + geom_point() + geom_smooth() + ggtitle("2016") + theme_bw()
```

```
## Warning: Returning more (or less) than 1 row per `summarise()` group was deprecated in
## dplyr 1.1.0.
## ℹ Please use `reframe()` instead.
## ℹ When switching from `summarise()` to `reframe()`, remember that `reframe()`
##   always returns an ungrouped data frame and adjust accordingly.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## `summarise()` has grouped output by 'date', 'party'. You can override using the
## `.groups` argument.
```

``` r
p.ev.2 <- d_campaign_events |> group_by(date, party) |> summarize(n_events = n(), year) |> filter(year == 2020) |> ggplot(aes(x = date, y = n_events, color = party)) + geom_point() + geom_smooth() + ggtitle("2020") +  theme_bw()
```

```
## Warning: Returning more (or less) than 1 row per `summarise()` group was deprecated in
## dplyr 1.1.0.
## ℹ Please use `reframe()` instead.
## ℹ When switching from `summarise()` to `reframe()`, remember that `reframe()`
##   always returns an ungrouped data frame and adjust accordingly.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## `summarise()` has grouped output by 'date', 'party'. You can override using the
## `.groups` argument.
```

``` r
p.ev.3 <- d_campaign_events |> group_by(date, party) |> summarize(n_events = n(), year) |> filter(year == 2024) |> ggplot(aes(x = date, y = n_events, color = party)) + geom_point() + geom_smooth() + ggtitle("2024") + theme_bw()
```

```
## Warning: Returning more (or less) than 1 row per `summarise()` group was deprecated in
## dplyr 1.1.0.
## ℹ Please use `reframe()` instead.
## ℹ When switching from `summarise()` to `reframe()`, remember that `reframe()`
##   always returns an ungrouped data frame and adjust accordingly.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## `summarise()` has grouped output by 'date', 'party'. You can override using the
## `.groups` argument.
```

``` r
ggarrange(p.ev.1, p.ev.2, p.ev.3)
```

```
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-10-1.png" width="672" />

``` r
# Can the number of campaign events predict state-level vote share? 
d_ev_state <- d_campaign_events |> 
  group_by(year, state, party) |> 
  summarize(n_events = n()) |> 
  pivot_wider(names_from = party, values_from = n_events) |> 
  rename(n_ev_D = DEM, n_ev_R = REP)
```

```
## `summarise()` has grouped output by 'year', 'state'. You can override using the
## `.groups` argument.
```

``` r
d_ev_state$n_ev_D[which(is.na(d_ev_state$n_ev_D))] <- 0
d_ev_state$n_ev_R[which(is.na(d_ev_state$n_ev_R))] <- 0
d_ev_state$ev_diff_D_R <- d_ev_state$n_ev_D - d_ev_state$n_ev_R
d_ev_state$ev_diff_R_D <- d_ev_state$n_ev_R - d_ev_state$n_ev_D

d <- d |> 
  left_join(d_ev_state, by = c("year", "state_abb" = "state"))

lm_ev_D <- lm(D_pv2p ~ n_ev_D + ev_diff_D_R, data = d)
lm_ev_R <- lm(R_pv2p ~ n_ev_R + ev_diff_R_D, data = d)

summary(lm_ev_D)
```

```
## 
## Call:
## lm(formula = D_pv2p ~ n_ev_D + ev_diff_D_R, data = d)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -17.6200  -4.7648   0.3904   5.2448  16.4092 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 48.18936    0.36910 130.559  < 2e-16 ***
## n_ev_D       0.12588    0.03351   3.757 0.000186 ***
## ev_diff_D_R  0.10453    0.06726   1.554 0.120618    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 7.508 on 711 degrees of freedom
##   (6408 observations deleted due to missingness)
## Multiple R-squared:  0.02141,	Adjusted R-squared:  0.01866 
## F-statistic: 7.778 on 2 and 711 DF,  p-value: 0.0004554
```

``` r
summary(lm_ev_R)
```

```
## 
## Call:
## lm(formula = R_pv2p ~ n_ev_R + ev_diff_R_D, data = d)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -16.4088  -5.2449  -0.3903   4.7647  17.6204 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 51.81023    0.36913 140.359  < 2e-16 ***
## n_ev_R      -0.12586    0.03351  -3.756 0.000187 ***
## ev_diff_R_D  0.23042    0.07796   2.956 0.003222 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 7.509 on 711 degrees of freedom
##   (6408 observations deleted due to missingness)
## Multiple R-squared:  0.02141,	Adjusted R-squared:  0.01865 
## F-statistic: 7.776 on 2 and 711 DF,  p-value: 0.0004564
```

``` r
ev_D_results <- tidy(lm_ev_D, conf.int = TRUE)
ev_R_results <- tidy(lm_ev_R, conf.int = TRUE)

ev_D_results <- ev_D_results |> 
  mutate(vote_share = "D_pv2p")
ev_R_results <- ev_R_results |> 
  mutate(vote_share = "R_pv2p")

all_results <- bind_rows(ev_D_results, ev_R_results) |>
  select(term, estimate, p.value, conf.low, conf.high, vote_share )

# Display table using kable
kable(all_results, digits = 3, caption = "Association Between Campaign Events and Voting Outcomes of Interest",
      col.names = c("Term", "Estimate", "p-value", "CI Lower", "CI Upper", "Party Vote Share"))
```



Table: <span id="tab:unnamed-chunk-10"></span>Table 1: Association Between Campaign Events and Voting Outcomes of Interest

|Term        | Estimate| p-value| CI Lower| CI Upper|Party Vote Share |
|:-----------|--------:|-------:|--------:|--------:|:----------------|
|(Intercept) |   48.189|   0.000|   47.465|   48.914|D_pv2p           |
|n_ev_D      |    0.126|   0.000|    0.060|    0.192|D_pv2p           |
|ev_diff_D_R |    0.105|   0.121|   -0.028|    0.237|D_pv2p           |
|(Intercept) |   51.810|   0.000|   51.086|   52.535|R_pv2p           |
|n_ev_R      |   -0.126|   0.000|   -0.192|   -0.060|R_pv2p           |
|ev_diff_R_D |    0.230|   0.003|    0.077|    0.383|R_pv2p           |


``` r
#This table will only have state_gdp data for q2 of each year 
state_gdp <- read_csv("state_gdp.csv") |>
  rename(state = GeoName) |>
  pivot_longer(
    cols = starts_with("20"),
    names_to = c("year", "quarter"), 
    names_sep = ":Q",
    values_to = "gdp",
    ) |>
  filter(quarter == "2") |>
  select(state, year, gdp)
```

```
## Warning: One or more parsing issues, call `problems()` on your data frame for details,
## e.g.:
##   dat <- vroom(...)
##   problems(dat)
```

```
## Rows: 54 Columns: 28
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (2): GeoFips, GeoName
## dbl (26): 2018:Q1, 2018:Q2, 2018:Q3, 2018:Q4, 2019:Q1, 2019:Q2, 2019:Q3, 201...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
state_gdp$year <- as.numeric(state_gdp$year)

state_gdp
```

```
## # A tibble: 378 × 3
##    state    year     gdp
##    <chr>   <dbl>   <dbl>
##  1 Alabama  2018 220360.
##  2 Alabama  2019 224229.
##  3 Alabama  2020 208569.
##  4 Alabama  2021 233098.
##  5 Alabama  2022 236710.
##  6 Alabama  2023 243977 
##  7 Alabama  2024 250740.
##  8 Alaska   2018  52478.
##  9 Alaska   2019  51864.
## 10 Alaska   2020  48242.
## # ℹ 368 more rows
```


``` r
personal_dis_income <- read_csv("personalIncomeState.csv", skip = 3)
```

```
## Warning: One or more parsing issues, call `problems()` on your data frame for details,
## e.g.:
##   dat <- vroom(...)
##   problems(dat)
```

```
## Rows: 66 Columns: 78
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (14): GeoFips, GeoName, 1948, 1949, 1950, 1951, 1952, 1953, 1954, 1955, ...
## dbl (64): 1960, 1961, 1962, 1963, 1964, 1965, 1966, 1967, 1968, 1969, 1970, ...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
regions_to_remove <- c(
  "United States ", "Far West ", "New England", "Mideast", "Great Lakes",
  "Plains", "Southeast", "Southwest", "Rocky Mountain", "District of Columbia"
)

#cleaning the data and processing it
personal_dis_income <- personal_dis_income |>
  rename(state = GeoName) |>
  mutate(across(-state, ~ as.numeric(gsub("\\(NA\\)", "", .)))) |>
  pivot_longer(
    cols = -c(state, GeoFips),  
    names_to = "year", 
    values_to = "disposable_income" 
  ) |>
  mutate(year = as.numeric(year)) |>
  mutate(state = gsub("\\*", "", state)) |>
  filter(year >= 2008) |>
  filter(!is.na(state) & !(state %in% regions_to_remove))
```

```
## Warning: There was 1 warning in `mutate()`.
## ℹ In argument: `across(-state, ~as.numeric(gsub("\\(NA\\)", "", .)))`.
## Caused by warning:
## ! NAs introduced by coercion
```

``` r
personal_dis_income
```

```
## # A tibble: 800 × 4
##    GeoFips state    year disposable_income
##      <dbl> <chr>   <dbl>             <dbl>
##  1    1000 Alabama  2008             30060
##  2    1000 Alabama  2009             30104
##  3    1000 Alabama  2010             31091
##  4    1000 Alabama  2011             31760
##  5    1000 Alabama  2012             32500
##  6    1000 Alabama  2013             32399
##  7    1000 Alabama  2014             33351
##  8    1000 Alabama  2015             34538
##  9    1000 Alabama  2016             34919
## 10    1000 Alabama  2017             36173
## # ℹ 790 more rows
```


``` r
d_polls <- read_csv("national_polls_1968-2024.csv")
```

```
## Rows: 7436 Columns: 9
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

``` r
d_state_polls <- read_csv("state_polls_1968-2024.csv")
```

```
## Rows: 205678 Columns: 9
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

``` r
# Process state-level polling data.
d_pollav_state <- d_state_polls |>
  group_by(year, state, party) |>
  mutate(mean_pollav = mean(poll_support, na.rm = TRUE)) |>
  top_n(1, poll_date) |>
  rename(latest_pollav = poll_support) |>
  select(-c(weeks_left, days_left, poll_date, candidate, before_convention)) |>
  pivot_wider(names_from = party, values_from = c(latest_pollav, mean_pollav))

# Merge data.
d <- d_pollav_state |>
  left_join(d_state_popvote, by = c("year", "state")) |>
  left_join(d_popvote |> filter(party == "democrat"), by = "year") |>
  left_join(d_turnout, by = c("year", "state")) |>
  left_join(personal_dis_income, by = c("state", "year")) |>
  left_join(state_gdp, by = c("state", "year")) |>
  filter(year >= 1980) |>
  ungroup()

d
```

```
## # A tibble: 557 × 48
##     year state               latest_pollav_REP latest_pollav_DEM mean_pollav_REP
##    <dbl> <chr>                           <dbl>             <dbl>           <dbl>
##  1  2016 Alabama                         56.3               35.1           66.3 
##  2  2016 Alaska                          45.2               39.1           46.8 
##  3  2016 Arizona                         45.6               42.9           41.4 
##  4  2016 Arkansas                        53.0               33.5           49.5 
##  5  2016 California                      32.8               54.9           31.7 
##  6  2016 Colorado                        40.8               43.6           41.4 
##  7  2016 Connecticut                     36.5               51.1           38.5 
##  8  2016 Delaware                        36.1               50.1           33.9 
##  9  2016 District of Columb…              6.50              85.6            5.00
## 10  2016 Florida                         45.5               46.4           42.6 
## # ℹ 547 more rows
## # ℹ 43 more variables: mean_pollav_DEM <dbl>, D_pv <dbl>, R_pv <dbl>,
## #   D_pv2p <dbl>, R_pv2p <dbl>, votes_D <dbl>, votes_R <dbl>,
## #   total_votes <dbl>, two_party_votes <dbl>, D_pv_lag1 <dbl>, R_pv_lag1 <dbl>,
## #   D_pv2p_lag1 <dbl>, R_pv2p_lag1 <dbl>, D_pv_lag2 <dbl>, R_pv_lag2 <dbl>,
## #   D_pv2p_lag2 <dbl>, R_pv2p_lag2 <dbl>, party <chr>, winner <lgl>,
## #   candidate <chr>, pv <dbl>, pv2p <dbl>, incumbent <lgl>, …
```

``` r
# Sequester states for which we have polling data for 2024.
states.2024 <- unique(d$state[d$year == 2024])
states.2024 <- states.2024[-which(states.2024 == "Nebraska Cd 2")]
d <- d |>
  filter(state %in% states.2024)

d
```

```
## # A tibble: 233 × 48
##     year state      latest_pollav_REP latest_pollav_DEM mean_pollav_REP
##    <dbl> <chr>                  <dbl>             <dbl>           <dbl>
##  1  2016 Arizona                 45.6              42.9            41.4
##  2  2016 California              32.8              54.9            31.7
##  3  2016 Florida                 45.5              46.4            42.6
##  4  2016 Georgia                 48.3              44.4            45.5
##  5  2016 Maryland                27.4              59.9            29.0
##  6  2016 Michigan                42.5              45.7            36.9
##  7  2016 Minnesota               38.1              47.0            39.0
##  8  2016 Missouri                50.3              38.2            44.1
##  9  2016 Montana                 50.7              32.7            54.0
## 10  2016 Nebraska                51.4              33.6            48.6
## # ℹ 223 more rows
## # ℹ 43 more variables: mean_pollav_DEM <dbl>, D_pv <dbl>, R_pv <dbl>,
## #   D_pv2p <dbl>, R_pv2p <dbl>, votes_D <dbl>, votes_R <dbl>,
## #   total_votes <dbl>, two_party_votes <dbl>, D_pv_lag1 <dbl>, R_pv_lag1 <dbl>,
## #   D_pv2p_lag1 <dbl>, R_pv2p_lag1 <dbl>, D_pv_lag2 <dbl>, R_pv_lag2 <dbl>,
## #   D_pv2p_lag2 <dbl>, R_pv2p_lag2 <dbl>, party <chr>, winner <lgl>,
## #   candidate <chr>, pv <dbl>, pv2p <dbl>, incumbent <lgl>, …
```

``` r
# Separate into training and testing for simple poll prediction model. 
d.train <- d |> filter(year < 2024) |> select(year, state, D_pv2p, latest_pollav_DEM, mean_pollav_DEM, D_pv2p_lag1, D_pv2p_lag2, disposable_income, gdp) |> drop_na()

names(d.train)
```

```
## [1] "year"              "state"             "D_pv2p"           
## [4] "latest_pollav_DEM" "mean_pollav_DEM"   "D_pv2p_lag1"      
## [7] "D_pv2p_lag2"       "disposable_income" "gdp"
```

``` r
d.test <- d |> filter(year == 2024) |> select(year, state, D_pv2p, latest_pollav_DEM, mean_pollav_DEM, D_pv2p_lag1, D_pv2p_lag2, disposable_income, gdp)

d.test
```

```
## # A tibble: 20 × 9
##     year state  D_pv2p latest_pollav_DEM mean_pollav_DEM D_pv2p_lag1 D_pv2p_lag2
##    <dbl> <chr>   <dbl>             <dbl>           <dbl>       <dbl>       <dbl>
##  1  2024 Arizo…     NA              46.8            41.5          NA          NA
##  2  2024 Calif…     NA              58.9            53.6          NA          NA
##  3  2024 Flori…     NA              45.2            39.9          NA          NA
##  4  2024 Georg…     NA              47.2            41.7          NA          NA
##  5  2024 Maryl…     NA              61.7            62.0          NA          NA
##  6  2024 Michi…     NA              47.8            42.9          NA          NA
##  7  2024 Minne…     NA              50.0            46.0          NA          NA
##  8  2024 Misso…     NA              41.6            41.6          NA          NA
##  9  2024 Monta…     NA              39.3            38.9          NA          NA
## 10  2024 Nebra…     NA              38.3            38.4          NA          NA
## 11  2024 Nevada     NA              47.7            40.5          NA          NA
## 12  2024 New H…     NA              51.1            50.9          NA          NA
## 13  2024 New M…     NA              50.2            50.1          NA          NA
## 14  2024 New Y…     NA              53.9            48.6          NA          NA
## 15  2024 North…     NA              47.3            41.2          NA          NA
## 16  2024 Ohio       NA              43.7            39.3          NA          NA
## 17  2024 Penns…     NA              48.1            43.3          NA          NA
## 18  2024 Texas      NA              44.3            38.6          NA          NA
## 19  2024 Virgi…     NA              50.4            48.3          NA          NA
## 20  2024 Wisco…     NA              48.0            43.2          NA          NA
## # ℹ 2 more variables: disposable_income <dbl>, gdp <dbl>
```

``` r
# Add back in lagged vote share for 2024. 
t <- d |> 
  filter(year >= 2016) |> 
  arrange(year) |> 
  group_by(state) |> 
  mutate(
    D_pv2p_lag1 = lag(D_pv2p, 1),
    R_pv2p_lag1 = lag(R_pv2p, 1), 
    D_pv2p_lag2 = lag(D_pv2p, 2),
    R_pv2p_lag2 = lag(R_pv2p, 2),
    disposable_income = lag(disposable_income, 1)) |> 
  filter(year == 2024) |> 
  select(state, year, D_pv2p, D_pv2p_lag1, R_pv2p_lag1, D_pv2p_lag2, R_pv2p_lag2, disposable_income, gdp) 

t
```

```
## # A tibble: 20 × 9
## # Groups:   state [20]
##    state           year D_pv2p D_pv2p_lag1 R_pv2p_lag1 D_pv2p_lag2 R_pv2p_lag2
##    <chr>          <dbl>  <dbl>       <dbl>       <dbl>       <dbl>       <dbl>
##  1 Arizona         2024     NA        50.2        49.8        48.1        51.9
##  2 California      2024     NA        64.9        35.1        66.1        33.9
##  3 Florida         2024     NA        48.3        51.7        49.4        50.6
##  4 Georgia         2024     NA        50.1        49.9        47.3        52.7
##  5 Maryland        2024     NA        67.0        33.0        64.0        36.0
##  6 Michigan        2024     NA        51.4        48.6        49.9        50.1
##  7 Minnesota       2024     NA        53.6        46.4        50.8        49.2
##  8 Missouri        2024     NA        42.2        57.8        40.2        59.8
##  9 Montana         2024     NA        41.6        58.4        38.9        61.1
## 10 Nebraska        2024     NA        40.2        59.8        36.5        63.5
## 11 Nevada          2024     NA        51.2        48.8        51.3        48.7
## 12 New Hampshire   2024     NA        53.7        46.3        50.2        49.8
## 13 New Mexico      2024     NA        55.5        44.5        54.7        45.3
## 14 New York        2024     NA        61.7        38.3        63.4        36.6
## 15 North Carolina  2024     NA        49.3        50.7        48.1        51.9
## 16 Ohio            2024     NA        45.9        54.1        45.7        54.3
## 17 Pennsylvania    2024     NA        50.6        49.4        49.6        50.4
## 18 Texas           2024     NA        47.2        52.8        45.3        54.7
## 19 Virginia        2024     NA        55.2        44.8        52.8        47.2
## 20 Wisconsin       2024     NA        50.3        49.7        49.6        50.4
## # ℹ 2 more variables: disposable_income <dbl>, gdp <dbl>
```

``` r
d.test <- d.test|>
  left_join(t, by = c("state", "year"), suffix = c("", ".t")) |>
  mutate(
    D_pv2p_lag1 = ifelse(is.na(D_pv2p_lag1), D_pv2p_lag1.t, D_pv2p_lag1),
    D_pv2p_lag2 = ifelse(is.na(D_pv2p_lag2), D_pv2p_lag2.t, D_pv2p_lag2),
    disposable_income = ifelse(is.na(disposable_income), disposable_income.t, disposable_income),
    D_pv2p = ifelse(is.na(D_pv2p), D_pv2p.t, D_pv2p), 
    gdp = ifelse(is.na(gdp), gdp, gdp)
  ) |>
  select(-ends_with(".t"))

d.test
```

```
## # A tibble: 20 × 11
##     year state  D_pv2p latest_pollav_DEM mean_pollav_DEM D_pv2p_lag1 D_pv2p_lag2
##    <dbl> <chr>   <dbl>             <dbl>           <dbl>       <dbl>       <dbl>
##  1  2024 Arizo…     NA              46.8            41.5        50.2        48.1
##  2  2024 Calif…     NA              58.9            53.6        64.9        66.1
##  3  2024 Flori…     NA              45.2            39.9        48.3        49.4
##  4  2024 Georg…     NA              47.2            41.7        50.1        47.3
##  5  2024 Maryl…     NA              61.7            62.0        67.0        64.0
##  6  2024 Michi…     NA              47.8            42.9        51.4        49.9
##  7  2024 Minne…     NA              50.0            46.0        53.6        50.8
##  8  2024 Misso…     NA              41.6            41.6        42.2        40.2
##  9  2024 Monta…     NA              39.3            38.9        41.6        38.9
## 10  2024 Nebra…     NA              38.3            38.4        40.2        36.5
## 11  2024 Nevada     NA              47.7            40.5        51.2        51.3
## 12  2024 New H…     NA              51.1            50.9        53.7        50.2
## 13  2024 New M…     NA              50.2            50.1        55.5        54.7
## 14  2024 New Y…     NA              53.9            48.6        61.7        63.4
## 15  2024 North…     NA              47.3            41.2        49.3        48.1
## 16  2024 Ohio       NA              43.7            39.3        45.9        45.7
## 17  2024 Penns…     NA              48.1            43.3        50.6        49.6
## 18  2024 Texas      NA              44.3            38.6        47.2        45.3
## 19  2024 Virgi…     NA              50.4            48.3        55.2        52.8
## 20  2024 Wisco…     NA              48.0            43.2        50.3        49.6
## # ℹ 4 more variables: disposable_income <dbl>, gdp <dbl>, R_pv2p_lag1 <dbl>,
## #   R_pv2p_lag2 <dbl>
```

``` r
# Standard frequentist linear regression. 
reg.ols <- lm(D_pv2p ~ latest_pollav_DEM + mean_pollav_DEM + D_pv2p_lag1 + D_pv2p_lag2 + disposable_income + gdp, data = d.train)

summary(reg.ols)
```

```
## 
## Call:
## lm(formula = D_pv2p ~ latest_pollav_DEM + mean_pollav_DEM + D_pv2p_lag1 + 
##     D_pv2p_lag2 + disposable_income + gdp, data = d.train)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -1.32065 -0.59382  0.05444  0.36398  1.07895 
## 
## Coefficients:
##                     Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       -4.694e+00  3.101e+00  -1.514  0.15395    
## latest_pollav_DEM  4.098e-01  1.908e-01   2.148  0.05114 .  
## mean_pollav_DEM    2.217e-01  1.472e-01   1.507  0.15582    
## D_pv2p_lag1        7.690e-01  1.739e-01   4.422  0.00069 ***
## D_pv2p_lag2       -3.192e-01  8.151e-02  -3.916  0.00177 ** 
## disposable_income  6.254e-05  6.223e-05   1.005  0.33328    
## gdp               -1.576e-06  4.276e-07  -3.686  0.00274 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.829 on 13 degrees of freedom
## Multiple R-squared:  0.9906,	Adjusted R-squared:  0.9862 
## F-statistic: 227.7 on 6 and 13 DF,  p-value: 2.135e-12
```

``` r
pred.ols.dem <- predict(reg.ols, newdata = d.test)
pred.ols.dem
```

```
##        1        2        3        4        5        6        7        8 
## 49.13361 58.62676 45.09022 49.11751 68.30337 50.09180 53.76306 43.53704 
##        9       10       11       12       13       14       15       16 
## 42.50767 41.77598 49.59802 56.49968 54.73233 56.23131 48.24814 44.48534 
##       17       18       19       20 
## 49.75267 43.57579 54.60861 49.97932
```

``` r
# Create dataset to summarize winners and EC vote distributions. 
win_pred <- data.frame(state = d.test$state,
                       year = rep(2024, length(d.test$state)),
                       simp_pred_dem = pred.ols.dem,
                       simp_pred_rep = 100 - pred.ols.dem) |> 
            mutate(winner = ifelse(simp_pred_dem > simp_pred_rep, "Democrat", "Republican")) |>
            left_join(d_ec, by = c("state", "year"))

win_pred
```

```
##             state year simp_pred_dem simp_pred_rep     winner stateab electors
## 1         Arizona 2024      49.13361      50.86639 Republican      AZ       11
## 2      California 2024      58.62676      41.37324   Democrat      CA       54
## 3         Florida 2024      45.09022      54.90978 Republican      FL       30
## 4         Georgia 2024      49.11751      50.88249 Republican      GA       16
## 5        Maryland 2024      68.30337      31.69663   Democrat      MD       10
## 6        Michigan 2024      50.09180      49.90820   Democrat      MI       15
## 7       Minnesota 2024      53.76306      46.23694   Democrat      MN       10
## 8        Missouri 2024      43.53704      56.46296 Republican      MO       10
## 9         Montana 2024      42.50767      57.49233 Republican      MT        4
## 10       Nebraska 2024      41.77598      58.22402 Republican      NE        5
## 11         Nevada 2024      49.59802      50.40198 Republican      NV        6
## 12  New Hampshire 2024      56.49968      43.50032   Democrat      NH        4
## 13     New Mexico 2024      54.73233      45.26767   Democrat      NM        5
## 14       New York 2024      56.23131      43.76869   Democrat      NY       28
## 15 North Carolina 2024      48.24814      51.75186 Republican      NC       16
## 16           Ohio 2024      44.48534      55.51466 Republican      OH       17
## 17   Pennsylvania 2024      49.75267      50.24733 Republican      PA       19
## 18          Texas 2024      43.57579      56.42421 Republican      TX       40
## 19       Virginia 2024      54.60861      45.39139   Democrat      VA       13
## 20      Wisconsin 2024      49.97932      50.02068 Republican      WI       10
```

``` r
win_pred |> 
  filter(winner == "Democrat") |> 
  select(state)
```

```
##           state
## 1    California
## 2      Maryland
## 3      Michigan
## 4     Minnesota
## 5 New Hampshire
## 6    New Mexico
## 7      New York
## 8      Virginia
```

``` r
win_pred |> 
  filter(winner == "Republican") |> 
  select(state)
```

```
##             state
## 1         Arizona
## 2         Florida
## 3         Georgia
## 4        Missouri
## 5         Montana
## 6        Nebraska
## 7          Nevada
## 8  North Carolina
## 9            Ohio
## 10   Pennsylvania
## 11          Texas
## 12      Wisconsin
```

``` r
win_pred |> 
  group_by(winner) |> 
  summarize(n = n(), ec = sum(electors))
```

```
## # A tibble: 2 × 3
##   winner         n    ec
##   <chr>      <int> <dbl>
## 1 Democrat       8   139
## 2 Republican    12   184
```

``` r
# Create data set to summarize winners and EC vote distributions. 
win_pred <- data.frame(state = d.test$state,
                       year = rep(2024, length(d.test$state)),
                       simp_pred_dem = pred.ols.dem,
                       simp_pred_rep = 100 - pred.ols.dem) |> 
            mutate(winner = ifelse(simp_pred_dem > simp_pred_rep, "Democrat", "Republican")) |> left_join(d_ec, by = c("state", "year"))


#### monte carlo simulations so i can create confidence intervals 
# Set the number of simulations
m <- 1e4  # 10,000 simulations

residual_se <- summary(reg.ols)$sigma

pred_dem <- predict(reg.ols, newdata = d.test)

n_states <- nrow(d.test)

pred.mat <- data.frame(
  state = rep(d.test$state, times = m),
  year = rep(2024, times = m * n_states),
  simp_pred_dem = numeric(m * n_states),
  simp_pred_rep = numeric(m * n_states)
)

j <- 1
for (i in 1:m) {
  if (i %% 1000 == 0) {
    print(paste("Simulation", i))
  }
    simulated_errors <- rnorm(n_states, mean = 0, sd = residual_se)
    pred_dem_sim <- pred_dem + simulated_errors
    pred_dem_sim <- pmin(pmax(pred_dem_sim, 0), 100)
    pred_rep_sim <- 100 - pred_dem_sim
  
  idx <- j:(i * n_states)
  pred.mat$simp_pred_dem[idx] <- pred_dem_sim
  pred.mat$simp_pred_rep[idx] <- pred_rep_sim
  
  j <- j + n_states
}
```

```
## [1] "Simulation 1000"
## [1] "Simulation 2000"
## [1] "Simulation 3000"
## [1] "Simulation 4000"
## [1] "Simulation 5000"
## [1] "Simulation 6000"
## [1] "Simulation 7000"
## [1] "Simulation 8000"
## [1] "Simulation 9000"
## [1] "Simulation 10000"
```

``` r
pred.mat <- pred.mat %>%
  mutate(
    winner = ifelse(simp_pred_dem > simp_pred_rep, "Democrat", "Republican")
  )
####


electoral_outcomes <- pred.mat |>
  group_by(state) |>
  summarize(
    mean_dem = mean(simp_pred_dem),
    mean_rep = mean(simp_pred_rep),
    sd_dem = sd(simp_pred_dem),
    sd_rep = sd(simp_pred_rep),
    lower_dem = mean_dem - 1.96 * sd_dem,
    upper_dem = mean_dem + 1.96 * sd_dem,
    lower_rep = mean_rep - 1.96 * sd_rep,
    upper_rep = mean_rep + 1.96 * sd_rep
  ) |>
  mutate(
    winner = ifelse(mean_dem > mean_rep, "Democrat", "Republican")
  )

view(electoral_outcomes)


#Since this only gives us the results for 19 states, it includes all swing states so a simple calculation with lagged vote will allow me to fill in the winner for the rest of the US
#list of all states 
all_states <- state.name

missing_states <- setdiff(all_states, unique(electoral_outcomes$state))
missing_states
```

```
##  [1] "Alabama"        "Alaska"         "Arkansas"       "Colorado"      
##  [5] "Connecticut"    "Delaware"       "Hawaii"         "Idaho"         
##  [9] "Illinois"       "Indiana"        "Iowa"           "Kansas"        
## [13] "Kentucky"       "Louisiana"      "Maine"          "Massachusetts" 
## [17] "Mississippi"    "New Jersey"     "North Dakota"   "Oklahoma"      
## [21] "Oregon"         "Rhode Island"   "South Carolina" "South Dakota"  
## [25] "Tennessee"      "Utah"           "Vermont"        "Washington"    
## [29] "West Virginia"  "Wyoming"
```

``` r
#created a new data frame for the missing states 
missing_data <- d_state_popvote |>
  filter(state %in% missing_states) |>
  filter(year == 2020) |>
  select(state, D_pv2p_lag1, R_pv2p_lag1)

#Create the winner column based on lagged vote shares. Since these are not swing states, using the outcome from the last election reflects whethere it is a blue/red state
missing_data <- missing_data|>
  mutate(
    winner = ifelse(D_pv2p_lag1 > R_pv2p_lag1, "Democrat", "Republican")
  )

ec_2024 <- d_ec |>
  filter(year == 2024) |> 
  select(state, electors)
  


#Combine datasets 
all_states_pred <- bind_rows(electoral_outcomes, missing_data) |>
  left_join(ec_2024, by = "state")

winner <- all_states_pred |>
  group_by(winner) |>
  summarize(total_electors = sum(electors))

winner
```

```
## # A tibble: 2 × 2
##   winner     total_electors
##   <chr>               <dbl>
## 1 Democrat              238
## 2 Republican            297
```

``` r
#DC not included so add 3 for Dem electoral college count
election_results <- tibble(
  party = c("Democrat", "Republican"),
  total_electors = c(winner$total_electors[1] + 3, winner$total_electors[2])
)

kable(election_results)
```



|party      | total_electors|
|:----------|--------------:|
|Democrat   |            241|
|Republican |            297|

``` r
plot_usmap(data = all_states_pred, regions = "states", values = "winner") + scale_fill_manual(
    values = c("Democrat" = "blue", "Republican" = "red"),
    name = "Predicted Winner"
  ) +
  labs(title = "Electoral College Predictions") 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="672" />


``` r
#how close is this race?

electoral_outcomes_close <- electoral_outcomes |>
  filter(state %in% c("Arizona", "Georgia", "Michigan", "Nevada", "North Carolina", "Pennsylvania", "Wisconsin")) |>
  select(state, mean_dem, mean_rep, lower_dem, upper_dem,lower_rep, upper_rep, winner)

kable(electoral_outcomes_close)
```



|state          | mean_dem| mean_rep| lower_dem| upper_dem| lower_rep| upper_rep|winner     |
|:--------------|--------:|--------:|---------:|---------:|---------:|---------:|:----------|
|Arizona        | 49.14538| 50.85462|  47.53204|  50.75872|  49.24128|  52.46796|Republican |
|Georgia        | 49.12712| 50.87288|  47.48810|  50.76615|  49.23385|  52.51190|Republican |
|Michigan       | 50.09396| 49.90604|  48.46863|  51.71928|  48.28072|  51.53137|Democrat   |
|Nevada         | 49.60372| 50.39628|  47.98223|  51.22522|  48.77478|  52.01777|Republican |
|North Carolina | 48.24440| 51.75560|  46.62913|  49.85967|  50.14033|  53.37087|Republican |
|Pennsylvania   | 49.75145| 50.24855|  48.12915|  51.37375|  48.62625|  51.87085|Republican |
|Wisconsin      | 49.98719| 50.01281|  48.35405|  51.62033|  48.37967|  51.64595|Republican |




