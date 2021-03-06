---
layout: post
title: "Web Scraping NBA"
subtitle: "Data Scraping and Data Munging"
background: '/img/posts/web_scrap-nba/web-scrap-nba-datriks.jpg'
---

Web-Scraping NBA
================

## Import the necessary packages

``` r
library(tidyverse) # manipulation, visualisation, tidying data
library(janitor) # cleaning the data
library(rvest) # scrapping the web data
#install.packages("hablar") # converts data type
library(hablar)
library(flextable)
```

## Declaring the url of the table to be imported

``` r
url = "https://www.basketball-reference.com/allstar/NBA-allstar-career-stats.html"
# create a var pageobj to read the url address and transform the content into a data frame
pageobj = read_html(url, as.tibble = T)

# create a variable ALLStarPlayes that is holding the content of the first table from the webpage we imported anterior.
AllStarsPlayers = pageobj %>% 
  html_nodes("table") %>% .[[1]] %>% 
  html_table(fill = T)

view(AllStarsPlayers)
glimpse(AllStarsPlayers)
```

    ## Rows: 469
    ## Columns: 30
    ## $ ``         <chr> "Player", "Kareem Abdul-Jabbar", "LeBron James", "Kobe B...
    ## $ ``         <chr> "G", "18", "16", "15", "15", "14", "14", "13", "13", "13...
    ## $ ``         <chr> "GS", "13", "16", "15", "12", "11", "2", "13", "9", "", ...
    ## $ ``         <chr> "MP", "449", "460", "415", "311", "287", "227", "382", "...
    ## $ ``         <chr> "FG", "105", "159", "119", "63", "72", "49", "110", "72"...
    ## $ ``         <chr> "FGA", "213", "303", "238", "115", "141", "109", "233", ...
    ## $ ``         <chr> "FG%", ".493", ".525", ".500", ".548", ".511", ".450", "...
    ## $ ``         <chr> "3P", "0", "38", "22", "1", "0", "10", "3", "", "", "", ...
    ## $ ``         <chr> "3PA", "1", "116", "68", "4", "4", "34", "11", "", "", "...
    ## $ ``         <chr> "3P%", ".000", ".328", ".324", ".250", ".000", ".294", "...
    ## $ ``         <chr> "2P", "105", "121", "97", "62", "72", "39", "107", "72",...
    ## $ ``         <chr> "2PA", "212", "187", "170", "111", "137", "75", "222", "...
    ## $ ``         <chr> "2P%", ".495", ".647", ".571", ".559", ".526", ".520", "...
    ## $ ``         <chr> "FT", "41", "29", "30", "13", "14", "14", "39", "47", "4...
    ## $ ``         <chr> "FTA", "50", "40", "38", "17", "16", "16", "52", "94", "...
    ## $ ``         <chr> "FT%", ".820", ".725", ".789", ".765", ".875", ".875", "...
    ## $ ``         <chr> "ORB", "33", "13", "28", "38", "25", "15", "22", "", "",...
    ## $ ``         <chr> "DRB", "84", "88", "47", "98", "63", "37", "39", "", "",...
    ## $ ``         <chr> "TRB", "149", "101", "75", "136", "88", "52", "61", "197...
    ## $ ``         <chr> "AST", "51", "94", "70", "31", "40", "16", "54", "36", "...
    ## $ ``         <chr> "STL", "6", "19", "38", "13", "16", "10", "37", "", "", ...
    ## $ ``         <chr> "BLK", "31", "6", "6", "8", "11", "5", "6", "", "", "0",...
    ## $ ``         <chr> "TOV", "28", "54", "35", "31", "20", "14", "42", "", "",...
    ## $ ``         <chr> "PF", "57", "18", "35", "16", "10", "8", "31", "23", "27...
    ## $ ``         <chr> "PTS", "251", "385", "290", "140", "158", "122", "262", ...
    ## $ ``         <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ `Per Game` <chr> "MP", "24.9", "28.7", "27.7", "20.7", "20.5", "16.2", "2...
    ## $ `Per Game` <chr> "PTS", "13.9", "24.1", "19.3", "9.3", "11.3", "8.7", "20...
    ## $ `Per Game` <chr> "TRB", "8.3", "6.3", "5.0", "9.1", "6.3", "3.7", "4.7", ...
    ## $ `Per Game` <chr> "AST", "2.8", "5.9", "4.7", "2.1", "2.9", "1.1", "4.2", ...

