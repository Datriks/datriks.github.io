---
layout: post
title: "Data Manipulation with Dlookr"
subtitle: "Data Diagnostics"
background: '/img/posts/airquality/man-on-computer-keyboard.jpg'
---

Airquality Data Analysis
================

## Import the necessary libraries

``` r
library(tidyverse)
library(janitor)
library(dlookr)
library(lubridate)
#install.packages("openair")
library(openair)
library(flextable)
```

## Import the dirty data set use read.csv2 because data is sep by ;

``` r
dt = read.csv2("AirQualityUCI.csv", stringsAsFactors = F)
```

## Get a feel of the data

``` r
glimpse(dt)
```

    ## Rows: 9,471
    ## Columns: 17
    ## $ Date          <chr> "10/03/2004", "10/03/2004", "10/03/2004", "10/03/2004...
    ## $ Time          <chr> "18.00.00", "19.00.00", "20.00.00", "21.00.00", "22.0...
    ## $ CO.GT.        <dbl> 2.6, 2.0, 2.2, 2.2, 1.6, 1.2, 1.2, 1.0, 0.9, 0.6, -20...
    ## $ PT08.S1.CO.   <int> 1360, 1292, 1402, 1376, 1272, 1197, 1185, 1136, 1094,...
    ## $ NMHC.GT.      <int> 150, 112, 88, 80, 51, 38, 31, 31, 24, 19, 14, 8, 16, ...
    ## $ C6H6.GT.      <dbl> 11.9, 9.4, 9.0, 9.2, 6.5, 4.7, 3.6, 3.3, 2.3, 1.7, 1....
    ## $ PT08.S2.NMHC. <int> 1046, 955, 939, 948, 836, 750, 690, 672, 609, 561, 52...
    ## $ NOx.GT.       <int> 166, 103, 131, 172, 131, 89, 62, 62, 45, -200, 21, 16...
    ## $ PT08.S3.NOx.  <int> 1056, 1174, 1140, 1092, 1205, 1337, 1462, 1453, 1579,...
    ## $ NO2.GT.       <int> 113, 92, 114, 122, 116, 96, 77, 76, 60, -200, 34, 28,...
    ## $ PT08.S4.NO2.  <int> 1692, 1559, 1555, 1584, 1490, 1393, 1333, 1333, 1276,...
    ## $ PT08.S5.O3.   <int> 1268, 972, 1074, 1203, 1110, 949, 733, 730, 620, 501,...
    ## $ T             <dbl> 13.6, 13.3, 11.9, 11.0, 11.2, 11.2, 11.3, 10.7, 10.7,...
    ## $ RH            <dbl> 48.9, 47.7, 54.0, 60.0, 59.6, 59.2, 56.8, 60.0, 59.7,...
    ## $ AH            <dbl> 0.7578, 0.7255, 0.7502, 0.7867, 0.7888, 0.7848, 0.760...
    ## $ X             <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
    ## $ X.1           <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...

