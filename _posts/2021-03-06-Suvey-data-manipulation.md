---
layout: post
title: "Data Manipulation in R"
subtitle: "Survey data Cleaning and Reshaping"
background: '/img/posts/survey-data/survey-data-analysis-datriks.jpg'
---

Suvey data manipulation
================

## Loading the Survey Data

### We are investigating the animal species diversity and weights found within plots at our study site. The dataset is stored as a comma separated value (CSV) file.

``` r
# set the working directory
setwd("D:\\!TRAINING\\!R Programming for Statistics")
```

## Import the Library

``` r
library(tidyverse)
library(janitor)
library(hablar)
library(flextable)
```

## Import the data set from the web location

``` r
download.file(url = "https://ndownloader.figshare.com/files/2292169",
              destfile = "portal_data_joined.csv")
```

## Insert Theme configuration

``` r
newtheme <- theme_light() + 
  theme(plot.title = element_text(color = "#002949", face = 'bold', size =12),
        plot.subtitle = element_text(color = "#890000", size = 10),
        plot.caption = element_text(color = '#890000', face = 'italic', size =8),
        #panel.border = element_rect(color = "#002949", size = 1),
        #legend.position = c(0.8, 0.2),
        #legend.text = element_text(colour="blue", size=10, face="bold"),
        #legend.title = element_text(colour="blue", size=10, face="bold"),
        legend.position='none',
        axis.title.x = element_text(colour = "#890000"),
        axis.title.y = element_text(colour = "#002949"),
        axis.text.x = element_text(angle = 45, hjust = 1, color = '#890000'),
        axis.text.y = element_text(angle = 45, hjust = 1, color = '#002949'),
        axis.line = element_line(color = "#002949", size =1),
  )

theme_set(newtheme)
```

## Read the file

``` r
survey = read_csv("portal_data_joined.csv") %>% 
  clean_names() %>% 
  remove_empty()
```

    ## value for "which" not specified, defaulting to c("rows", "cols")

    ## 
    ## -- Column specification --------------------------------------------------------
    ## cols(
    ##   record_id = col_double(),
    ##   month = col_double(),
    ##   day = col_double(),
    ##   year = col_double(),
    ##   plot_id = col_double(),
    ##   species_id = col_character(),
    ##   sex = col_character(),
    ##   hindfoot_length = col_double(),
    ##   weight = col_double(),
    ##   genus = col_character(),
    ##   species = col_character(),
    ##   taxa = col_character(),
    ##   plot_type = col_character()
    ## )

## Evaluate the data frame

``` r
glimpse(survey)
```

    ## Rows: 34,786
    ## Columns: 13
    ## $ record_id       <dbl> 1, 72, 224, 266, 349, 363, 435, 506, 588, 661, 748,...
    ## $ month           <dbl> 7, 8, 9, 10, 11, 11, 12, 1, 2, 3, 4, 5, 6, 8, 9, 10...
    ## $ day             <dbl> 16, 19, 13, 16, 12, 12, 10, 8, 18, 11, 8, 6, 9, 5, ...
    ## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1978, 197...
    ## $ plot_id         <dbl> 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, ...
    ## $ species_id      <chr> "NL", "NL", "NL", "NL", "NL", "NL", "NL", "NL", "NL...
    ## $ sex             <chr> "M", "M", NA, NA, NA, NA, NA, NA, "M", NA, NA, "M",...
    ## $ hindfoot_length <dbl> 32, 31, NA, NA, NA, NA, NA, NA, NA, NA, NA, 32, NA,...
    ## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, 218, NA, NA, 204, 2...
    ## $ genus           <chr> "Neotoma", "Neotoma", "Neotoma", "Neotoma", "Neotom...
    ## $ species         <chr> "albigula", "albigula", "albigula", "albigula", "al...
    ## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "...
    ## $ plot_type       <chr> "Control", "Control", "Control", "Control", "Contro...

``` r
view(survey)
str(survey)
```

    ## tibble [34,786 x 13] (S3: tbl_df/tbl/data.frame)
    ##  $ record_id      : num [1:34786] 1 72 224 266 349 363 435 506 588 661 ...
    ##  $ month          : num [1:34786] 7 8 9 10 11 11 12 1 2 3 ...
    ##  $ day            : num [1:34786] 16 19 13 16 12 12 10 8 18 11 ...
    ##  $ year           : num [1:34786] 1977 1977 1977 1977 1977 ...
    ##  $ plot_id        : num [1:34786] 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ species_id     : chr [1:34786] "NL" "NL" "NL" "NL" ...
    ##  $ sex            : chr [1:34786] "M" "M" NA NA ...
    ##  $ hindfoot_length: num [1:34786] 32 31 NA NA NA NA NA NA NA NA ...
    ##  $ weight         : num [1:34786] NA NA NA NA NA NA NA NA 218 NA ...
    ##  $ genus          : chr [1:34786] "Neotoma" "Neotoma" "Neotoma" "Neotoma" ...
    ##  $ species        : chr [1:34786] "albigula" "albigula" "albigula" "albigula" ...
    ##  $ taxa           : chr [1:34786] "Rodent" "Rodent" "Rodent" "Rodent" ...
    ##  $ plot_type      : chr [1:34786] "Control" "Control" "Control" "Control" ...