``` r
# Notice for All-Star Player stats there is a single data table on the website we specified. Therefore, as shown above we referenced the page object as [[1]] indicating it is the first (in this case only) data table on the site.
```

## Data Cleaning: Updating Variable Names

``` r
# The janitor package offers two functions that help us with the cleaning proocess:
# Converts all row names to follow tidyverse style requirements
# column names are changed from capital letters to small letters
AllStarsPlayers = AllStarsPlayers %>% 
  row_to_names(row_number = 1, remove_row = T, remove_rows_above = F) %>% 
  clean_names()

view(AllStarsPlayers)
names(AllStarsPlayers)
```

    ##  [1] "player"      "g"           "gs"          "mp"          "fg"         
    ##  [6] "fga"         "fg_percent"  "x3p"         "x3pa"        "x3p_percent"
    ## [11] "x2p"         "x2pa"        "x2p_percent" "ft"          "fta"        
    ## [16] "ft_percent"  "orb"         "drb"         "trb"         "ast"        
    ## [21] "stl"         "blk"         "tov"         "pf"          "pts"        
    ## [26] "na"          "mp_2"        "pts_2"       "trb_2"       "ast_2"

``` r
# In AllStarPlayers ast: is the total number of assists across all games
# In AllStarPlayers ast2: is the average number of assists per game
# In ONeal ast: is the average number of assists per game
# To avoid confussion we have to create column names that are more descriptive
AllStarsPlayers = AllStarsPlayers %>% 
  rename("mins_total" = "mp",
         "pts_total" = "pts",
         "trb_total" = "trb",
         "ast_total" = "ast",
         "mins_per_game" = "mp_2",
         "pts_per_game" = "pts_2",
         "trb_per_game" = "trb_2",
         "ast_per_game" = "ast_2")

names(AllStarsPlayers)
```

    ##  [1] "player"        "g"             "gs"            "mins_total"   
    ##  [5] "fg"            "fga"           "fg_percent"    "x3p"          
    ##  [9] "x3pa"          "x3p_percent"   "x2p"           "x2pa"         
    ## [13] "x2p_percent"   "ft"            "fta"           "ft_percent"   
    ## [17] "orb"           "drb"           "trb_total"     "ast_total"    
    ## [21] "stl"           "blk"           "tov"           "pf"           
    ## [25] "pts_total"     "na"            "mins_per_game" "pts_per_game" 
    ## [29] "trb_per_game"  "ast_per_game"

## Data Cleaning: Remove Subheadings

``` r
# The AllStarPlayers dataset has multiple rows that repeat the variable heading names. This is a useful feature when scrolling through large datasets online, as there is always a row displayed on the screen to reference.

get_dupes(AllStarsPlayers) %>% flextable()
```

``` r
AllStarsPlayers = AllStarsPlayers %>% 
  filter(
    player != "Player" & player != ""
  )

view(AllStarsPlayers)
```

## Create New Variables