``` r
tail(dt, n =115)
```

    ##            Date     Time CO.GT. PT08.S1.CO. NMHC.GT. C6H6.GT. PT08.S2.NMHC.
    ## 9357 04/04/2005 14.00.00    2.2        1071     -200     11.9          1047
    ## 9358                         NA          NA       NA       NA            NA
    ## 9359                         NA          NA       NA       NA            NA
    ## 9360                         NA          NA       NA       NA            NA
    ## 9361                         NA          NA       NA       NA            NA
    ## 9362                         NA          NA       NA       NA            NA
    ## 9363                         NA          NA       NA       NA            NA
    ## 9364                         NA          NA       NA       NA            NA
    ## 9365                         NA          NA       NA       NA            NA
    ## 9366                         NA          NA       NA       NA            NA
    ## 9367                         NA          NA       NA       NA            NA
    ## 9368                         NA          NA       NA       NA            NA
    ## 9369                         NA          NA       NA       NA            NA
    ## 9370                         NA          NA       NA       NA            NA
    ## 9371                         NA          NA       NA       NA            NA
    ## 9372                         NA          NA       NA       NA            NA
    ## 9373                         NA          NA       NA       NA            NA
    ## 9374                         NA          NA       NA       NA            NA
    ## 9375                         NA          NA       NA       NA            NA
    ## 9376                         NA          NA       NA       NA            NA
    ## 9377                         NA          NA       NA       NA            NA
    ## 9378                         NA          NA       NA       NA            NA
    ## 9379                         NA          NA       NA       NA            NA
    ## 9380                         NA          NA       NA       NA            NA
    ## 9381                         NA          NA       NA       NA            NA
    ## 9382                         NA          NA       NA       NA            NA
    ## 9383                         NA          NA       NA       NA            NA
    ## 9384                         NA          NA       NA       NA            NA
    ## 9385                         NA          NA       NA       NA            NA
    ## 9386                         NA          NA       NA       NA            NA
    ## 9387                         NA          NA       NA       NA            NA
    ## 9388                         NA          NA       NA       NA            NA
    ## 9389                         NA          NA       NA       NA            NA
    ## 9390                         NA          NA       NA       NA            NA
    ## 9391                         NA          NA       NA       NA            NA
    ## 9392                         NA          NA       NA       NA            NA
    ## 9393                         NA          NA       NA       NA            NA
    ## 9394                         NA          NA       NA       NA            NA
    ## 9395                         NA          NA       NA       NA            NA
    ## 9396                         NA          NA       NA       NA            NA
    ## 9397                         NA          NA       NA       NA            NA
    ## 9398                         NA          NA       NA       NA            NA
    ## 9399                         NA          NA       NA       NA            NA
    ## 9400                         NA          NA       NA       NA            NA
    ## 9401                         NA          NA       NA       NA            NA
    ## 9402                         NA          NA       NA       NA            NA
    ## 9403                         NA          NA       NA       NA            NA
    ## 9404                         NA          NA       NA       NA            NA
    ## 9405                         NA          NA       NA       NA            NA
    ## 9406                         NA          NA       NA       NA            NA
    ## 9407                         NA          NA       NA       NA            NA
    ## 9408                         NA          NA       NA       NA            NA
    ## 9409                         NA          NA       NA       NA            NA
    ## 9410                         NA          NA       NA       NA            NA
    ## 9411                         NA          NA       NA       NA            NA
    ## 9412                         NA          NA       NA       NA            NA
    ## 9413                         NA          NA       NA       NA            NA
    ## 9414                         NA          NA       NA       NA            NA
    ## 9415                         NA          NA       NA       NA            NA
    ## 9416                         NA          NA       NA       NA            NA
    ## 9417                         NA          NA       NA       NA            NA
    ## 9418                         NA          NA       NA       NA            NA
    ## 9419                         NA          NA       NA       NA            NA
    ## 9420                         NA          NA       NA       NA            NA
    ## 9421                         NA          NA       NA       NA            NA
    ## 9422                         NA          NA       NA       NA            NA
    ## 9423                         NA          NA       NA       NA            NA
    ## 9424                         NA          NA       NA       NA            NA
    ## 9425                         NA          NA       NA       NA            NA
    ## 9426                         NA          NA       NA       NA            NA
    ## 9427                         NA          NA       NA       NA            NA
    ## 9428                         NA          NA       NA       NA            NA
    ## 9429                         NA          NA       NA       NA            NA
    ## 9430                         NA          NA       NA       NA            NA
    ## 9431                         NA          NA       NA       NA            NA
    ## 9432                         NA          NA       NA       NA            NA
    ## 9433                         NA          NA       NA       NA            NA
    ## 9434                         NA          NA       NA       NA            NA
    ## 9435                         NA          NA       NA       NA            NA
    ## 9436                         NA          NA       NA       NA            NA
    ## 9437                         NA          NA       NA       NA            NA
    ## 9438                         NA          NA       NA       NA            NA
    ## 9439                         NA          NA       NA       NA            NA
    ## 9440                         NA          NA       NA       NA            NA
    ## 9441                         NA          NA       NA       NA            NA
    ## 9442                         NA          NA       NA       NA            NA
    ## 9443                         NA          NA       NA       NA            NA
    ## 9444                         NA          NA       NA       NA            NA
    ## 9445                         NA          NA       NA       NA            NA
    ## 9446                         NA          NA       NA       NA            NA
    ## 9447                         NA          NA       NA       NA            NA
    ## 9448                         NA          NA       NA       NA            NA
    ## 9449                         NA          NA       NA       NA            NA
    ## 9450                         NA          NA       NA       NA            NA
    ## 9451                         NA          NA       NA       NA            NA
    ## 9452                         NA          NA       NA       NA            NA
    ## 9453                         NA          NA       NA       NA            NA
    ## 9454                         NA          NA       NA       NA            NA
    ## 9455                         NA          NA       NA       NA            NA
    ## 9456                         NA          NA       NA       NA            NA
    ## 9457                         NA          NA       NA       NA            NA
    ## 9458                         NA          NA       NA       NA            NA
    ## 9459                         NA          NA       NA       NA            NA
    ## 9460                         NA          NA       NA       NA            NA
    ## 9461                         NA          NA       NA       NA            NA
    ## 9462                         NA          NA       NA       NA            NA
    ## 9463                         NA          NA       NA       NA            NA
    ## 9464                         NA          NA       NA       NA            NA
    ## 9465                         NA          NA       NA       NA            NA
    ## 9466                         NA          NA       NA       NA            NA
    ## 9467                         NA          NA       NA       NA            NA
    ## 9468                         NA          NA       NA       NA            NA
    ## 9469                         NA          NA       NA       NA            NA
    ## 9470                         NA          NA       NA       NA            NA
    ## 9471                         NA          NA       NA       NA            NA
    ##      NOx.GT. PT08.S3.NOx. NO2.GT. PT08.S4.NO2. PT08.S5.O3.    T   RH     AH  X
    ## 9357     265          654     168         1129         816 28.5 13.1 0.5028 NA
    ## 9358      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9359      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9360      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9361      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9362      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9363      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9364      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9365      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9366      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9367      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9368      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9369      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9370      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9371      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9372      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9373      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9374      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9375      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9376      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9377      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9378      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9379      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9380      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9381      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9382      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9383      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9384      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9385      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9386      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9387      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9388      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9389      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9390      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9391      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9392      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9393      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9394      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9395      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9396      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9397      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9398      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9399      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9400      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9401      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9402      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9403      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9404      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9405      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9406      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9407      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9408      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9409      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9410      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9411      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9412      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9413      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9414      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9415      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9416      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9417      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9418      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9419      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9420      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9421      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9422      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9423      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9424      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9425      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9426      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9427      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9428      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9429      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9430      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9431      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9432      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9433      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9434      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9435      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9436      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9437      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9438      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9439      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9440      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9441      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9442      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9443      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9444      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9445      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9446      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9447      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9448      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9449      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9450      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9451      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9452      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9453      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9454      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9455      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9456      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9457      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9458      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9459      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9460      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9461      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9462      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9463      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9464      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9465      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9466      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9467      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9468      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9469      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9470      NA           NA      NA           NA          NA   NA   NA     NA NA
    ## 9471      NA           NA      NA           NA          NA   NA   NA     NA NA
    ##      X.1
    ## 9357  NA
    ## 9358  NA
    ## 9359  NA
    ## 9360  NA
    ## 9361  NA
    ## 9362  NA
    ## 9363  NA
    ## 9364  NA
    ## 9365  NA
    ## 9366  NA
    ## 9367  NA
    ## 9368  NA
    ## 9369  NA
    ## 9370  NA
    ## 9371  NA
    ## 9372  NA
    ## 9373  NA
    ## 9374  NA
    ## 9375  NA
    ## 9376  NA
    ## 9377  NA
    ## 9378  NA
    ## 9379  NA
    ## 9380  NA
    ## 9381  NA
    ## 9382  NA
    ## 9383  NA
    ## 9384  NA
    ## 9385  NA
    ## 9386  NA
    ## 9387  NA
    ## 9388  NA
    ## 9389  NA
    ## 9390  NA
    ## 9391  NA
    ## 9392  NA
    ## 9393  NA
    ## 9394  NA
    ## 9395  NA
    ## 9396  NA
    ## 9397  NA
    ## 9398  NA
    ## 9399  NA
    ## 9400  NA
    ## 9401  NA
    ## 9402  NA
    ## 9403  NA
    ## 9404  NA
    ## 9405  NA
    ## 9406  NA
    ## 9407  NA
    ## 9408  NA
    ## 9409  NA
    ## 9410  NA
    ## 9411  NA
    ## 9412  NA
    ## 9413  NA
    ## 9414  NA
    ## 9415  NA
    ## 9416  NA
    ## 9417  NA
    ## 9418  NA
    ## 9419  NA
    ## 9420  NA
    ## 9421  NA
    ## 9422  NA
    ## 9423  NA
    ## 9424  NA
    ## 9425  NA
    ## 9426  NA
    ## 9427  NA
    ## 9428  NA
    ## 9429  NA
    ## 9430  NA
    ## 9431  NA
    ## 9432  NA
    ## 9433  NA
    ## 9434  NA
    ## 9435  NA
    ## 9436  NA
    ## 9437  NA
    ## 9438  NA
    ## 9439  NA
    ## 9440  NA
    ## 9441  NA
    ## 9442  NA
    ## 9443  NA
    ## 9444  NA
    ## 9445  NA
    ## 9446  NA
    ## 9447  NA
    ## 9448  NA
    ## 9449  NA
    ## 9450  NA
    ## 9451  NA
    ## 9452  NA
    ## 9453  NA
    ## 9454  NA
    ## 9455  NA
    ## 9456  NA
    ## 9457  NA
    ## 9458  NA
    ## 9459  NA
    ## 9460  NA
    ## 9461  NA
    ## 9462  NA
    ## 9463  NA
    ## 9464  NA
    ## 9465  NA
    ## 9466  NA
    ## 9467  NA
    ## 9468  NA
    ## 9469  NA
    ## 9470  NA
    ## 9471  NA