``` r
summary(survey)
```

    ##    record_id         month             day            year         plot_id     
    ##  Min.   :    1   Min.   : 1.000   Min.   : 1.0   Min.   :1977   Min.   : 1.00  
    ##  1st Qu.: 8964   1st Qu.: 4.000   1st Qu.: 9.0   1st Qu.:1984   1st Qu.: 5.00  
    ##  Median :17762   Median : 6.000   Median :16.0   Median :1990   Median :11.00  
    ##  Mean   :17804   Mean   : 6.474   Mean   :16.1   Mean   :1990   Mean   :11.34  
    ##  3rd Qu.:26655   3rd Qu.:10.000   3rd Qu.:23.0   3rd Qu.:1997   3rd Qu.:17.00  
    ##  Max.   :35548   Max.   :12.000   Max.   :31.0   Max.   :2002   Max.   :24.00  
    ##                                                                                
    ##   species_id            sex            hindfoot_length     weight      
    ##  Length:34786       Length:34786       Min.   : 2.00   Min.   :  4.00  
    ##  Class :character   Class :character   1st Qu.:21.00   1st Qu.: 20.00  
    ##  Mode  :character   Mode  :character   Median :32.00   Median : 37.00  
    ##                                        Mean   :29.29   Mean   : 42.67  
    ##                                        3rd Qu.:36.00   3rd Qu.: 48.00  
    ##                                        Max.   :70.00   Max.   :280.00  
    ##                                        NA's   :3348    NA's   :2503    
    ##     genus             species              taxa            plot_type        
    ##  Length:34786       Length:34786       Length:34786       Length:34786      
    ##  Class :character   Class :character   Class :character   Class :character  
    ##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
    ##                                                                             
    ##                                                                             
    ##                                                                             
    ## 

## Check for NAs

``` r
survey %>% 
  map(~ sum(is.na(.))) %>% 
  view()
```

``` r
class(survey)
```

    ## [1] "tbl_df"     "tbl"        "data.frame"

``` r
# extract the last row
survey_last = survey[34786,]
survey_last
```

    ## # A tibble: 1 x 13
    ##   record_id month   day  year plot_id species_id sex   hindfoot_length weight
    ##       <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <chr>           <dbl>  <dbl>
    ## 1     30986     7     1  2000       7 PX         <NA>               NA     NA
    ## # ... with 4 more variables: genus <chr>, species <chr>, taxa <chr>,
    ## #   plot_type <chr>

## Convert character columns to factors

``` r
library(forcats)
survey = survey %>% 
  convert(fct(sex,taxa,genus))
#convert the level NA in feature sex as undetermined
levels(survey$sex)
```

    ## [1] "F" "M"

``` r
# as we saw when we used summary(surveys$sex), there are about 1700 individuals for which the sex information hasn't been recorded. To show them in the plot, we can turn the missing values into a factor level with the addNA()
survey = survey %>% 
  mutate(
    sex = fct_explicit_na(sex, "undetermined")
  )
# check the levels
levels(survey$sex)
```

    ## [1] "F"            "M"            "undetermined"

``` r
# check the number of levels
nlevels(survey$genus)
```

    ## [1] 26

``` r
survey %>% 
  ggplot(aes(sex))+
  geom_bar(aes(fill = sex))
```

![](img/posts/survey-data/unnamed-chunk-10-1.png)<!-- -->
\#\# Date time manipulation

``` r
# most necessary library for date time transformation is lubridate
# combine the year, month, day features to form one feature called date
# survey = survey %>% 
#   mutate(
#     date = ymd(paste(year, month, day, sep = "-"))
#   )
# # because we obtain an error with 129 values failed to parse
# summary(survey$date)
```

## Extract the dates that present themselves as NAs

``` r
# missing_dates = survey[is.na(survey$date), c("year","month","day")]
# missing_dates

survey %>% 
  filter(
    is.na(date)
  ) %>% 
  select(
    year,month,day
  )
```

    ## Warning in is.na(date): is.na() applied to non-(list or vector) of type
    ## 'closure'

    ## # A tibble: 0 x 3
    ## # ... with 3 variables: year <dbl>, month <dbl>, day <dbl>

# Data manipulation using dplyr and tidyr

``` r
# To select columns of a data frame, use select()
survey %>% 
  select(
    plot_id,species_id,weight
  )
```

    ## # A tibble: 34,786 x 3
    ##    plot_id species_id weight
    ##      <dbl> <chr>       <dbl>
    ##  1       2 NL             NA
    ##  2       2 NL             NA
    ##  3       2 NL             NA
    ##  4       2 NL             NA
    ##  5       2 NL             NA
    ##  6       2 NL             NA
    ##  7       2 NL             NA
    ##  8       2 NL             NA
    ##  9       2 NL            218
    ## 10       2 NL             NA
    ## # ... with 34,776 more rows

``` r
# To select all columns except certain ones, put a "-" in front of the variable to exclude it
survey %>% 
  select(
    -record_id,species_id
  )
```

    ## # A tibble: 34,786 x 12
    ##    month   day  year plot_id species_id sex   hindfoot_length weight genus
    ##    <dbl> <dbl> <dbl>   <dbl> <chr>      <fct>           <dbl>  <dbl> <fct>
    ##  1     7    16  1977       2 NL         M                  32     NA Neot~
    ##  2     8    19  1977       2 NL         M                  31     NA Neot~
    ##  3     9    13  1977       2 NL         unde~              NA     NA Neot~
    ##  4    10    16  1977       2 NL         unde~              NA     NA Neot~
    ##  5    11    12  1977       2 NL         unde~              NA     NA Neot~
    ##  6    11    12  1977       2 NL         unde~              NA     NA Neot~
    ##  7    12    10  1977       2 NL         unde~              NA     NA Neot~
    ##  8     1     8  1978       2 NL         unde~              NA     NA Neot~
    ##  9     2    18  1978       2 NL         M                  NA    218 Neot~
    ## 10     3    11  1978       2 NL         unde~              NA     NA Neot~
    ## # ... with 34,776 more rows, and 3 more variables: species <chr>, taxa <fct>,
    ## #   plot_type <chr>

## Use filter to sample the data

