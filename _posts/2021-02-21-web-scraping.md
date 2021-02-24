---
layout: post
title: "Web Scraping in R"
subtitle: "Scraping Movie Data  from IMDB  with the Rvest package"
date: 2020-01-26 23:45:13 -0400
background: '/img/posts/web-scraping/cinema-bg.jpg'
---
library(rvest)
library(dplyr)

## ***The webpage***

![IMDB page](/img/posts/web-scraping/myimdbimage.png)


## Create a var named link and a var page that reads that link
link =  "https://www.imdb.com/search/title/?title_type=feature&genres=adventure&moviemeter=25000,&sort=user_rating,desc"
page = read_html(link)


## Extract the name feature 
name = page %>% 
  html_nodes(".lister-item-header a") %>% 
  html_text()

name
## Extract the year
year = page %>% 
  html_nodes(".text-muted.unbold") %>% 
  html_text()

year

## Extract the rating

rating = page %>% 
  html_nodes(".ratings-imdb-rating strong") %>% 
  html_text()

rating

## Extract the sinopsys

synopsis = page %>% 
  html_nodes(".ratings-bar+ .text-muted") %>% 
  html_text()

synopsis
View(synopsis)

## Create a data frame
movies = tibble(name, year, rating, synopsis)

View(movies)
glimpse(movies)
## Save the data frame

write.csv(movies, "movies-juv.csv")