## Remove the last 2 columns containing NAs and all the rows containing NAs

``` r
dt = dt %>% 
  clean_names() %>% 
  remove_empty() %>% 
  drop_na()

view(dt)
```

## Save to a csv file

``` r
# save the data set in a new csv file
#write.csv(dt, "juvAirquality.csv")

# Replacing -200 value in the dataset with NA
dt[dt == -200] = NA
```

## Format the feature time as a date time

``` r
dt = dt %>% 
  mutate(
    DateTime = as_datetime(dmy_hms(paste(date, time))),
    )
colnames(dt)
```

    ##  [1] "date"         "time"         "co_gt"        "pt08_s1_co"   "nmhc_gt"     
    ##  [6] "c6h6_gt"      "pt08_s2_nmhc" "n_ox_gt"      "pt08_s3_n_ox" "no2_gt"      
    ## [11] "pt08_s4_no2"  "pt08_s5_o3"   "t"            "rh"           "ah"          
    ## [16] "DateTime"

``` r
glimpse(dt)
```

    ## Rows: 9,357
    ## Columns: 16
    ## $ date         <chr> "10/03/2004", "10/03/2004", "10/03/2004", "10/03/2004"...
    ## $ time         <chr> "18.00.00", "19.00.00", "20.00.00", "21.00.00", "22.00...
    ## $ co_gt        <dbl> 2.6, 2.0, 2.2, 2.2, 1.6, 1.2, 1.2, 1.0, 0.9, 0.6, NA, ...
    ## $ pt08_s1_co   <int> 1360, 1292, 1402, 1376, 1272, 1197, 1185, 1136, 1094, ...
    ## $ nmhc_gt      <int> 150, 112, 88, 80, 51, 38, 31, 31, 24, 19, 14, 8, 16, 2...
    ## $ c6h6_gt      <dbl> 11.9, 9.4, 9.0, 9.2, 6.5, 4.7, 3.6, 3.3, 2.3, 1.7, 1.3...
    ## $ pt08_s2_nmhc <int> 1046, 955, 939, 948, 836, 750, 690, 672, 609, 561, 527...
    ## $ n_ox_gt      <int> 166, 103, 131, 172, 131, 89, 62, 62, 45, NA, 21, 16, 3...
    ## $ pt08_s3_n_ox <int> 1056, 1174, 1140, 1092, 1205, 1337, 1462, 1453, 1579, ...
    ## $ no2_gt       <int> 113, 92, 114, 122, 116, 96, 77, 76, 60, NA, 34, 28, 48...
    ## $ pt08_s4_no2  <int> 1692, 1559, 1555, 1584, 1490, 1393, 1333, 1333, 1276, ...
    ## $ pt08_s5_o3   <int> 1268, 972, 1074, 1203, 1110, 949, 733, 730, 620, 501, ...
    ## $ t            <dbl> 13.6, 13.3, 11.9, 11.0, 11.2, 11.2, 11.3, 10.7, 10.7, ...
    ## $ rh           <dbl> 48.9, 47.7, 54.0, 60.0, 59.6, 59.2, 56.8, 60.0, 59.7, ...
    ## $ ah           <dbl> 0.7578, 0.7255, 0.7502, 0.7867, 0.7888, 0.7848, 0.7603...
    ## $ DateTime     <dttm> 2004-03-10 18:00:00, 2004-03-10 19:00:00, 2004-03-10 ...

