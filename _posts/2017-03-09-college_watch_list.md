---
layout: post
title: "Colleges In Trouble"
date: 2017-3-9
output:
  html_document
share: true
categories: blog
excerpt: "Making an interactive leaflet map with colleges that are on the financial watchlist"
tags: [rstats]
---


The Chronicle of Higher Education [released a list](http://www.chronicle.com/article/177-Private-Colleges-Fail/239436) of 177 schools that were on the financial watchlist. I wanted to try and work with the [leaflet package](https://rstudio.github.io/leaflet/) to create an interactive map. Here's what I came up with. 

{% highlight r %}
library(readr)
library(rgdal)
library(rgeos)
library(sp)
library(leaflet)
library(magrittr)

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



{% highlight r %}
pop<-paste0("<b>Name</b>: ",ourSpatialPoints$ID, "<br>",
            "<b>Composite Score</b>: ",merge$`Composite Score`)
{% endhighlight %}




{% highlight r %}

map<-leaflet()%>%
  addTiles('http://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}.png', attribution='Map tiles by <a href="http://stamen.com">Stamen Design</a>, <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a> &mdash; Map data &copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>')%>%
  addCircleMarkers(data=ourSpatialPoints, radius =5, fillColor = "firebrick", fillOpacity=0.8, weight=1, color='black' , popup=pop)
{% endhighlight %}




{% highlight r %}
map %>% setView(-98.690940, 39.651426, zoom = 4)
{% endhighlight %}


### The Finished Product

<iframe src="//rstudio-pubs-static.s3.amazonaws.com/257221_6cc90b9ad7e84cdc8d57c4cfd9cd71c3.html"
style="border: none; width: 900px; height: 800px">></iframe>