``` r
survey %>% 
  filter(
    year == 1977
  )
```

    ## # A tibble: 487 x 13
    ##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
    ##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <fct>           <dbl>  <dbl>
    ##  1         1     7    16  1977       2 NL         M                  32     NA
    ##  2        72     8    19  1977       2 NL         M                  31     NA
    ##  3       224     9    13  1977       2 NL         unde~              NA     NA
    ##  4       266    10    16  1977       2 NL         unde~              NA     NA
    ##  5       349    11    12  1977       2 NL         unde~              NA     NA
    ##  6       363    11    12  1977       2 NL         unde~              NA     NA
    ##  7       435    12    10  1977       2 NL         unde~              NA     NA
    ##  8         3     7    16  1977       2 DM         F                  37     NA
    ##  9       226     9    13  1977       2 DM         M                  37     51
    ## 10       233     9    13  1977       2 DM         M                  25     44
    ## # ... with 477 more rows, and 4 more variables: genus <fct>, species <chr>,
    ## #   taxa <fct>, plot_type <chr>

``` r
survey %>% 
  filter(
    weight < 5
  ) %>% 
  select(
    species_id, sex, weight
  )
```

    ## # A tibble: 17 x 3
    ##    species_id sex          weight
    ##    <chr>      <fct>         <dbl>
    ##  1 PF         F                 4
    ##  2 PF         F                 4
    ##  3 PF         M                 4
    ##  4 RM         F                 4
    ##  5 RM         M                 4
    ##  6 PF         undetermined      4
    ##  7 PP         M                 4
    ##  8 RM         M                 4
    ##  9 RM         M                 4
    ## 10 RM         M                 4
    ## 11 PF         M                 4
    ## 12 PF         F                 4
    ## 13 RM         M                 4
    ## 14 RM         M                 4
    ## 15 RM         F                 4
    ## 16 RM         M                 4
    ## 17 RM         M                 4

``` r
survey %>% 
  filter(
    year == 1995
  ) %>% 
  select(
    year,sex,weight
  )
```

    ## # A tibble: 1,180 x 3
    ##     year sex   weight
    ##    <dbl> <fct>  <dbl>
    ##  1  1995 M         NA
    ##  2  1995 F        165
    ##  3  1995 F        171
    ##  4  1995 F         NA
    ##  5  1995 M         41
    ##  6  1995 F         45
    ##  7  1995 M         46
    ##  8  1995 F         49
    ##  9  1995 M         46
    ## 10  1995 M         48
    ## # ... with 1,170 more rows

## Use mutate dplyr function

``` r
survey %>% 
  mutate(
    weight_kg = weight / 1000
  )
```

    ## # A tibble: 34,786 x 14
    ##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
    ##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <fct>           <dbl>  <dbl>
    ##  1         1     7    16  1977       2 NL         M                  32     NA
    ##  2        72     8    19  1977       2 NL         M                  31     NA
    ##  3       224     9    13  1977       2 NL         unde~              NA     NA
    ##  4       266    10    16  1977       2 NL         unde~              NA     NA
    ##  5       349    11    12  1977       2 NL         unde~              NA     NA
    ##  6       363    11    12  1977       2 NL         unde~              NA     NA
    ##  7       435    12    10  1977       2 NL         unde~              NA     NA
    ##  8       506     1     8  1978       2 NL         unde~              NA     NA
    ##  9       588     2    18  1978       2 NL         M                  NA    218
    ## 10       661     3    11  1978       2 NL         unde~              NA     NA
    ## # ... with 34,776 more rows, and 5 more variables: genus <fct>, species <chr>,
    ## #   taxa <fct>, plot_type <chr>, weight_kg <dbl>

``` r
#You can also create a second new column based on the first new column within the same call of mutate()
survey %>% 
  mutate(
    weight_kg = weight /1000,
    weight_lb = weight_kg * 2.2
  )
```

    ## # A tibble: 34,786 x 15
    ##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
    ##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <fct>           <dbl>  <dbl>
    ##  1         1     7    16  1977       2 NL         M                  32     NA
    ##  2        72     8    19  1977       2 NL         M                  31     NA
    ##  3       224     9    13  1977       2 NL         unde~              NA     NA
    ##  4       266    10    16  1977       2 NL         unde~              NA     NA
    ##  5       349    11    12  1977       2 NL         unde~              NA     NA
    ##  6       363    11    12  1977       2 NL         unde~              NA     NA
    ##  7       435    12    10  1977       2 NL         unde~              NA     NA
    ##  8       506     1     8  1978       2 NL         unde~              NA     NA
    ##  9       588     2    18  1978       2 NL         M                  NA    218
    ## 10       661     3    11  1978       2 NL         unde~              NA     NA
    ## # ... with 34,776 more rows, and 6 more variables: genus <fct>, species <chr>,
    ## #   taxa <fct>, plot_type <chr>, weight_kg <dbl>, weight_lb <dbl>

``` r
# The first few rows of the output are full of NAs, so if we wanted to remove those we could insert a filter() in the chain:
survey %>% 
  filter(!is.na(weight)) %>% 
  mutate(
    weight_kg = weight /1000,
    weight_lb = weight_kg * 2.2
  )
```

    ## # A tibble: 32,283 x 15
    ##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
    ##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <fct>           <dbl>  <dbl>
    ##  1       588     2    18  1978       2 NL         M                  NA    218
    ##  2       845     5     6  1978       2 NL         M                  32    204
    ##  3       990     6     9  1978       2 NL         M                  NA    200
    ##  4      1164     8     5  1978       2 NL         M                  34    199
    ##  5      1261     9     4  1978       2 NL         M                  32    197
    ##  6      1453    11     5  1978       2 NL         M                  NA    218
    ##  7      1756     4    29  1979       2 NL         M                  33    166
    ##  8      1818     5    30  1979       2 NL         M                  32    184
    ##  9      1882     7     4  1979       2 NL         M                  32    206
    ## 10      2133    10    25  1979       2 NL         F                  33    274
    ## # ... with 32,273 more rows, and 6 more variables: genus <fct>, species <chr>,
    ## #   taxa <fct>, plot_type <chr>, weight_kg <dbl>, weight_lb <dbl>