## Rename the column to a more conventional format

``` r
dt.final = dt %>% 
  select("date" = "DateTime",
         "co" = "co_gt",
         "co_s" = "pt08_s1_co", 
         "NMHC" = "nmhc_gt",
         "NMHC_s" = "pt08_s2_nmhc",
         "c6h6" = "c6h6_gt", 
         "nox" = "n_ox_gt",
         "nox_s" = "pt08_s3_n_ox",
         "no2" = "no2_gt",
         "no2_s" = "pt08_s4_no2",
         "o3_s" = "pt08_s5_o3", t,rh,ah)
         #"t" = "temp",
         #"rh" = "RH%",
         #"ah" = "AH")
```

## Get a feel of the data

``` r
glimpse(dt.final)
```

    ## Rows: 9,357
    ## Columns: 14
    ## $ date   <dttm> 2004-03-10 18:00:00, 2004-03-10 19:00:00, 2004-03-10 20:00:...
    ## $ co     <dbl> 2.6, 2.0, 2.2, 2.2, 1.6, 1.2, 1.2, 1.0, 0.9, 0.6, NA, 0.7, 0...
    ## $ co_s   <int> 1360, 1292, 1402, 1376, 1272, 1197, 1185, 1136, 1094, 1010, ...
    ## $ NMHC   <int> 150, 112, 88, 80, 51, 38, 31, 31, 24, 19, 14, 8, 16, 29, 64,...
    ## $ NMHC_s <int> 1046, 955, 939, 948, 836, 750, 690, 672, 609, 561, 527, 512,...
    ## $ c6h6   <dbl> 11.9, 9.4, 9.0, 9.2, 6.5, 4.7, 3.6, 3.3, 2.3, 1.7, 1.3, 1.1,...
    ## $ nox    <int> 166, 103, 131, 172, 131, 89, 62, 62, 45, NA, 21, 16, 34, 98,...
    ## $ nox_s  <int> 1056, 1174, 1140, 1092, 1205, 1337, 1462, 1453, 1579, 1705, ...
    ## $ no2    <int> 113, 92, 114, 122, 116, 96, 77, 76, 60, NA, 34, 28, 48, 82, ...
    ## $ no2_s  <int> 1692, 1559, 1555, 1584, 1490, 1393, 1333, 1333, 1276, 1235, ...
    ## $ o3_s   <int> 1268, 972, 1074, 1203, 1110, 949, 733, 730, 620, 501, 445, 4...
    ## $ t      <dbl> 13.6, 13.3, 11.9, 11.0, 11.2, 11.2, 11.3, 10.7, 10.7, 10.3, ...
    ## $ rh     <dbl> 48.9, 47.7, 54.0, 60.0, 59.6, 59.2, 56.8, 60.0, 59.7, 60.2, ...
    ## $ ah     <dbl> 0.7578, 0.7255, 0.7502, 0.7867, 0.7888, 0.7848, 0.7603, 0.77...

