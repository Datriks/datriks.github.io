---
layout: post
title: "Location Estimates of Population and Murder Rates"
subtitle: "Location Estimates of Population and Murder Rates with the help of dplyr"
background: '/img/posts/Location-Estimates-of-Population/Location Estimates of Population and Murder Rates-datriks.jpg'
---

Location Estimates of Population and Murder Rates
================

## Import the neccesary libraries

``` r
library(tidyverse)
library(flextable)
library(janitor)
```

## Import the file to be analised

``` r
state = read_csv("state.csv") %>% 
  clean_names() %>% 
  remove_empty()
```

## Analyse the file

``` r
# glimpse is part of the tydiverse package helps to get insights about the data frame : nr of column, nr of rows, features name, features data types
glimpse(state)
```

    ## Rows: 50
    ## Columns: 4
    ## $ state        <chr> "Alabama", "Alaska", "Arizona", "Arkansas", "Californi...
    ## $ population   <dbl> 4779736, 710231, 6392017, 2915918, 37253956, 5029196, ...
    ## $ murder_rate  <dbl> 5.7, 5.6, 4.7, 5.6, 4.4, 2.8, 2.4, 5.8, 5.8, 5.7, 1.8,...
    ## $ abbreviation <chr> "AL", "AK", "AZ", "AR", "CA", "CO", "CT", "DE", "FL", ...

## Observe a summary of the data

``` r
# with the help of the summary function we can obtain a rough statistics of the features that we have to analyse for more in depth analysis see dlookr package
summary(state)
```

    ##     state             population        murder_rate     abbreviation      
    ##  Length:50          Min.   :  563626   Min.   : 0.900   Length:50         
    ##  Class :character   1st Qu.: 1833004   1st Qu.: 2.425   Class :character  
    ##  Mode  :character   Median : 4436370   Median : 4.000   Mode  :character  
    ##                     Mean   : 6162876   Mean   : 4.066                     
    ##                     3rd Qu.: 6680312   3rd Qu.: 5.550                     
    ##                     Max.   :37253956   Max.   :10.300

## Computing the mean, trimmed mean, and median for the population using R:

``` r
state %>% 
  group_by(state) %>% 
  summarise(mean.pop = mean(population)) %>% 
  arrange(desc(mean.pop))
```

    ## # A tibble: 50 x 2
    ##    state          mean.pop
    ##    <chr>             <dbl>
    ##  1 California     37253956
    ##  2 Texas          25145561
    ##  3 New York       19378102
    ##  4 Florida        18801310
    ##  5 Illinois       12830632
    ##  6 Pennsylvania   12702379
    ##  7 Ohio           11536504
    ##  8 Michigan        9883640
    ##  9 Georgia         9687653
    ## 10 North Carolina  9535483
    ## # ... with 40 more rows

## Trimmed Mean Calculation

``` r
# The average of all values after dropping a fixed number of extreme values.
# in ou case we drop 10% of the extreme cases
state %>% 
  group_by(state) %>% 
  summarise(mean.trim = mean(population, trim = 0.1)) %>% 
  arrange(desc(mean.trim))
```

    ## # A tibble: 50 x 2
    ##    state          mean.trim
    ##    <chr>              <dbl>
    ##  1 California      37253956
    ##  2 Texas           25145561
    ##  3 New York        19378102
    ##  4 Florida         18801310
    ##  5 Illinois        12830632
    ##  6 Pennsylvania    12702379
    ##  7 Ohio            11536504
    ##  8 Michigan         9883640
    ##  9 Georgia          9687653
    ## 10 North Carolina   9535483
    ## # ... with 40 more rows

## Trimmed Mean Calculation for Murder.Rate

``` r
state %>% 
  group_by(state) %>% 
  summarise(mean.trim.murder = mean(murder_rate, trim = 0.1)) %>% 
  arrange(desc(mean.trim.murder))
```

    ## # A tibble: 50 x 2
    ##    state          mean.trim.murder
    ##    <chr>                     <dbl>
    ##  1 Louisiana                  10.3
    ##  2 Mississippi                 8.6
    ##  3 Missouri                    6.6
    ##  4 South Carolina              6.4
    ##  5 Maryland                    6.1
    ##  6 Nevada                      6  
    ##  7 Delaware                    5.8
    ##  8 Florida                     5.8
    ##  9 Alabama                     5.7
    ## 10 Georgia                     5.7
    ## # ... with 40 more rows

## Comparison between mean and trim mean of population

``` r
state %>% 
  group_by(state) %>% 
  summarise(mean.pop = mean(population),
            mean.trim.pop = mean(population, trim = 0.2)) %>% 
  arrange(desc(mean.pop))
```

    ## # A tibble: 50 x 3
    ##    state          mean.pop mean.trim.pop
    ##    <chr>             <dbl>         <dbl>
    ##  1 California     37253956      37253956
    ##  2 Texas          25145561      25145561
    ##  3 New York       19378102      19378102
    ##  4 Florida        18801310      18801310
    ##  5 Illinois       12830632      12830632
    ##  6 Pennsylvania   12702379      12702379
    ##  7 Ohio           11536504      11536504
    ##  8 Michigan        9883640       9883640
    ##  9 Georgia         9687653       9687653
    ## 10 North Carolina  9535483       9535483
    ## # ... with 40 more rows