## Practice subsetting baset on multiple criteria

``` r
survey_hindfoot = survey %>% 
  mutate(
    hindfoot_cm = hindfoot_length/10
  ) %>% 
  select(species_id, hindfoot_cm) %>% 
  filter(!is.na(hindfoot_cm) & hindfoot_cm < 3) %>% 
  arrange(desc(hindfoot_cm))

survey_hindfoot
```

    ## # A tibble: 15,371 x 2
    ##    species_id hindfoot_cm
    ##    <chr>            <dbl>
    ##  1 NL                 2.9
    ##  2 NL                 2.9
    ##  3 PB                 2.9
    ##  4 NL                 2.9
    ##  5 NL                 2.9
    ##  6 NL                 2.9
    ##  7 NL                 2.9
    ##  8 SH                 2.9
    ##  9 SH                 2.9
    ## 10 SH                 2.9
    ## # ... with 15,361 more rows

## Split-apply-combine data analysis and the summarize() function

``` r
survey %>% 
  group_by(sex) %>% 
  summarise(mean_weight = mean(weight, na.rm = T))
```

    ## # A tibble: 3 x 2
    ##   sex          mean_weight
    ##   <fct>              <dbl>
    ## 1 F                   42.2
    ## 2 M                   43.0
    ## 3 undetermined        64.7

### Group by multiple columns:

``` r
survey %>% 
  group_by(sex,species_id) %>% 
  summarise(mean_weight = mean(weight, na.rm = T)) %>% 
  arrange(desc(mean_weight))
```

    ## `summarise()` regrouping output by 'sex' (override with `.groups` argument)

    ## # A tibble: 92 x 3
    ## # Groups:   sex [3]
    ##    sex          species_id mean_weight
    ##    <fct>        <chr>            <dbl>
    ##  1 undetermined NL               168. 
    ##  2 M            NL               166. 
    ##  3 F            NL               154. 
    ##  4 M            SS               130  
    ##  5 undetermined SH               130  
    ##  6 M            DS               122. 
    ##  7 undetermined DS               120  
    ##  8 F            DS               118. 
    ##  9 F            SH                78.8
    ## 10 F            SF                69  
    ## # ... with 82 more rows

### Eliminate na.rm from our script

``` r
survey %>% 
  filter(!is.na(weight)) %>% 
  group_by(sex, species_id) %>% 
  summarise(mean_weight = mean(weight),
            min_weight = min(weight)) %>% 
  arrange(desc(min_weight))
```

    ## `summarise()` regrouping output by 'sex' (override with `.groups` argument)

    ## # A tibble: 64 x 4
    ## # Groups:   sex [3]
    ##    sex          species_id mean_weight min_weight
    ##    <fct>        <chr>            <dbl>      <dbl>
    ##  1 M            SS               130          130
    ##  2 undetermined SH               130          130
    ##  3 undetermined NL               168.          83
    ##  4 undetermined DS               120           78
    ##  5 F            SS                57           57
    ##  6 F            SF                69           46
    ##  7 F            DS               118.          45
    ##  8 undetermined DO                50.7         44
    ##  9 undetermined SF                40.5         36
    ## 10 M            SO                55.7         35
    ## # ... with 54 more rows

### Counting

``` r
# When working with data, we often want to know the number of observations found for each factor or combination of factors. For this task, dplyr provides count().

survey %>% 
  count(sex)
```

    ## # A tibble: 3 x 2
    ##   sex              n
    ##   <fct>        <int>
    ## 1 F            15690
    ## 2 M            17348
    ## 3 undetermined  1748

``` r
# this is a short hand,
survey %>% 
  count(sex, sort = T)
```

    ## # A tibble: 3 x 2
    ##   sex              n
    ##   <fct>        <int>
    ## 1 M            17348
    ## 2 F            15690
    ## 3 undetermined  1748

``` r
# for this
survey %>% 
  group_by(sex) %>% 
  summarise(count = n())
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 3 x 2
    ##   sex          count
    ##   <fct>        <int>
    ## 1 F            15690
    ## 2 M            17348
    ## 3 undetermined  1748

## Visualisation of species distribution

``` r
obs = survey %>% 
  count(sex, species) %>% 
  arrange(species, desc(n))

# visualisation of distribution of species by sex
obs %>% 
  ggplot(aes(species))+
  geom_bar(aes(fill = species))+
  labs(
    caption = "by Paul Juverdeanu"
  )
```

![](img/posts/survey-data/unnamed-chunk-21-1.png)<!-- -->

``` r
survey %>% 
  group_by(plot_type) %>% 
  summarise(count = n())
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 5 x 2
    ##   plot_type                 count
    ##   <chr>                     <int>
    ## 1 Control                   15611
    ## 2 Long-term Krat Exclosure   5118
    ## 3 Rodent Exclosure           4233
    ## 4 Short-term Krat Exclosure  5906
    ## 5 Spectab exclosure          3918

``` r
survey %>% 
  count(plot_type, sort = T)
```

    ## # A tibble: 5 x 2
    ##   plot_type                     n
    ##   <chr>                     <int>
    ## 1 Control                   15611
    ## 2 Short-term Krat Exclosure  5906
    ## 3 Long-term Krat Exclosure   5118
    ## 4 Rodent Exclosure           4233
    ## 5 Spectab exclosure          3918

### Find the mean, min and max of hindfoot\_length

