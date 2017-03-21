---
layout: post
title: "The Financial Stability of Private Colleges"
date: 2017-3-9
output:
  html_document
share: true
categories: blog
excerpt: "Making an interactive leaflet map with colleges that are on the financial watchlist"
tags: [rstats]
---


The Chronicle of Higher Education [released a list](http://www.chronicle.com/article/177-Private-Colleges-Fail/239436) of the financial composite scores for each private university in the United States. I wanted to try and work with the [leaflet package](https://rstudio.github.io/leaflet/) to create an interactive map. Here's what I came up with. 

{% highlight r %}
library(readr)
library(rgdal)
library(rgeos)
library(sp)
library(leaflet)
library(magrittr)
{% endhighlight %}

I had to get the dataframes into the right format for the leaflet package to work properly. That required some cleaning and some merging. The most important thing I did was use the geocode function in the [ggmap package](https://cran.r-project.org/web/packages/ggmap/ggmap.pdf) to acquire the longitude and latitude of every college on the list. Unfortunately it wasn't 100% accurate, but it got it right most of the time. 

{% highlight r %}
college <- read_csv("D:/mapping_colleges/chronicle.csv")
college$name <- college$`ï»¿"College"`
geocodes <- read_csv("D:/mapping_colleges/geocodes.csv")

merge <- cbind(college, geocodes)
geocodes$X1 <- NULL
merge <- na.omit(merge)

wgs84<-CRS("+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs +towgs84=0,0,0")
ourCoords <- data.frame(lon = merge$lon, lat = merge$lat)
ourSpatialPoints <- SpatialPointsDataFrame(ourCoords, data = data.frame(ID = merge$name), proj4string = wgs84)
{% endhighlight %}

I also wanted to create a helpful pop up whenever the user clicked on one of the dots. Here I created a pop up that did two things. First, it the name of the college. Second, it gives the composite score which indicates overall financial health. 

{% highlight r %}
pop<-paste0("<b>Name</b>: ",ourSpatialPoints$ID, "<br>",
            "<b>Composite Score</b>: ",merge$`Composite Score`)
{% endhighlight %}

 
I wanted to create a green, yellow, red legend so that people could easily see which schools were in the worse trouble. In order to do that I had to write a simple function to break the scores down into three categories. Then I wanted to use some nice icons. 
{% highlight r %}
getColor <- function(merge) {
  sapply(merge$score, function(score) {
    if(score >= 2.5) {
      "green"
    } else if(score >= 1.5) {
      "orange"
    } else {
      "red"
    } })
}


icons <- awesomeIcons(
  icon = 'ios-close',
  iconColor = 'black',
  library = 'ion',
  markerColor = getColor(merge)
)
{% endhighlight %}

I really liked the look of a darker map, so I wanted to use that as my base. In addition I wanted the map to be zoomed to the center of the United States to give the user a nice starting point. 

{% highlight r %}
leaflet(merge) %>% addTiles('http://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png', attribution='Map tiles by <a href="http://stamen.com">Stamen Design</a>, <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a> &mdash; Map data &copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>') %>%
  addAwesomeMarkers(~lon, ~lat, icon=icons, popup =pop)  %>% 
  addLegend("bottomright", colors= c("Green", "Yellow", "Red"), labels= c("> 2.5", "1.5-2.5", "< 1.5"),  title="Composite Score")%>% 
  setView(-98.690940, 39.651426, zoom = 4)
{% endhighlight %}


### The Finished Product

Every college in the database is mapped here. The scores range from -1 to 3. Any school that scores 1.5 or lower is considered to be in financial distress. 


<iframe src="//rstudio-pubs-static.s3.amazonaws.com/257221_20d5cfc305264f3cbcc159f9408dbce5.html"
style="border: none; width: 900px; height: 800px">></iframe>


