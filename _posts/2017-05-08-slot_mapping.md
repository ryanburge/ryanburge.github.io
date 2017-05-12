---
layout: post
title: "Mapping Slot Machine Revenues in Illinois in March 2017"
date: 2017-5-8
output:
  html_document
share: true
categories: blog
excerpt: "Using Leaflet to Visualize Electronic Gaming in the State of Illinois"
tags: [rstats]
---

## Here is the static map, scroll down for the interactive version. 

![alt text](https://raw.githubusercontent.com/ryanburge/Slot_Machine/master/screenshot.png "Screenshot")





{% highlight r %}
library(ggplot2)
library(rvest)
library(dplyr)
library(car)
library(leaflet)
library(fuzzyjoin)
library(readr)
library(ggmap)
library(DT)
{% endhighlight %}
## Introduction

I noticed that someone posted on Facebook how much slot revenue was generated in my hometown of Salem, Illinois and it seemed like a lot. I wanted to see if it really was or not, so I decided to take a look for myself. [The Illinois Gaming Board](http://www.igb.illinois.gov/VideoReports.aspx) actually makes it easy to pull down the monthly reports, which I was suprised to see. It even gives the option of download a .csv file. Nice. 


{% highlight r %}
slots <- read_csv("D://Slot_Machine/slots.csv")
slots$Name <- slots$Municipality
{% endhighlight %}

## Scraping Wikipedia

Gaming revenue is highly dependent on population, or at least it would seem to be. It would make no sense to just plot this revenue without any sort of context associated. So, I needed to find a dataset of the population of cities and towns in the state of Illinois. Wikipedia to the rescue. Using the rvest package, I was able to scrape the [Wikipedia table](https://en.wikipedia.org/wiki/List_of_cities_in_Illinois), then do some cleaning on it.  



{% highlight r %}
url <- "https://en.wikipedia.org/wiki/List_of_cities_in_Illinois"
ill <- url %>%
  read_html() %>%
  html_nodes(xpath='//*[@id="mw-content-text"]/table[2]') %>%
  html_table()

ill <- ill[[1]]

ill$pop <- as.numeric(gsub(",", "", ill$Population))
{% endhighlight %}

## Merging the Datasets

The problem comes in joining the two things together. I already had taken out some of the county level data from the Illinois Gaming database but I didn't want to plot the slots that are in the counties, but instead just the ones located in cities and towns. A straight merge is going to not make some matches that need to be made. So, I used the [fuzzyjoin R package](https://cran.r-project.org/web/packages/fuzzyjoin/index.html), which allows a user to "fudge" the merge a little bit so that the key vector doesn't have to be exactly right to actually make a match. Then, I removed any duplicates that occurred. 


{% highlight r %}
joined <- slots %>%
  stringdist_right_join(ill, by = c("Name"), max_dist = 2)

joined <- joined %>% distinct(Municipality, .keep_all = TRUE)
{% endhighlight %}

## How to Measure Gambling Activity? 

There are a lot of different numbers in the gaming reports, and each offers a different way to measure how much gambling is going on. After thinking about it a lot, I decided that total amount lost is probably not a good approach. Here's why. I could gamble today with one hundred dollars. I could get some wins and get some losses. Play for 4 hours and walk out with the same hundred dollars. Tomorrow I could start with the same hundred dollars. Gamble for ten minutes, lose it all, and stop. On the first day I gambled probably over a thousand dollars in total, the second day it was just one hundred. Total amount played gets us closer to actual volume. Let's divide that by the number of people in each town. 


{% highlight r %}
joined$percapita <- joined$`Amount Played`/joined$pop
capita <- select(joined, Name.x, percapita)
capita %>% arrange(-percapita)
{% endhighlight %}



{% highlight text %}
## # A tibble: 327 ? 2
##              Name.x percapita
##               <chr>     <dbl>
## 1           McHenry  3480.948
## 2  Oakbrook Terrace  2457.842
## 3           Addison  1923.118
## 4      East Dubuque  1523.911
## 5            Ashley  1443.103
## 6         Grayville  1182.561
## 7              Cary  1171.151
## 8       Countryside  1162.938
## 9            Orient  1036.096
## 10           Dawson  1024.434
## # ... with 317 more rows
{% endhighlight %}

## Getting my Coordinates

How I need to get the longitude and latitude of each city in the dataset. To make that easier, I added a column with the state name. Then I merged that into a new column which I fed into the geocode command to get a more accurate set of coordinates. Here, I just saved that data and loaded it instead of hitting the Google Maps API every time. 


{% highlight r %}
capita$state <- c("Illinois")
capita$citystate <- paste(capita$Name.x, capita$state, sep = ", ")
#coords <- geocode(capita$citystate)
coords <- read_csv("D:/Slot_Machine/coords.csv")
map <- cbind(capita, coords)
map <- cbind(map, joined)
map <- map %>% subset(., select=which(!duplicated(names(.))))
{% endhighlight %}

## Making a Popup

Finally, I am going to create some variables that will be displayed in a popup whenever the user clicks each pin. This took some work, but I think it provides the user a lot more information that may be useful for them. 


{% highlight r %}
map$taxcap <- map$`Municipality Share`/map$pop
map$macpop <- map$pop/map$`VGT Count`

pop<-paste0("<b>City</b>: ",map$Name.x, "<br>",
            "<b>Population</b>: ", map$Population, "<br>",
            "<b>Gambling $ Per Capita</b>: ",round(map$percapita, 0), "<br>",
            "<b>Tax $ Per Capita</b>: ",round(map$taxcap, 0), "<br>",
            "<b>Residents Per Machine</b>: ",round(map$macpop, 0), "<br>",
            "<b>Number of Gambling Establishments</b>: ",map$`Establishment Count`, "<br>")
{% endhighlight %}

## Let's Map!


{% highlight r %}
leaflet()%>%
  addProviderTiles(providers$Esri.NatGeoWorldMap)%>%
  addCircleMarkers(lng = map$lon, lat= map$lat, radius = map$percapita/100, popup = pop)
{% endhighlight %}


## Here's the Interactive Map and Searchable Table

<iframe src="//rstudio-pubs-static.s3.amazonaws.com/271573_499555e5490f44a9834f262f114cd5d7.html"
style="border: none; width: 900px; height: 800px">></iframe>

