``` r
hindfoot = survey %>% 
  filter(!is.na(hindfoot_length)) %>% 
  group_by(species_id) %>% 
  summarise(
    mean_hindfoot = mean(hindfoot_length),
    min_hindfoot = min(hindfoot_length),
    max_hindfoot = max(hindfoot_length)
  )
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

``` r
hindfoot
```

    ## # A tibble: 25 x 4
    ##    species_id mean_hindfoot min_hindfoot max_hindfoot
    ##    <chr>              <dbl>        <dbl>        <dbl>
    ##  1 AH                  33             31           35
    ##  2 BA                  13              6           16
    ##  3 DM                  36.0           16           50
    ##  4 DO                  35.6           26           64
    ##  5 DS                  49.9           39           58
    ##  6 NL                  32.3           21           70
    ##  7 OL                  20.5           12           39
    ##  8 OT                  20.3           13           50
    ##  9 OX                  19.1           13           21
    ## 10 PB                  26.1            2           47
    ## # ... with 15 more rows

### What was the heaviest animal measured in each year?

``` r
survey %>% 
  filter(!is.na(weight)) %>% 
  group_by(year) %>% 
  filter(weight == max(weight)) %>% 
  select(year, genus, species_id,weight) %>% 
  arrange(desc(weight))
```

    ## # A tibble: 27 x 4
    ## # Groups:   year [26]
    ##     year genus   species_id weight
    ##    <dbl> <fct>   <chr>       <dbl>
    ##  1  2001 Neotoma NL            280
    ##  2  1987 Neotoma NL            278
    ##  3  1989 Neotoma NL            275
    ##  4  1979 Neotoma NL            274
    ##  5  2000 Neotoma NL            265
    ##  6  1981 Neotoma NL            264
    ##  7  1984 Neotoma NL            259
    ##  8  1983 Neotoma NL            256
    ##  9  1982 Neotoma NL            252
    ## 10  1988 Neotoma NL            248
    ## # ... with 17 more rows

## Reshaping the data with pivot\_longer anf pivot\_wider

``` r
# Let's use pivot_wider() to transform surveys to find the mean weight of each genus in each plot over the entire survey period. We use filter(), group_by() and summarise() to filter our observations and variables of interest, and create a new variable for the mean_weight

survey_gw = survey %>% 
  filter(!is.na(weight)) %>% 
  group_by(plot_id, genus) %>% 
  summarise(mean_weight = mean(weight))
```

    ## `summarise()` regrouping output by 'plot_id' (override with `.groups` argument)

``` r
survey_gw %>% flextable()
```

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

``` r
str(survey_gw)
```

    ## tibble [196 x 3] (S3: grouped_df/tbl_df/tbl/data.frame)
    ##  $ plot_id    : num [1:196] 1 1 1 1 1 1 1 1 2 2 ...
    ##  $ genus      : Factor w/ 26 levels "Ammodramus","Ammospermophilus",..: 4 8 11 13 14 15 16 19 4 8 ...
    ##  $ mean_weight: num [1:196] 7 22.2 60.2 156.2 27.7 ...
    ##  - attr(*, "groups")= tibble [24 x 2] (S3: tbl_df/tbl/data.frame)
    ##   ..$ plot_id: num [1:24] 1 2 3 4 5 6 7 8 9 10 ...
    ##   ..$ .rows  : list<int> [1:24] 
    ##   .. ..$ : int [1:8] 1 2 3 4 5 6 7 8
    ##   .. ..$ : int [1:9] 9 10 11 12 13 14 15 16 17
    ##   .. ..$ : int [1:9] 18 19 20 21 22 23 24 25 26
    ##   .. ..$ : int [1:8] 27 28 29 30 31 32 33 34
    ##   .. ..$ : int [1:9] 35 36 37 38 39 40 41 42 43
    ##   .. ..$ : int [1:8] 44 45 46 47 48 49 50 51
    ##   .. ..$ : int [1:7] 52 53 54 55 56 57 58
    ##   .. ..$ : int [1:7] 59 60 61 62 63 64 65
    ##   .. ..$ : int [1:8] 66 67 68 69 70 71 72 73
    ##   .. ..$ : int [1:7] 74 75 76 77 78 79 80
    ##   .. ..$ : int [1:8] 81 82 83 84 85 86 87 88
    ##   .. ..$ : int [1:8] 89 90 91 92 93 94 95 96
    ##   .. ..$ : int [1:8] 97 98 99 100 101 102 103 104
    ##   .. ..$ : int [1:8] 105 106 107 108 109 110 111 112
    ##   .. ..$ : int [1:8] 113 114 115 116 117 118 119 120
    ##   .. ..$ : int [1:7] 121 122 123 124 125 126 127
    ##   .. ..$ : int [1:8] 128 129 130 131 132 133 134 135
    ##   .. ..$ : int [1:9] 136 137 138 139 140 141 142 143 144
    ##   .. ..$ : int [1:9] 145 146 147 148 149 150 151 152 153
    ##   .. ..$ : int [1:10] 154 155 156 157 158 159 160 161 162 163
    ##   .. ..$ : int [1:9] 164 165 166 167 168 169 170 171 172
    ##   .. ..$ : int [1:8] 173 174 175 176 177 178 179 180
    ##   .. ..$ : int [1:8] 181 182 183 184 185 186 187 188
    ##   .. ..$ : int [1:8] 189 190 191 192 193 194 195 196
    ##   .. ..@ ptype: int(0) 
    ##   ..- attr(*, ".drop")= logi TRUE

``` r
glimpse(survey_gw)
```

    ## Rows: 196
    ## Columns: 3
    ## Groups: plot_id [24]
    ## $ plot_id     <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2, 2, 3, 3...
    ## $ genus       <fct> Baiomys, Chaetodipus, Dipodomys, Neotoma, Onychomys, Pe...
    ## $ mean_weight <dbl> 7.000000, 22.199387, 60.232143, 156.222222, 27.675497, ...

## Transform the data frame with pivot\_wider()

``` r
# with pivot_wider a column ca be spread horizontally
survey_wider = survey_gw %>% 
  pivot_wider(names_from = genus, values_from = mean_weight)