``` r
# verify the the feature characteristics
glimpse(AllStarsPlayers)
```

    ## Rows: 426
    ## Columns: 30
    ## $ player        <chr> "Kareem Abdul-Jabbar", "LeBron James", "Kobe Bryant",...
    ## $ g             <chr> "18", "16", "15", "15", "14", "14", "13", "13", "13",...
    ## $ gs            <chr> "13", "16", "15", "12", "11", "2", "13", "9", "", "10...
    ## $ mins_total    <chr> "449", "460", "415", "311", "287", "227", "382", "388...
    ## $ fg            <chr> "105", "159", "119", "63", "72", "49", "110", "72", "...
    ## $ fga           <chr> "213", "303", "238", "115", "141", "109", "233", "122...
    ## $ fg_percent    <chr> ".493", ".525", ".500", ".548", ".511", ".450", ".472...
    ## $ x3p           <chr> "0", "38", "22", "1", "0", "10", "3", "", "", "", "1"...
    ## $ x3pa          <chr> "1", "116", "68", "4", "4", "34", "11", "", "", "", "...
    ## $ x3p_percent   <chr> ".000", ".328", ".324", ".250", ".000", ".294", ".273...
    ## $ x2p           <chr> "105", "121", "97", "62", "72", "39", "107", "72", "5...
    ## $ x2pa          <chr> "212", "187", "170", "111", "137", "75", "222", "122"...
    ## $ x2p_percent   <chr> ".495", ".647", ".571", ".559", ".526", ".520", ".482...
    ## $ ft            <chr> "41", "29", "30", "13", "14", "14", "39", "47", "43",...
    ## $ fta           <chr> "50", "40", "38", "17", "16", "16", "52", "94", "51",...
    ## $ ft_percent    <chr> ".820", ".725", ".789", ".765", ".875", ".875", ".750...
    ## $ orb           <chr> "33", "13", "28", "38", "25", "15", "22", "", "", "2"...
    ## $ drb           <chr> "84", "88", "47", "98", "63", "37", "39", "", "", "10...
    ## $ trb_total     <chr> "149", "101", "75", "136", "88", "52", "61", "197", "...
    ## $ ast_total     <chr> "51", "94", "70", "31", "40", "16", "54", "36", "86",...
    ## $ stl           <chr> "6", "19", "38", "13", "16", "10", "37", "", "", "4",...
    ## $ blk           <chr> "31", "6", "6", "8", "11", "5", "6", "", "", "0", "23...
    ## $ tov           <chr> "28", "54", "35", "31", "20", "14", "42", "", "", "4"...
    ## $ pf            <chr> "57", "18", "35", "16", "10", "8", "31", "23", "27", ...
    ## $ pts_total     <chr> "251", "385", "290", "140", "158", "122", "262", "191...
    ## $ na            <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
    ## $ mins_per_game <chr> "24.9", "28.7", "27.7", "20.7", "20.5", "16.2", "29.4...
    ## $ pts_per_game  <chr> "13.9", "24.1", "19.3", "9.3", "11.3", "8.7", "20.2",...
    ## $ trb_per_game  <chr> "8.3", "6.3", "5.0", "9.1", "6.3", "3.7", "4.7", "15....
    ## $ ast_per_game  <chr> "2.8", "5.9", "4.7", "2.1", "2.9", "1.1", "4.2", "2.8...

``` r
# As one can see all the columns has been imported by rvest as characters we have now to work on converting them into integers and numeric
AllStarsPlayers = AllStarsPlayers %>% 
  convert(int("g", "gs", "mins_total", "fg", "fga", "x3p", "x3pa", "x2p",    
      "x2pa", "ft", "fta", "orb", "drb", "trb_total", "ast_total",  
      "stl", "blk", "tov", "pf", "mins_per_game", "pts_per_game",    
      "trb_per_game", "ast_per_game"),
      num("fg_percent", "x3p_percent", "x2p_percent", "ft_percent"))
# evaluate the transformation
glimpse(AllStarsPlayers)
```

    ## Rows: 426
    ## Columns: 30
    ## $ player        <chr> "Kareem Abdul-Jabbar", "LeBron James", "Kobe Bryant",...
    ## $ g             <int> 18, 16, 15, 15, 14, 14, 13, 13, 13, 13, 12, 12, 12, 1...
    ## $ gs            <int> 13, 16, 15, 12, 11, 2, 13, 9, NA, 10, 8, 11, 8, 9, 10...
    ## $ mins_total    <int> 449, 460, 415, 311, 287, 227, 382, 388, NA, 303, 270,...
    ## $ fg            <int> 105, 159, 119, 63, 72, 49, 110, 72, 52, 74, 45, 62, 5...
    ## $ fga           <int> 213, 303, 238, 115, 141, 109, 233, 122, 158, 154, 110...
    ## $ fg_percent    <dbl> 0.493, 0.525, 0.500, 0.548, 0.511, 0.450, 0.472, 0.59...
    ## $ x3p           <int> 0, 38, 22, 1, 0, 10, 3, NA, NA, NA, 1, NA, 0, 0, NA, ...
    ## $ x3pa          <int> 1, 116, 68, 4, 4, 34, 11, NA, NA, NA, 1, NA, 0, 2, NA...
    ## $ x3p_percent   <dbl> 0.000, 0.328, 0.324, 0.250, 0.000, 0.294, 0.273, NA, ...
    ## $ x2p           <int> 105, 121, 97, 62, 72, 39, 107, 72, 52, 74, 44, 62, 58...
    ## $ x2pa          <int> 212, 187, 170, 111, 137, 75, 222, 122, 158, 154, 109,...
    ## $ x2p_percent   <dbl> 0.495, 0.647, 0.571, 0.559, 0.526, 0.520, 0.482, 0.59...
    ## $ ft            <int> 41, 29, 30, 13, 14, 14, 39, 47, 43, 31, 26, 36, 29, 2...
    ## $ fta           <int> 50, 40, 38, 17, 16, 16, 52, 94, 51, 41, 50, 50, 40, 6...
    ## $ ft_percent    <dbl> 0.820, 0.725, 0.789, 0.765, 0.875, 0.875, 0.750, 0.50...
    ## $ orb           <int> 33, 13, 28, 38, 25, 15, 22, NA, NA, 2, 38, NA, 19, 39...
    ## $ drb           <int> 84, 88, 47, 98, 63, 37, 39, NA, NA, 10, 56, NA, 55, 5...
    ## $ trb_total     <int> 149, 101, 75, 136, 88, 52, 61, 197, 78, 46, 94, 47, 7...
    ## $ ast_total     <int> 51, 94, 70, 31, 40, 16, 54, 36, 86, 31, 17, 55, 19, 1...
    ## $ stl           <int> 6, 19, 38, 13, 16, 10, 37, NA, NA, 4, 15, NA, 12, 13,...
    ## $ blk           <int> 31, 6, 6, 8, 11, 5, 6, NA, NA, 0, 23, NA, 5, 19, NA, ...
    ## $ tov           <int> 28, 54, 35, 31, 20, 14, 42, NA, NA, 4, 26, NA, 15, 22...
    ## $ pf            <int> 57, 18, 35, 16, 10, 8, 31, 23, 27, 20, 31, 28, 16, 29...
    ## $ pts_total     <chr> "251", "385", "290", "140", "158", "122", "262", "191...
    ## $ na            <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, N...
    ## $ mins_per_game <int> 24, 28, 27, 20, 20, 16, 29, 29, NA, 23, 22, 28, 20, 2...
    ## $ pts_per_game  <int> 13, 24, 19, 9, 11, 8, 20, 14, 11, 13, 9, 13, 12, 16, ...
    ## $ trb_per_game  <int> 8, 6, 5, 9, 6, 3, 4, 15, 6, 3, 7, 3, 6, 8, 5, 3, 7, 1...
    ## $ ast_per_game  <int> 2, 5, 4, 2, 2, 1, 4, 2, 6, 2, 1, 4, 1, 1, 6, 4, 1, 3,...