``` r
colnames(dt.final)
```

    ##  [1] "date"   "co"     "co_s"   "NMHC"   "NMHC_s" "c6h6"   "nox"    "nox_s" 
    ##  [9] "no2"    "no2_s"  "o3_s"   "t"      "rh"     "ah"

``` r
view(dt.final)
```

## Summary plot for the data set

``` r
dt.final %>% 
  summaryPlot(period = "months")
```

    ##     date1     date2        co      co_s      NMHC    NMHC_s      c6h6       nox 
    ## "POSIXct"  "POSIXt" "numeric" "integer" "integer" "integer" "numeric" "integer" 
    ##     nox_s       no2     no2_s      o3_s         t        rh        ah 
    ## "integer" "integer" "integer" "integer" "numeric" "numeric" "numeric"

![](/unnamed-chunk-9-1.png)<!-- -->

## Trend Level Analysis

``` r
dt.final %>% 
  trendLevel(pollutant = 'ah', auto.text = T, main = 'ah')
```

![](/unnamed-chunk-10-1.png)<!-- -->

## Summary plot for the data set

``` r
dt.final %>% 
  trendLevel(pollutant = 'no2_s', auto.text = T, main = 'no2_s')
```

![](/unnamed-chunk-11-1.png)<!-- -->

## Time Variation