survey_wider %>% flextable()
```

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

``` r
glimpse(survey_wider)
```

    ## Rows: 24
    ## Columns: 11
    ## Groups: plot_id [24]
    ## $ plot_id         <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, ...
    ## $ Baiomys         <dbl> 7.000000, 6.000000, 8.611111, NA, 7.750000, NA, NA,...
    ## $ Chaetodipus     <dbl> 22.19939, 25.11014, 24.63636, 23.02381, 17.98276, 2...
    ## $ Dipodomys       <dbl> 60.23214, 55.68259, 52.04688, 57.52454, 51.11356, 5...
    ## $ Neotoma         <dbl> 156.2222, 169.1436, 158.2414, 164.1667, 190.0370, 1...
    ## $ Onychomys       <dbl> 27.67550, 26.87302, 26.03241, 28.09375, 27.01695, 2...
    ## $ Perognathus     <dbl> 9.625000, 6.947368, 7.507812, 7.824427, 8.658537, 7...
    ## $ Peromyscus      <dbl> 22.22222, 22.26966, 21.37037, 22.60000, 21.23171, 2...
    ## $ Reithrodontomys <dbl> 11.375000, 10.680556, 10.516588, 10.263158, 11.1545...
    ## $ Sigmodon        <dbl> NA, 70.85714, 65.61404, 82.00000, 82.66667, 68.7777...
    ## $ Spermophilus    <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA,...

``` r
str(survey_wider)
```

    ## tibble [24 x 11] (S3: grouped_df/tbl_df/tbl/data.frame)
    ##  $ plot_id        : num [1:24] 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ Baiomys        : num [1:24] 7 6 8.61 NA 7.75 ...
    ##  $ Chaetodipus    : num [1:24] 22.2 25.1 24.6 23 18 ...
    ##  $ Dipodomys      : num [1:24] 60.2 55.7 52 57.5 51.1 ...
    ##  $ Neotoma        : num [1:24] 156 169 158 164 190 ...
    ##  $ Onychomys      : num [1:24] 27.7 26.9 26 28.1 27 ...
    ##  $ Perognathus    : num [1:24] 9.62 6.95 7.51 7.82 8.66 ...
    ##  $ Peromyscus     : num [1:24] 22.2 22.3 21.4 22.6 21.2 ...
    ##  $ Reithrodontomys: num [1:24] 11.4 10.7 10.5 10.3 11.2 ...
    ##  $ Sigmodon       : num [1:24] NA 70.9 65.6 82 82.7 ...
    ##  $ Spermophilus   : num [1:24] NA NA NA NA NA NA NA NA NA NA ...
    ##  - attr(*, "groups")= tibble [24 x 2] (S3: tbl_df/tbl/data.frame)
    ##   ..$ plot_id: num [1:24] 1 2 3 4 5 6 7 8 9 10 ...
    ##   ..$ .rows  : list<int> [1:24] 
    ##   .. ..$ : int 1
    ##   .. ..$ : int 2
    ##   .. ..$ : int 3
    ##   .. ..$ : int 4
    ##   .. ..$ : int 5
    ##   .. ..$ : int 6
    ##   .. ..$ : int 7
    ##   .. ..$ : int 8
    ##   .. ..$ : int 9
    ##   .. ..$ : int 10
    ##   .. ..$ : int 11
    ##   .. ..$ : int 12
    ##   .. ..$ : int 13
    ##   .. ..$ : int 14
    ##   .. ..$ : int 15
    ##   .. ..$ : int 16
    ##   .. ..$ : int 17
    ##   .. ..$ : int 18
    ##   .. ..$ : int 19
    ##   .. ..$ : int 20
    ##   .. ..$ : int 21
    ##   .. ..$ : int 22
    ##   .. ..$ : int 23
    ##   .. ..$ : int 24
    ##   .. ..@ ptype: int(0) 
    ##   ..- attr(*, ".drop")= logi TRUE

## Using pivot\_longer() to reshape the data frame to it’s original

``` r
# do not forget to select the columns that needs to be pivoted
survey_longer = survey_wider %>% 
  pivot_longer(2:11,names_to = "genus", values_to = "mean_weight")

survey_longer
```

    ## # A tibble: 240 x 3
    ## # Groups:   plot_id [24]
    ##    plot_id genus           mean_weight
    ##      <dbl> <chr>                 <dbl>
    ##  1       1 Baiomys                7   
    ##  2       1 Chaetodipus           22.2 
    ##  3       1 Dipodomys             60.2 
    ##  4       1 Neotoma              156.  
    ##  5       1 Onychomys             27.7 
    ##  6       1 Perognathus            9.62
    ##  7       1 Peromyscus            22.2 
    ##  8       1 Reithrodontomys       11.4 
    ##  9       1 Sigmodon              NA   
    ## 10       1 Spermophilus          NA   
    ## # ... with 230 more rows

## Spread the surveys data frame with year as columns, plot\_id as rows, and the number of genera per plot as the values.

``` r
survey_spread_genera = survey %>% 
  group_by(plot_id, year) %>% 
  summarise(n_genera = n_distinct(genus)) %>% 
  pivot_wider(names_from = plot_id, values_from = n_genera)