``` r
# Other data sets, especially those for individual players, calculate the stats as averages per-game. Having variables on the same scale allows us to merge and compare data.
# Converting the totals to per-game averages can be done using traditional math operators.

AllStarsPlayers = AllStarsPlayers %>% 
  mutate(
    fg_per_game = fg / g,
    fga_per_game = fga / g,
    x3p_per_game = x3p / g
  ) %>% 
  rename(
    "fg_total" = "fg",
    "fga_total" = "fga",
    "x3p_total" = "x3p"
  )

names(AllStarsPlayers)
```

    ##  [1] "player"        "g"             "gs"            "mins_total"   
    ##  [5] "fg_total"      "fga_total"     "fg_percent"    "x3p_total"    
    ##  [9] "x3pa"          "x3p_percent"   "x2p"           "x2pa"         
    ## [13] "x2p_percent"   "ft"            "fta"           "ft_percent"   
    ## [17] "orb"           "drb"           "trb_total"     "ast_total"    
    ## [21] "stl"           "blk"           "tov"           "pf"           
    ## [25] "pts_total"     "na"            "mins_per_game" "pts_per_game" 
    ## [29] "trb_per_game"  "ast_per_game"  "fg_per_game"   "fga_per_game" 
    ## [33] "x3p_per_game"

## Import the O’Neal Data set