``` r
timeVariation(dt.final, pollutant = 'co', auto.text = TRUE, main = 'CO TimeVariation plot')
```

![](/unnamed-chunk-12-1.png)<!-- -->

``` r
timeVariation(dt.final, pollutant = 'c6h6', auto.text = TRUE, main = 'C6H6 TimeVariation plot')
```

![](/unnamed-chunk-12-2.png)<!-- -->

``` r
timeVariation(dt.final, pollutant = 'nox', auto.text = TRUE, main = 'NOx TimeVariation plot')
```

![](/unnamed-chunk-12-3.png)<!-- -->

``` r
timeVariation(dt.final, pollutant = 'no2', auto.text = TRUE, main = 'NO2 TimeVariation plot')
```

![](/unnamed-chunk-12-4.png)<!-- -->

## More in depth analysis witk dlookr- diagnose

``` r
diagnose(dt.final) %>% flextable()
```

``` r
diagnose_numeric(dt.final) %>% flextable()
```

``` r
diagnose_outlier(dt.final) %>% flextable()
```

## Plot outliers and NAs distribution

``` r
plot_outlier(dt.final)
```

![](Airquality-data-manipulation_files/figure-gf/unnamed-chunk-14-2.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-3.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-4.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-5.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-6.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-7.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-8.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-9.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-10.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-11.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-12.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-13.png)<!-- -->![](Airquality-data-manipulation_files/figure-gfm/unnamed-chunk-14-14.png)<!-- -->

``` r
plot_na_pareto(dt.final)
```
![](/unnamed-chunk-14-15.png)<!-- -->

``` r
plot_na_pareto(dt.final, only_na = T)
```

![](/img/posts/airquality/unnamed-chunk-14-16.png)<!-- -->

``` r
plot_na_pareto(dt.final, only_na = T, plot = F) %>% flextable()
```

## Distribution of missing values by combination of variables

``` r
#plot_na_hclust(dt.final)
```

## Missing with intersection of variables

``` r
# plot_na_intersect(dt.final)

# diagnose_report(dt.final, output_format = "html")
```