Claculate the median of the Population Feature

``` r
# The value such that one-half of the data lies above and below.
state %>% 
  group_by(state) %>% 
  summarise(median.pop = median(population)) %>% 
  arrange(desc(median.pop))
```

    ## # A tibble: 50 x 2
    ##    state          median.pop
    ##    <chr>               <dbl>
    ##  1 California       37253956
    ##  2 Texas            25145561
    ##  3 New York         19378102
    ##  4 Florida          18801310
    ##  5 Illinois         12830632
    ##  6 Pennsylvania     12702379
    ##  7 Ohio             11536504
    ##  8 Michigan          9883640
    ##  9 Georgia           9687653
    ## 10 North Carolina    9535483
    ## # ... with 40 more rows

## Calculate the median of the

``` r
glimpse(state)
```

    ## Rows: 50
    ## Columns: 4
    ## $ state        <chr> "Alabama", "Alaska", "Arizona", "Arkansas", "Californi...
    ## $ population   <dbl> 4779736, 710231, 6392017, 2915918, 37253956, 5029196, ...
    ## $ murder_rate  <dbl> 5.7, 5.6, 4.7, 5.6, 4.4, 2.8, 2.4, 5.8, 5.8, 5.7, 1.8,...
    ## $ abbreviation <chr> "AL", "AK", "AZ", "AR", "CA", "CO", "CT", "DE", "FL", ...

``` r
state %>% 
  group_by(state) %>% 
  summarise(median.murder = median(murder_rate),
            median.pop = median(population)) %>% 
  arrange(desc(median.murder))
```

    ## # A tibble: 50 x 3
    ##    state          median.murder median.pop
    ##    <chr>                  <dbl>      <dbl>
    ##  1 Louisiana               10.3    4533372
    ##  2 Mississippi              8.6    2967297
    ##  3 Missouri                 6.6    5988927
    ##  4 South Carolina           6.4    4625364
    ##  5 Maryland                 6.1    5773552
    ##  6 Nevada                   6      2700551
    ##  7 Delaware                 5.8     897934
    ##  8 Florida                  5.8   18801310
    ##  9 Alabama                  5.7    4779736
    ## 10 Georgia                  5.7    9687653
    ## # ... with 40 more rows

## Load package to analyse weighted median

``` r
#install.packages("matrixStats")
library(matrixStats)
weighted.mean(state[['murder_rate']], w=state[['population']])
```

    ## [1] 4.445834

## This evaluates the weighted mean of murder rate per individual state

``` r
# In order to compute the average murder rate for the country, we need to use a weighted mean or median to account for different populations in the states.
state %>% 
  group_by(state) %>% 
  summarise(meanpop = weighted.mean(murder_rate, population)) %>% 
  arrange(desc(meanpop))
```

    ## # A tibble: 50 x 2
    ##    state          meanpop
    ##    <chr>            <dbl>
    ##  1 Louisiana         10.3
    ##  2 Mississippi        8.6
    ##  3 Missouri           6.6
    ##  4 South Carolina     6.4
    ##  5 Maryland           6.1
    ##  6 Nevada             6  
    ##  7 Delaware           5.8
    ##  8 Florida            5.8
    ##  9 Alabama            5.7
    ## 10 Georgia            5.7
    ## # ... with 40 more rows

## This evaluates the weighted mean of murder rate per all states

``` r
state %>% 
  summarise(meanpop = weighted.mean(murder_rate, population))
```

    ## # A tibble: 1 x 1
    ##   meanpop
    ##     <dbl>
    ## 1    4.45

## Evaluates the weighted median of murder rate by state

``` r
state %>% 
  group_by(state) %>% 
  summarise(medianmurder = weightedMedian(murder_rate, population)) %>% 
  arrange(desc(medianmurder))
```

    ## # A tibble: 50 x 2
    ##    state          medianmurder
    ##    <chr>                 <dbl>
    ##  1 Louisiana              10.3
    ##  2 Mississippi             8.6
    ##  3 Missouri                6.6
    ##  4 South Carolina          6.4
    ##  5 Maryland                6.1
    ##  6 Nevada                  6  
    ##  7 Delaware                5.8
    ##  8 Florida                 5.8
    ##  9 Alabama                 5.7
    ## 10 Georgia                 5.7
    ## # ... with 40 more rows

## Evaluation of weighted median of murder rate per all states

``` r
state %>% 
  summarise(medianmurder = weightedMedian(murder_rate, population))
```

    ## # A tibble: 1 x 1
    ##   medianmurder
    ##          <dbl>
    ## 1          4.4

### Key Ideas

• The basic metric for location is the mean, but it can be sensitive to
extreme values (outlier). • Other metrics (median, trimmed mean) are
less sensitive to outliers and unusual distributions and hence are more
robust.