``` r
# put the link into a variable
url1 = "https://www.basketball-reference.com/players/o/onealsh01.html"
# read the url link with rvest
pageobj1 = read_html(url1, as.tibble = T)
# extract the necessary table by specifying the nr of the table on the web page
Oneal = pageobj1 %>% html_nodes("table") %>% .[[1]] %>% html_table()
# get a glimpse of the dta imported
glimpse(Oneal)
```

    ## Rows: 29
    ## Columns: 30
    ## $ Season <chr> "1992-93", "1993-94", "1994-95", "1995-96", "1996-97", "1997...
    ## $ Age    <int> 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, ...
    ## $ Tm     <chr> "ORL", "ORL", "ORL", "ORL", "LAL", "LAL", "LAL", "LAL", "LAL...
    ## $ Lg     <chr> "NBA", "NBA", "NBA", "NBA", "NBA", "NBA", "NBA", "NBA", "NBA...
    ## $ Pos    <chr> "C", "C", "C", "C", "C", "C", "C", "C", "C", "C", "C", "C", ...
    ## $ G      <int> 81, 81, 79, 54, 51, 60, 49, 79, 74, 67, 67, 67, 73, 59, 40, ...
    ## $ GS     <int> 81, 81, 79, 52, 51, 57, 49, 79, 74, 66, 66, 67, 73, 58, 39, ...
    ## $ MP     <dbl> 37.9, 39.8, 37.0, 36.0, 38.1, 36.3, 34.8, 40.0, 39.5, 36.1, ...
    ## $ FG     <dbl> 9.0, 11.8, 11.8, 11.0, 10.8, 11.2, 10.4, 12.1, 11.0, 10.6, 1...
    ## $ FGA    <dbl> 16.1, 19.6, 20.2, 19.1, 19.4, 19.1, 18.1, 21.1, 19.2, 18.3, ...
    ## $ `FG%`  <dbl> 0.562, 0.599, 0.583, 0.573, 0.557, 0.584, 0.576, 0.574, 0.57...
    ## $ `3P`   <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
    ## $ `3PA`  <dbl> 0.0, 0.0, 0.1, 0.0, 0.1, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, ...
    ## $ `3P%`  <dbl> 0.000, 0.000, 0.000, 0.500, 0.000, NA, 0.000, 0.000, 0.000, ...
    ## $ `2P`   <dbl> 9.0, 11.8, 11.8, 10.9, 10.8, 11.2, 10.4, 12.1, 11.0, 10.6, 1...
    ## $ `2PA`  <dbl> 16.1, 19.6, 20.1, 19.1, 19.4, 19.1, 18.0, 21.1, 19.2, 18.3, ...
    ## $ `2P%`  <dbl> 0.563, 0.600, 0.585, 0.573, 0.559, 0.584, 0.577, 0.575, 0.57...
    ## $ `eFG%` <dbl> 0.562, 0.599, 0.583, 0.574, 0.557, 0.584, 0.576, 0.574, 0.57...
    ## $ FT     <dbl> 5.3, 5.8, 5.8, 4.6, 4.5, 6.0, 5.5, 5.5, 6.7, 5.9, 6.7, 4.9, ...
    ## $ FTA    <dbl> 8.9, 10.5, 10.8, 9.5, 9.4, 11.4, 10.2, 10.4, 13.1, 10.7, 10....
    ## $ `FT%`  <dbl> 0.592, 0.554, 0.533, 0.487, 0.484, 0.527, 0.540, 0.524, 0.51...
    ## $ ORB    <dbl> 4.2, 4.7, 4.2, 3.4, 3.8, 3.5, 3.8, 4.3, 3.9, 3.5, 3.9, 3.7, ...
    ## $ DRB    <dbl> 9.6, 8.5, 7.3, 7.7, 8.7, 7.9, 6.9, 9.4, 8.8, 7.2, 7.2, 7.8, ...
    ## $ TRB    <dbl> 13.9, 13.2, 11.4, 11.0, 12.5, 11.4, 10.7, 13.6, 12.7, 10.7, ...
    ## $ AST    <dbl> 1.9, 2.4, 2.7, 2.9, 3.1, 2.4, 2.3, 3.8, 3.7, 3.0, 3.1, 2.9, ...
    ## $ STL    <dbl> 0.7, 0.9, 0.9, 0.6, 0.9, 0.7, 0.7, 0.5, 0.6, 0.6, 0.6, 0.5, ...
    ## $ BLK    <dbl> 3.5, 2.9, 2.4, 2.1, 2.9, 2.4, 1.7, 3.0, 2.8, 2.0, 2.4, 2.5, ...
    ## $ TOV    <dbl> 3.8, 2.7, 2.6, 2.9, 2.9, 2.9, 2.5, 2.8, 2.9, 2.6, 2.9, 2.9, ...
    ## $ PF     <dbl> 4.0, 3.5, 3.3, 3.6, 3.5, 3.2, 3.2, 3.2, 3.5, 3.0, 3.4, 3.4, ...
    ## $ PTS    <dbl> 23.4, 29.3, 29.3, 26.6, 26.2, 28.3, 26.3, 29.7, 28.7, 27.2, ...

``` r
view(Oneal)
```

## Select only the evident rows