```

    ## `summarise()` regrouping output by 'plot_id' (override with `.groups` argument)

``` r
survey_spread_genera
```

    ## # A tibble: 26 x 25
    ##     year   `1`   `2`   `3`   `4`   `5`   `6`   `7`   `8`   `9`  `10`  `11`  `12`
    ##    <dbl> <int> <int> <int> <int> <int> <int> <int> <int> <int> <int> <int> <int>
    ##  1  1977     2     6     5     4     4     3     3     2     3     1     5     5
    ##  2  1978     3     6     6     4     3     4     1     4     3    NA     6     4
    ##  3  1979     4     6     4     3     2     3     3     3     3     2     5     4
    ##  4  1980     7     8     6     4     5     4     1     5     4     5     6     5
    ##  5  1981     5     5     6     5     4     5     1     6     5     5     8     7
    ##  6  1982     6     9     8     4     6     9     4     6     6     8     8    10
    ##  7  1983     7     9    10     6     7     9     2     4     7     4     9     9
    ##  8  1984     6     9    11     3     7     7     3     6     4     2     7     9
    ##  9  1985     4     6     7     4     3     5     4     4     5     3     5     8
    ## 10  1986     3     4     6     3     1     6     3     3     3     1     6     7
    ## # ... with 16 more rows, and 12 more variables: `13` <int>, `14` <int>,
    ## #   `15` <int>, `16` <int>, `17` <int>, `18` <int>, `19` <int>, `20` <int>,
    ## #   `21` <int>, `22` <int>, `23` <int>, `24` <int>

``` r
glimpse(survey_spread_genera)
```

    ## Rows: 26
    ## Columns: 25
    ## $ year <dbl> 1977, 1978, 1979, 1980, 1981, 1982, 1983, 1984, 1985, 1986, 19...
    ## $ `1`  <int> 2, 3, 4, 7, 5, 6, 7, 6, 4, 3, 7, 6, 8, 7, 7, 5, 5, 6, 5, 5, 7,...
    ## $ `2`  <int> 6, 6, 6, 8, 5, 9, 9, 9, 6, 4, 9, 7, 9, 8, 8, 8, 8, 5, 6, 7, 7,...
    ## $ `3`  <int> 5, 6, 4, 6, 6, 8, 10, 11, 7, 6, 8, 9, 9, 10, 9, 8, 6, 5, 6, 7,...
    ## $ `4`  <int> 4, 4, 3, 4, 5, 4, 6, 3, 4, 3, 7, 7, 6, 2, 8, 4, 3, 3, 4, 6, 8,...
    ## $ `5`  <int> 4, 3, 2, 5, 4, 6, 7, 7, 3, 1, 7, 8, 9, 7, 6, 6, 4, 5, 8, 7, 6,...
    ## $ `6`  <int> 3, 4, 3, 4, 5, 9, 9, 7, 5, 6, 6, 8, 7, 10, 5, 5, 5, 4, 7, 6, 1...
    ## $ `7`  <int> 3, 1, 3, 1, 1, 4, 2, 3, 4, 3, 6, 3, 4, 3, 5, 7, 4, 4, 5, 4, 5,...
    ## $ `8`  <int> 2, 4, 3, 5, 6, 6, 4, 6, 4, 3, 10, 7, 6, 4, 5, 4, 4, 6, 6, 6, 6...
    ## $ `9`  <int> 3, 3, 3, 4, 5, 6, 7, 4, 5, 3, 7, 7, 7, 5, 5, 7, 2, 4, 5, 7, 8,...
    ## $ `10` <int> 1, NA, 2, 5, 5, 8, 4, 2, 3, 1, 4, 5, 2, 2, 4, 1, 1, 2, 3, 2, 3...
    ## $ `11` <int> 5, 6, 5, 6, 8, 8, 9, 7, 5, 6, 11, 10, 8, 5, 6, 10, 4, 2, 6, 7,...
    ## $ `12` <int> 5, 4, 4, 5, 7, 10, 9, 9, 8, 7, 10, 7, 9, 9, 10, 8, 7, 3, 7, 9,...
    ## $ `13` <int> 3, 6, 5, 6, 5, 7, 7, 6, 4, 8, 10, 9, 8, 6, 9, 10, 9, 6, 8, 8, ...
    ## $ `14` <int> 3, 5, 4, 3, 6, 9, 6, 6, 7, 5, 8, 9, 9, 5, 5, 6, 6, 4, 5, 6, 8,...
    ## $ `15` <int> 2, 5, 5, 4, 9, 8, 8, 8, 5, 5, 9, 7, 6, 7, 8, 4, 6, 5, 7, 6, 8,...
    ## $ `16` <int> 3, 2, 2, 3, 3, 8, 5, 3, 4, 7, 4, 4, 3, 2, 2, 3, 5, 3, 4, 5, 5,...
    ## $ `17` <int> 4, 6, 4, 6, 7, 6, 8, 6, 6, 5, 7, 9, 8, 6, 7, 6, 7, 4, 7, 7, 8,...
    ## $ `18` <int> 2, 7, 4, 6, 4, 9, 5, 5, 5, 6, 6, 7, 7, 8, 9, 5, 5, 4, 4, 3, 7,...
    ## $ `19` <int> 4, 4, 5, 7, 6, 7, 6, 4, 8, 4, 4, 6, 9, 7, 10, 7, 5, 4, 4, 5, 5...
    ## $ `20` <int> 3, 6, 6, 7, 8, 10, 9, 6, 5, 7, 8, 9, 7, 8, 11, 8, 6, 6, 8, 6, ...
    ## $ `21` <int> 3, 4, 4, 6, 6, 7, 8, 4, 7, 5, 4, 6, 6, 5, 6, 5, 4, 3, 4, 6, 6,...
    ## $ `22` <int> 4, 4, 3, 6, 4, 5, 7, 5, 7, 4, 6, 7, 6, 4, 9, 5, 4, 3, 7, 4, 7,...
    ## $ `23` <int> 1, NA, 2, 4, 4, 9, 5, 5, 3, 5, 5, 8, 6, 5, 3, 2, 3, 2, 5, 5, 4...
    ## $ `24` <int> NA, NA, 4, 6, 5, 7, 9, 6, 8, 5, 9, 7, 8, 6, 7, 5, 4, 4, 3, 3, ...

## Evaluate the relationship of mean value of hindfoot\_length and mean of weight

``` r
survey_longer = survey_spread_genera %>% 
  pivot_longer(2:25, names_to = "plot_id", values_to = "n_genera")
  
