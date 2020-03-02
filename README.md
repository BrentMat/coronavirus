
<!-- README.md is generated from README.Rmd. Please edit that file -->

# coronavirus

<!-- badges: start --->

[![build](https://github.com/RamiKrispin/coronavirus/workflows/build/badge.svg?branch=master)](https://github.com/RamiKrispin/coronavirus/actions?query=workflow%3Abuild)
[![CRAN\_Status\_Badge](https://www.r-pkg.org/badges/version/coronavirus)](https://cran.r-project.org/package=coronavirus)
[![lifecycle](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![License:
MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
<!-- badges: end -->

The coronavirus package provides a tidy format dataset of the 2019 Novel
Coronavirus COVID-19 (2019-nCoV) epidemic. The raw data pulled from the
Johns Hopkins University Center for Systems Science and Engineering (JHU
CCSE) Coronavirus
[repository](https://github.com/CSSEGISandData/COVID-19).

More details available
[here](https://ramikrispin.github.io/coronavirus/), and a `csv` format
of the package dataset available
[here](https://github.com/RamiKrispin/coronavirus-csv)

A summary dashboard is available
[here](https://ramikrispin.github.io/coronavirus_dashboard/)

<img src="man/figures/2019-nCoV-CDC-23312_without_background.png" width="65%" align="center"/></a>

<figcaption>

Source: Centers for Disease Control and Prevention’s Public Health Image
Library

</figcaption>

## Installation

Install the CRAN version:

``` r
install.packages("coronavirus") 
```

Install the Github version (refreshed on a daily bases):

``` r
# install.packages("devtools")
devtools::install_github("RamiKrispin/coronavirus")
```

## Usage

The package contains a single dataset - `coronavirus`:

``` r
library(coronavirus) 

data("coronavirus")
```

This `coronavirus` dataset has the following fields:

``` r
head(coronavirus)
#>   Province.State Country.Region     Lat     Long       date cases      type
#> 1                         Japan 36.0000 138.0000 2020-01-22     2 confirmed
#> 2                   South Korea 36.0000 128.0000 2020-01-22     1 confirmed
#> 3                      Thailand 15.0000 101.0000 2020-01-22     2 confirmed
#> 4          Anhui Mainland China 31.8257 117.2264 2020-01-22     1 confirmed
#> 5        Beijing Mainland China 40.1824 116.4142 2020-01-22    14 confirmed
#> 6      Chongqing Mainland China 30.0572 107.8740 2020-01-22     6 confirmed
tail(coronavirus) 
#>      Province.State Country.Region     Lat     Long       date cases      type
#> 2480         Shanxi Mainland China 37.5777 112.2922 2020-03-01     2 recovered
#> 2481        Sichuan Mainland China 30.6171 102.7103 2020-03-01    14 recovered
#> 2482        Tianjin Mainland China 39.3054 117.3230 2020-03-01     2 recovered
#> 2483       Xinjiang Mainland China 41.1129  85.2401 2020-03-01     2 recovered
#> 2484         Yunnan Mainland China 24.9740 101.4870 2020-03-01     6 recovered
#> 2485       Zhejiang Mainland China 29.1832 120.0934 2020-03-01    30 recovered
```

Here is an example of a summary total cases by region and type (top 20):

``` r
library(dplyr)

summary_df <- coronavirus %>% group_by(Country.Region, type) %>%
  summarise(total_cases = sum(cases)) %>%
  arrange(-total_cases)

summary_df %>% head(20) 
#> # A tibble: 20 x 3
#> # Groups:   Country.Region [14]
#>    Country.Region type      total_cases
#>    <chr>          <chr>           <int>
#>  1 Mainland China confirmed       79826
#>  2 Mainland China recovered       42118
#>  3 South Korea    confirmed        3736
#>  4 Mainland China death            2870
#>  5 Italy          confirmed        1694
#>  6 Iran           confirmed         978
#>  7 Others         confirmed         705
#>  8 Japan          confirmed         256
#>  9 Iran           recovered         175
#> 10 France         confirmed         130
#> 11 Germany        confirmed         130
#> 12 Singapore      confirmed         106
#> 13 Hong Kong      confirmed          96
#> 14 Spain          confirmed          84
#> 15 Italy          recovered          83
#> 16 US             confirmed          76
#> 17 Singapore      recovered          72
#> 18 Iran           death              54
#> 19 Bahrain        confirmed          47
#> 20 Kuwait         confirmed          45
```

Summary of new cases during the past 24 hours by country and type (as of
2020-03-01):

``` r
library(tidyr)

coronavirus %>% 
  filter(date == max(date)) %>%
  select(country = Country.Region, type, cases) %>%
  group_by(country, type) %>%
  summarise(total_cases = sum(cases)) %>%
  pivot_wider(names_from = type,
              values_from = total_cases) %>%
  arrange(-confirmed)
#> # A tibble: 41 x 4
#> # Groups:   country [41]
#>    country            confirmed death recovered
#>    <chr>                  <int> <int>     <int>
#>  1 South Korea              586     1         3
#>  2 Mainland China           575    35      2839
#>  3 Italy                    566     5        37
#>  4 Iran                     385    11        52
#>  5 Germany                   51    NA        NA
#>  6 Spain                     39    NA        NA
#>  7 France                    30    NA        NA
#>  8 Japan                     15     1        NA
#>  9 UK                        13    NA        NA
#> 10 Switzerland                9    NA        NA
#> 11 Bahrain                    6    NA        NA
#> 12 Ecuador                    6    NA        NA
#> 13 Iraq                       6    NA        NA
#> 14 Lebanon                    6    NA        NA
#> 15 US                         6    NA        NA
#> 16 Austria                    5    NA        NA
#> 17 Canada                     4    NA        NA
#> 18 Malaysia                   4    NA        NA
#> 19 Netherlands                4    NA        NA
#> 20 Norway                     4    NA        NA
#> 21 Singapore                  4    NA        NA
#> 22 Azerbaijan                 3    NA        NA
#> 23 Czech Republic             3    NA        NA
#> 24 Finland                    3    NA        NA
#> 25 Greece                     3    NA        NA
#> 26 Israel                     3    NA        NA
#> 27 Australia                  2     1        NA
#> 28 Georgia                    2    NA        NA
#> 29 Iceland                    2    NA        NA
#> 30 Qatar                      2    NA        NA
#> 31 Sweden                     2    NA        NA
#> 32 Armenia                    1    NA        NA
#> 33 Belgium                    1    NA        NA
#> 34 Croatia                    1    NA        NA
#> 35 Denmark                    1    NA        NA
#> 36 Dominican Republic         1    NA        NA
#> 37 Egypt                      1    NA        NA
#> 38 Hong Kong                  1    NA         3
#> 39 Mexico                     1    NA        NA
#> 40 Taiwan                     1    NA        NA
#> 41 Thailand                  NA     1        NA
```

## Data Sources

The raw data pulled and arranged by the Johns Hopkins University Center
for Systems Science and Engineering (JHU CCSE) from the following
resources:

  - World Health Organization (WHO): <https://www.who.int/> <br>
  - DXY.cn. Pneumonia. 2020. <http://3g.dxy.cn/newh5/view/pneumonia>.
    <br>
  - BNO News:
    <https://bnonews.com/index.php/2020/02/the-latest-coronavirus-cases/>
    <br>
  - National Health Commission of the People’s Republic of China (NHC):
    http:://www.nhc.gov.cn/xcs/yqtb/list\_gzbd.shtml
  - China CDC (CCDC):
    http:://weekly.chinacdc.cn/news/TrackingtheEpidemic.htm <br>
  - Hong Kong Department of Health:
    <https://www.chp.gov.hk/en/features/102465.html> <br>
  - Macau Government: <https://www.ssm.gov.mo/portal/> <br>
  - Taiwan CDC:
    <https://sites.google.com/cdc.gov.tw/2019ncov/taiwan?authuser=0>
    <br>
  - US CDC: <https://www.cdc.gov/coronavirus/2019-ncov/index.html> <br>
  - Government of Canada:
    <https://www.canada.ca/en/public-health/services/diseases/coronavirus.html>
    <br>
  - Australia Government Department of Health:
    <https://www.health.gov.au/news/coronavirus-update-at-a-glance> <br>
  - European Centre for Disease Prevention and Control (ECDC):
    <https://www.ecdc.europa.eu/en/geographical-distribution-2019-ncov-cases>
    <br>

<br>