``` r
# use slice function to sebset a data set
Oneal = Oneal %>% 
  slice(1:21) %>% 
  clean_names()
# check integrity of the data frame 
view(Oneal)
glimpse(Oneal)
```

    ## Rows: 21
    ## Columns: 30
    ## $ season       <chr> "1992-93", "1993-94", "1994-95", "1995-96", "1996-97",...
    ## $ age          <int> 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33...
    ## $ tm           <chr> "ORL", "ORL", "ORL", "ORL", "LAL", "LAL", "LAL", "LAL"...
    ## $ lg           <chr> "NBA", "NBA", "NBA", "NBA", "NBA", "NBA", "NBA", "NBA"...
    ## $ pos          <chr> "C", "C", "C", "C", "C", "C", "C", "C", "C", "C", "C",...
    ## $ g            <int> 81, 81, 79, 54, 51, 60, 49, 79, 74, 67, 67, 67, 73, 59...
    ## $ gs           <int> 81, 81, 79, 52, 51, 57, 49, 79, 74, 66, 66, 67, 73, 58...
    ## $ mp           <dbl> 37.9, 39.8, 37.0, 36.0, 38.1, 36.3, 34.8, 40.0, 39.5, ...
    ## $ fg           <dbl> 9.0, 11.8, 11.8, 11.0, 10.8, 11.2, 10.4, 12.1, 11.0, 1...
    ## $ fga          <dbl> 16.1, 19.6, 20.2, 19.1, 19.4, 19.1, 18.1, 21.1, 19.2, ...
    ## $ fg_percent   <dbl> 0.562, 0.599, 0.583, 0.573, 0.557, 0.584, 0.576, 0.574...
    ## $ x3p          <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
    ## $ x3pa         <dbl> 0.0, 0.0, 0.1, 0.0, 0.1, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0,...
    ## $ x3p_percent  <dbl> 0.0, 0.0, 0.0, 0.5, 0.0, NA, 0.0, 0.0, 0.0, 0.0, NA, N...
    ## $ x2p          <dbl> 9.0, 11.8, 11.8, 10.9, 10.8, 11.2, 10.4, 12.1, 11.0, 1...
    ## $ x2pa         <dbl> 16.1, 19.6, 20.1, 19.1, 19.4, 19.1, 18.0, 21.1, 19.2, ...
    ## $ x2p_percent  <dbl> 0.563, 0.600, 0.585, 0.573, 0.559, 0.584, 0.577, 0.575...
    ## $ e_fg_percent <dbl> 0.562, 0.599, 0.583, 0.574, 0.557, 0.584, 0.576, 0.574...
    ## $ ft           <dbl> 5.3, 5.8, 5.8, 4.6, 4.5, 6.0, 5.5, 5.5, 6.7, 5.9, 6.7,...
    ## $ fta          <dbl> 8.9, 10.5, 10.8, 9.5, 9.4, 11.4, 10.2, 10.4, 13.1, 10....
    ## $ ft_percent   <dbl> 0.592, 0.554, 0.533, 0.487, 0.484, 0.527, 0.540, 0.524...
    ## $ orb          <dbl> 4.2, 4.7, 4.2, 3.4, 3.8, 3.5, 3.8, 4.3, 3.9, 3.5, 3.9,...
    ## $ drb          <dbl> 9.6, 8.5, 7.3, 7.7, 8.7, 7.9, 6.9, 9.4, 8.8, 7.2, 7.2,...
    ## $ trb          <dbl> 13.9, 13.2, 11.4, 11.0, 12.5, 11.4, 10.7, 13.6, 12.7, ...
    ## $ ast          <dbl> 1.9, 2.4, 2.7, 2.9, 3.1, 2.4, 2.3, 3.8, 3.7, 3.0, 3.1,...
    ## $ stl          <dbl> 0.7, 0.9, 0.9, 0.6, 0.9, 0.7, 0.7, 0.5, 0.6, 0.6, 0.6,...
    ## $ blk          <dbl> 3.5, 2.9, 2.4, 2.1, 2.9, 2.4, 1.7, 3.0, 2.8, 2.0, 2.4,...
    ## $ tov          <dbl> 3.8, 2.7, 2.6, 2.9, 2.9, 2.9, 2.5, 2.8, 2.9, 2.6, 2.9,...
    ## $ pf           <dbl> 4.0, 3.5, 3.3, 3.6, 3.5, 3.2, 3.2, 3.2, 3.5, 3.0, 3.4,...
    ## $ pts          <dbl> 23.4, 29.3, 29.3, 26.6, 26.2, 28.3, 26.3, 29.7, 28.7, ...

## Add the column Player to the Oneal data frame

