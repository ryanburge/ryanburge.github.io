---
layout: post
title: "How does Trump speak to evangelicals?"
date: 2016-6-23
output:
  html_document
share: true
categories: blog
excerpt: "A text analysis of Donald Trump's recent meeting with evangelical leaders."
tags: [rstats]
---



{% highlight r %}
library(tm)
library(stringr)
library(wordcloud)
library(syuzhet)
library(lubridate)
library(ggplot2)
library(scales)
library(reshape2)
library(dplyr)
{% endhighlight %}

## Trump's Meeting with Evangelicals

Trump has an odd relationship with evangelicals. He tried to put money in the [communion plate at a church in Iowa.](https://www.washingtonpost.com/g00//news/acts-of-faith/wp/2016/02/01/donald-trump-accidentally-put-money-in-the-communion-plate-at-a-church-in-iowa/) and he said "two Corinthians" instead of "second Corinthians" [while speaking at Liberty University.](http://www.npr.org/2016/01/18/463528847/citing-two-corinthians-trump-struggles-to-make-the-sale-to-evangelicals). Now, that he won the Republican nomination, he really has to start building a coalition and evangelicals are a crucial part of that for any Republican. To that end, Trump had a meeting with several hundred leaders of evangelical churches and organizations last week in New York City. The meeting was not open to the press, but someone snuck in a tape recorder and then provided that audio to Yahoo, who transcribed it. I wanted to do some analysis of that transcript. 

## Transcript Collecting and Cleaning

The first task was to take the transcript and put it into a format that R can read, which is typically CSV. So, what I did was create a column for each speaker (as identified by Yahoo) and then the text of what they said in the next column. That file can be found [here](https://github.com/ryanburge/trump_transcript/blob/master/transcript.csv). I wanted to create a dataframe of just Trump's words and then everyone else's words: 


{% highlight r %}
full <- read.csv("D:/trump_transcript/transcript.csv", stringsAsFactors = FALSE)
trump <- subset(full, Speaker == "Trump")
not.trump <- subset(full, Speaker != "Trump")
{% endhighlight %}

I needed to do some cleaning. In order to do text analysis in R, there are some necessary steps. One is to make each letter lowercase so that R can match all instances of a word (which it wouldn't do if one was capitalized). Another is to get rid of "stopwords". These are words that like "in", "and", and so on that don't really make sense to analyze. 


How many total words did speak Trump use in the meeting? 

{% highlight r %}
rowSums(as.matrix(dtm2))
{% endhighlight %}



{% highlight text %}
##    1 
## 2407
{% endhighlight %}

How about everyone else? 





{% highlight r %}
rowSums(as.matrix(dtm2))
{% endhighlight %}



{% highlight text %}
##    1 
## 3470
{% endhighlight %}

## Who Spoke the Most? 

Let's visualize that difference. 


{% highlight r %}
count <- read.csv("D:/trump_transcript/totalcount.csv", stringsAsFactors = FALSE)

cbPalette <- c("red", "forestgreen")
ggplot(count, aes(1, count, fill=speaker)) + geom_bar(stat="identity") + theme(axis.ticks = element_blank(), axis.text.y = element_blank()) + theme(axis.title.y = element_blank()) + labs(fill="") + scale_fill_manual(values=cbPalette) + ylab("Word Count") + coord_flip() + theme(legend.position="bottom") +  guides(fill = guide_legend(reverse = TRUE))
{% endhighlight %}

![center](/figs/trump_transcript/unnamed-chunk-7-1.png)

Here's an interesting extension: How much did other people speak in the meeting? 

![center](/figs/trump_transcript/unnamed-chunk-8-1.png)

While Huckabee was the moderator of the event, and Ben Carson was intimately involved in getting evangelicals on board, neither of them spoke the most. In fact, Jerry Falwell Jr., spoke the most followed by James Dobson. Falwell has made news lately for how cozy he has become with Trump, including taking a photo with him in front of a wall of pictures, [one of which included Trump on Playboy magazine.](http://www.kansascity.com/news/politics-government/article85250202.html)



## What Words Did Trump Use? 

One of the better ways to see what words that Trump used is a wordcloud. 


{% highlight r %}
library(RColorBrewer)
pal <- brewer.pal(9,"Reds")
pal <- pal[-(1:4)]

review_text <- paste(trump$Text, collapse=" ")
review_source <- VectorSource(review_text)
wordCorpus <- Corpus(review_source)
wordCorpus <- tm_map(wordCorpus, removePunctuation)
wordCorpus <- tm_map(wordCorpus, content_transformer(tolower))
wordCorpus <- tm_map(wordCorpus, removeWords, stopwords("english"))
wordCorpus <- tm_map(wordCorpus, stripWhitespace)

wordcloud(words = wordCorpus, scale=c(5,0.1), max.words=100, random.order=FALSE,
          rot.per=0.35, use.r.layout=FALSE, colors=pal)
{% endhighlight %}

![center](/figs/trump_transcript/unnamed-chunk-9-1.png)

It's interesting to note that Trump talks a lot about what he is "going" to do. It's also interesting to note what words don't show up here. No talk of spiritual things really come up. Let's compare that to everyone else who spoke. 

![center](/figs/trump_transcript/unnamed-chunk-10-1.png)

Going is also a popular word here, but it's not as popular for this group. You can also see that there are some religious words that start to emerge around the periphery. Words like "pray" and "god", show up a lot more. 

So, I did some digging and pulled out words that are specifically religious in connotation. 

| Term                | Others | Trump |  
|---------------------|--------|-------|
| pray/praying/prayer | 25     | 0     | 
| god                 | 17     | 1     |
| jesus               | 11     | 0     | 
| faith               | 9      | 0     |  
| church              | 8      | 4     | 
| christ              | 7      | 0     |   
| evangelical(s)      | 7      | 2     |  
| bible               | 4      | 0     |  
| spiritual           | 4      | 0     |   

It's sort of staggering how uncomfortable Trump is using evangelical buzzwords. He never said "Christ" or any version of "prayer". He mentioned "God" just once. It's obvious from this that Trump is having a hard time understanding the evangelical mindset. 

## Is Trump Positive? Are evangelicals angry? 

How about some sentiment analysis. Is Trump positive or negative? Fearful or Angry?


{% highlight r %}
mySentiment <- get_nrc_sentiment(trump$Text)
trump <- cbind(trump, mySentiment)
sentimentTotals <- data.frame(colSums(trump[,c(3:12)]))
names(sentimentTotals) <- "count"
sentimentTotals <- cbind("sentiment" = rownames(sentimentTotals), sentimentTotals)
rownames(sentimentTotals) <- NULL
ggplot(data = sentimentTotals, aes(x = sentiment, y = count)) +
  geom_bar(aes(fill = sentiment), stat = "identity") +
  theme(legend.position = "none") +
  xlab("Sentiment") + ylab("Total Count") + ggtitle("Total Sentiment Score for Trump's Statements")
{% endhighlight %}

![center](/figs/trump_transcript/unnamed-chunk-11-1.png)

![center](/figs/trump_transcript/unnamed-chunk-12-1.png)

Both groups are very positive, with that sentiment scoring the highest. However there is a difference between the two. Trump's words are much angrier and more fearful than the rest of the speakers at the gathering. The other speakers also display a high level of trust in their language. I don't think that we can make any conlusions here, however. The number of evangelicals who spoke was very small and in no way represents evangelicalism as a whole. 

## Conclusion

Trump has an evangelical problem. In the last few months he has had some scrapes with evangelical leaders [like Russell Moore](http://time.com/4323009/donald-trump-southern-baptist-russell-moore-evangelicals-christianity/) who is very well known about Southern Baptists. With his continued stumbles and his sagging poll numbers, Trump has to work hard to bring together some sort of coalition. While evangelicals are usually firmly behind the Republican nominee, that can be a foregone conclusion here. 
