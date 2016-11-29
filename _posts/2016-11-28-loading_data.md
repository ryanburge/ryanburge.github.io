---
layout: post
title: "Loading Data into R"
date: 2016-11-28
output:
  html_document
share: true
categories: blog
excerpt: "A step by step process for loading data into R Studio"
tags: [rstats]
---





## Introduction

**You need to make sure that you are using R Studio Version 1.0 or higher to load data using this tutorial**

There are lots of ways to read data into R. The one that I have been having you use is probably the simplest because it takes zero work on your end. Essentially what the following command does it just download the .csv straight from the SIU website and load it into R. Here's the command you've been familiar with:


{% highlight r %}
simon <- read.csv(url("http://goo.gl/exQA14"))
{% endhighlight %}

So, left to right. We are calling our dataset "simon", it's a .csv file (which is a very basic type of data file that works in almost every situation), and then the web address where the data can be found: http://goo.gl/exQA14.

But what if you just found some data online somewhere and you want to load that into R. How do you go about doing that? 

## Manually loading data

Let's say you find some data to download. I will use Kaggle as a good example. They have some cool datasets.


![center](/figs/loading_data/kaggle1.png)

## Downloading the data

I like that "City Payroll Data", how do we get it? 

![center](/figs/loading_data/kaggle2.png)

## Unzipping the data

I downloaded it and unzipped. Now I have a .csv file called data.csv

![center](/figs/loading_data/kaggle3.PNG)

## Importing into R Studio

So, now I need ot get that into R Studio. Lucky for us, R Studio just added a nice import option. 

![center](/figs/loading_data/kaggle4.PNG)

## Importing Data Options

You can import data from all sorts of formats including SPSS, Stata, Excel, or CSV. For us, we have a CSV file so let's go to that option. 

![center](/figs/loading_data/kaggle5.PNG)

You need to do two things. 

1. In the top right there is a browse button. You can click that and it will open up "File Explorer" then you can just navigate to where you have saved your data file. For me, that was my desktop. When you do that R Studio will give you a nice little preview of the data. 

2. You need to determine what you are gonna call your dataset. We have always called ours "simon" because it's short and tells us what our data is. You can call it whatever. "df" is popular among the R community. So is "data." Just type that name into the bottom left box called "Name"

You should see in the bottom right box the exact code that R is going to use to open that data. Click import. 

I called mine "df."

## Final Results

![center](/figs/loading_data/kaggle6.PNG)

To make sure it worked just do: 


{% highlight text %}
## 
|================================================================================| 100%   88 MB
{% endhighlight %}


{% highlight r %}
head(df)
{% endhighlight %}



{% highlight text %}
## # A tibble: 6 ? 35
##   `Row ID`  Year        `Department Title` `Payroll Department`
##      <int> <int>                     <chr>                <chr>
## 1   111391  2014     Water And Power (DWP)                 <NA>
## 2    31732  2013             Police (LAPD)                 4301
## 3    27697  2013             Police (LAPD)                 4301
## 4    14136  2013       Harbor (Port of LA)                 3201
## 5    91896  2014 Public Works - Sanitation                 7024
## 6   106560  2014     Water And Power (DWP)                 <NA>
## # ... with 31 more variables: `Record Number` <dbl>, `Job Class
## #   Title` <chr>, `Employment Type` <chr>, `Hourly or Event Rate` <chr>,
## #   `Projected Annual Salary` <chr>, `Q1 Payments` <chr>, `Q2
## #   Payments` <chr>, `Q3 Payments` <chr>, `Q4 Payments` <chr>, `Payments
## #   Over Base Pay` <chr>, `% Over Base Pay` <chr>, `Total Payments` <chr>,
## #   `Base Pay` <chr>, `Permanent Bonus Pay` <chr>, `Longevity Bonus
## #   Pay` <chr>, `Temporary Bonus Pay` <chr>, `Lump Sum Pay` <chr>,
## #   `Overtime Pay` <chr>, `Other Pay & Adjustments` <chr>, `Other Pay
## #   (Payroll Explorer)` <chr>, MOU <chr>, `MOU Title` <chr>, `FMS
## #   Department` <chr>, `Job Class` <chr>, `Pay Grade` <chr>, `Average
## #   Health Cost` <chr>, `Average Dental Cost` <chr>, `Average Basic
## #   Life` <chr>, `Average Benefit Cost` <chr>, `Benefits Plan` <chr>, `Job
## #   Class Link` <chr>
{% endhighlight %}

## Writing a datafile

Let's say that you have done some manipulations to a datafile and you want to save that new file. Here's how you do that. 


{% highlight r %}
write.csv(df, "saved_data.csv")
{% endhighlight %}

And it's at the very bottom of the "Files" pane in the bottom right of R Studio. 

![center](/figs/loading_data/kaggle7.PNG)

## Where to find data? 

1. [Kaggle](https://www.kaggle.com) has a lot of crowd sourced datasets from all over the place. 
2. [Data.World](https://data.world/) is a newer website that has people upload interesting data. 
3. [General Social Survey](http://gss.norc.org/) is the most widely used social science dataset. It goes back to 1972 and asks questions about all sorts of behavior and politics. 
4. [National Election Study](http://www.electionstudies.org/) is the most comprehensive dataset of political behavior and opinion that exists. It goes back to 1948 and asks a lot of questions of interest to political scientists. 
5. [World Values Survey](http://www.worldvaluessurvey.org/wvs.jsp) is terrific if you are looking at comparative politics. It does a sample of almost all the countries on Earth on a rotation basis every five years. 