``` r
Oneal = Oneal %>% 
  mutate(
    player = "Shaquille O'Neal"
  )

AllStarsPlayers = AllStarsPlayers %>% 
  mutate(
    season = "AllStar"
  ) %>% 
  remove_empty()

view(AllStarsPlayers)
```

``` r
Oneal = Oneal %>% 
  mutate(
    fg_per_game = fg / g,
    fga_per_game = fga / g,
    x3p_per_game = x3p / g
  ) %>% 
  rename(
    "fg_total" = "fg",
    "fga_total" = "fga",
    "x3p_total" = "x3p"
  )
glimpse(Oneal)
```

    ## Rows: 21
    ## Columns: 34
    ## $ season       <chr> "1992-93", "1993-94", "1994-95", "1995-96", "1996-97",...
    ## $ age          <int> 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33...
    ## $ tm           <chr> "ORL", "ORL", "ORL", "ORL", "LAL", "LAL", "LAL", "LAL"...
    ## $ lg           <chr> "NBA", "NBA", "NBA", "NBA", "NBA", "NBA", "NBA", "NBA"...
    ## $ pos          <chr> "C", "C", "C", "C", "C", "C", "C", "C", "C", "C", "C",...
    ## $ g            <int> 81, 81, 79, 54, 51, 60, 49, 79, 74, 67, 67, 67, 73, 59...
    ## $ gs           <int> 81, 81, 79, 52, 51, 57, 49, 79, 74, 66, 66, 67, 73, 58...
    ## $ mp           <dbl> 37.9, 39.8, 37.0, 36.0, 38.1, 36.3, 34.8, 40.0, 39.5, ...
    ## $ fg_total     <dbl> 9.0, 11.8, 11.8, 11.0, 10.8, 11.2, 10.4, 12.1, 11.0, 1...
    ## $ fga_total    <dbl> 16.1, 19.6, 20.2, 19.1, 19.4, 19.1, 18.1, 21.1, 19.2, ...
    ## $ fg_percent   <dbl> 0.562, 0.599, 0.583, 0.573, 0.557, 0.584, 0.576, 0.574...
    ## $ x3p_total    <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...
    ## $ x3pa         <dbl> 0.0, 0.0, 0.1, 0.0, 0.1, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0,...
    ## $ x3p_percent  <dbl> 0.0, 0.0, 0.0, 0.5, 0.0, NA, 0.0, 0.0, 0.0, 0.0, NA, N...
    ## $ x2p          <dbl> 9.0, 11.8, 11.8, 10.9, 10.8, 11.2, 10.4, 12.1, 11.0, 1...
    ## $ x2pa         <dbl> 16.1, 19.6, 20.1, 19.1, 19.4, 19.1, 18.0, 21.1, 19.2, ...
    ## $ x2p_percent  <dbl> 0.563, 0.600, 0.585, 0.573, 0.559, 0.584, 0.577, 0.575...
    ## $ e_fg_percent <dbl> 0.562, 0.599, 0.583, 0.574, 0.557, 0.584, 0.576, 0.574...
    ## $ ft           <dbl> 5.3, 5.8, 5.8, 4.6, 4.5, 6.0, 5.5, 5.5, 6.7, 5.9, 6.7,...
    ## $ fta          <dbl> 8.9, 10.5, 10.8, 9.5, 9.4, 11.4, 10.2, 10.4, 13.1, 10....
    ## $ ft_percent   <dbl> 0.592, 0.554, 0.533, 0.487, 0.484, 0.527, 0.540, 0.524...
    ## $ orb          <dbl> 4.2, 4.7, 4.2, 3.4, 3.8, 3.5, 3.8, 4.3, 3.9, 3.5, 3.9,...
    ## $ drb          <dbl> 9.6, 8.5, 7.3, 7.7, 8.7, 7.9, 6.9, 9.4, 8.8, 7.2, 7.2,...
    ## $ trb          <dbl> 13.9, 13.2, 11.4, 11.0, 12.5, 11.4, 10.7, 13.6, 12.7, ...
    ## $ ast          <dbl> 1.9, 2.4, 2.7, 2.9, 3.1, 2.4, 2.3, 3.8, 3.7, 3.0, 3.1,...
    ## $ stl          <dbl> 0.7, 0.9, 0.9, 0.6, 0.9, 0.7, 0.7, 0.5, 0.6, 0.6, 0.6,...
    ## $ blk          <dbl> 3.5, 2.9, 2.4, 2.1, 2.9, 2.4, 1.7, 3.0, 2.8, 2.0, 2.4,...
    ## $ tov          <dbl> 3.8, 2.7, 2.6, 2.9, 2.9, 2.9, 2.5, 2.8, 2.9, 2.6, 2.9,...
    ## $ pf           <dbl> 4.0, 3.5, 3.3, 3.6, 3.5, 3.2, 3.2, 3.2, 3.5, 3.0, 3.4,...
    ## $ pts          <dbl> 23.4, 29.3, 29.3, 26.6, 26.2, 28.3, 26.3, 29.7, 28.7, ...
    ## $ player       <chr> "Shaquille O'Neal", "Shaquille O'Neal", "Shaquille O'N...
    ## $ fg_per_game  <dbl> 0.11111111, 0.14567901, 0.14936709, 0.20370370, 0.2117...
    ## $ fga_per_game <dbl> 0.1987654, 0.2419753, 0.2556962, 0.3537037, 0.3803922,...
    ## $ x3p_per_game <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, ...