survey_long = survey %>% 
  pivot_longer(c("hindfoot_length","weight"), names_to = "measurement", values_to = "value")
# other variant
survey_long = survey %>% 
  gather("measurement","value", hindfoot_length, weight)

survey_long
```

    ## # A tibble: 69,572 x 13
    ##    record_id month   day  year plot_id species_id sex   genus species taxa 
    ##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <fct> <fct> <chr>   <fct>
    ##  1         1     7    16  1977       2 NL         M     Neot~ albigu~ Rode~
    ##  2        72     8    19  1977       2 NL         M     Neot~ albigu~ Rode~
    ##  3       224     9    13  1977       2 NL         unde~ Neot~ albigu~ Rode~
    ##  4       266    10    16  1977       2 NL         unde~ Neot~ albigu~ Rode~
    ##  5       349    11    12  1977       2 NL         unde~ Neot~ albigu~ Rode~
    ##  6       363    11    12  1977       2 NL         unde~ Neot~ albigu~ Rode~
    ##  7       435    12    10  1977       2 NL         unde~ Neot~ albigu~ Rode~
    ##  8       506     1     8  1978       2 NL         unde~ Neot~ albigu~ Rode~
    ##  9       588     2    18  1978       2 NL         M     Neot~ albigu~ Rode~
    ## 10       661     3    11  1978       2 NL         unde~ Neot~ albigu~ Rode~
    ## # ... with 69,562 more rows, and 3 more variables: plot_type <chr>,
    ## #   measurement <chr>, value <dbl>

``` r
glimpse(survey_long)
```

    ## Rows: 69,572
    ## Columns: 13
    ## $ record_id   <dbl> 1, 72, 224, 266, 349, 363, 435, 506, 588, 661, 748, 845...
    ## $ month       <dbl> 7, 8, 9, 10, 11, 11, 12, 1, 2, 3, 4, 5, 6, 8, 9, 10, 11...
    ## $ day         <dbl> 16, 19, 13, 16, 12, 12, 10, 8, 18, 11, 8, 6, 9, 5, 4, 8...
    ## $ year        <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1978, 1978, 1...
    ## $ plot_id     <dbl> 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2, 2...
    ## $ species_id  <chr> "NL", "NL", "NL", "NL", "NL", "NL", "NL", "NL", "NL", "...
    ## $ sex         <fct> M, M, undetermined, undetermined, undetermined, undeter...
    ## $ genus       <fct> Neotoma, Neotoma, Neotoma, Neotoma, Neotoma, Neotoma, N...
    ## $ species     <chr> "albigula", "albigula", "albigula", "albigula", "albigu...
    ## $ taxa        <fct> Rodent, Rodent, Rodent, Rodent, Rodent, Rodent, Rodent,...
    ## $ plot_type   <chr> "Control", "Control", "Control", "Control", "Control", ...
    ## $ measurement <chr> "hindfoot_length", "hindfoot_length", "hindfoot_length"...
    ## $ value       <dbl> 32, 31, NA, NA, NA, NA, NA, NA, NA, NA, NA, 32, NA, 34,...

## With this new data set, calculate the average of each measurement in each year for each different plot\_type.

``` r
mean_eval = survey_long %>% 
  group_by(year, measurement, plot_type) %>% 
  summarise(
    mean_value = mean(value, na.rm = T)
  ) %>% 
  pivot_wider(names_from = measurement, values_from = mean_value)
```

    ## `summarise()` regrouping output by 'year', 'measurement' (override with `.groups` argument)

``` r
mean_eval = survey_long %>% 
  group_by(year, measurement, plot_type) %>% 
  summarise(
    mean_value = mean(value, na.rm = T)
  ) %>%
  spread(measurement, mean_value)
```

    ## `summarise()` regrouping output by 'year', 'measurement' (override with `.groups` argument)

``` r
mean_eval %>% flextable()
```

    ## PhantomJS not found. You can install it with webshot::install_phantomjs(). If it is installed, please make sure the phantomjs executable can be found via the PATH variable.

\#\# Export the data

``` r
# Clean the data first
# remove missing data from weight,hindfoot,sex columns
survey_complete = survey %>% 
  filter(!is.na(weight),
         !is.na(hindfoot_length),
         !is.na(sex))
```

## Remove observations for rare species (species\_id &gt; 50)

``` r
species_count = survey_complete %>% 
  count(species_id) %>% 
  filter(n >=50)

survey_complete = survey_complete %>% 
  filter(species_id %in% species_count$species_id)

survey_complete
```

    ## # A tibble: 30,521 x 13
    ##    record_id month   day  year plot_id species_id sex   hindfoot_length weight
    ##        <dbl> <dbl> <dbl> <dbl>   <dbl> <chr>      <fct>           <dbl>  <dbl>
    ##  1       845     5     6  1978       2 NL         M                  32    204
    ##  2      1164     8     5  1978       2 NL         M                  34    199
    ##  3      1261     9     4  1978       2 NL         M                  32    197
    ##  4      1756     4    29  1979       2 NL         M                  33    166
    ##  5      1818     5    30  1979       2 NL         M                  32    184
    ##  6      1882     7     4  1979       2 NL         M                  32    206
    ##  7      2133    10    25  1979       2 NL         F                  33    274
    ##  8      2184    11    17  1979       2 NL         F                  30    186
    ##  9      2406     1    16  1980       2 NL         F                  33    184
    ## 10      3000     5    18  1980       2 NL         F                  31     87
    ## # ... with 30,511 more rows, and 4 more variables: genus <fct>, species <chr>,
    ## #   taxa <fct>, plot_type <chr>

## Save the data in a csv file

``` r
write_csv(survey_complete, "survey_complete.csv")
```