``` r
Oneal = Oneal %>% 
  rename("mins_total" = "mp",
         "pts_total" = "pts",
         "trb_total" = "trb",
         "ast_total" = "ast")

view(Oneal)
```

``` r
Oneal = Oneal %>% 
  convert(int("g", "gs", "mins_total", "fg_total", "fga_total", "x3p_total", "x3pa", "x2p", "x2pa", "ft", "fta", "orb", "drb", "trb_total", "ast_total","stl", "blk", "tov", "pf"),
      num("fg_percent", "x3p_percent", "x2p_percent", "ft_percent"))
```

## Compare the names of the two data frames

``` r
# Instead of individually comparing the files we can use the janitor function compare_df_cols_same().
compare_df_cols_same(AllStarsPlayers, Oneal, bind_method = "rbind")
```

    ##      column_name       ..1       ..2
    ## 1            age      <NA>   integer
    ## 2   ast_per_game   integer      <NA>
    ## 3   e_fg_percent      <NA>   numeric
    ## 4             lg      <NA> character
    ## 5  mins_per_game   integer      <NA>
    ## 6            pos      <NA> character
    ## 7   pts_per_game   integer      <NA>
    ## 8      pts_total character   numeric
    ## 9             tm      <NA> character
    ## 10  trb_per_game   integer      <NA>

    ## [1] FALSE

## Remove the columns from AllStarPlayers that are not present in Oneal

``` r
AllStarsPlayers = AllStarsPlayers %>% 
  select(-ast_per_game,-mins_per_game,-pts_per_game,-pts_total,-trb_per_game)

Oneal = Oneal %>% 
  select(-age,-e_fg_percent,-lg,-pos,-pts_total,-tm)

AllStarsOneal = rbind(AllStarsPlayers, Oneal)

view(AllStarsOneal)
```

## Extract Shaquille O’Neal details from AllstarsOneal data frame

``` r
AllStarsOneal %>% 
  filter(player == "Shaquille O'Neal")
```

    ## # A tibble: 22 x 28
    ##    player     g    gs mins_total fg_total fga_total fg_percent x3p_total  x3pa
    ##    <chr>  <int> <int>      <int>    <int>     <int>      <dbl>     <int> <int>
    ##  1 Shaqu~    12     9        274       87       158      0.551         0     2
    ##  2 Shaqu~    81    81         37        9        16      0.562         0     0
    ##  3 Shaqu~    81    81         39       11        19      0.599         0     0
    ##  4 Shaqu~    79    79         37       11        20      0.583         0     0
    ##  5 Shaqu~    54    52         36       11        19      0.573         0     0
    ##  6 Shaqu~    51    51         38       10        19      0.557         0     0
    ##  7 Shaqu~    60    57         36       11        19      0.584         0     0
    ##  8 Shaqu~    49    49         34       10        18      0.576         0     0
    ##  9 Shaqu~    79    79         40       12        21      0.574         0     0
    ## 10 Shaqu~    74    74         39       11        19      0.572         0     0
    ## # ... with 12 more rows, and 19 more variables: x3p_percent <dbl>, x2p <int>,
    ## #   x2pa <int>, x2p_percent <dbl>, ft <int>, fta <int>, ft_percent <dbl>,
    ## #   orb <int>, drb <int>, trb_total <int>, ast_total <int>, stl <int>,
    ## #   blk <int>, tov <int>, pf <int>, fg_per_game <dbl>, fga_per_game <dbl>,
    ## #   x3p_per_game <dbl>, season <chr>
