---
layout: post
title: "The National Prayer Breakfast"
date: 2017-2-09
output:
  html_document
share: true
categories: blog
excerpt: "How do Presidents handle the National Prayer Breakfast? Featuring Text and Sentiment Analysis"
tags: [rstats]
---


{% highlight r %}
library(dplyr)
library(tm)
library(wordcloud2)
library(wordcloud)
library(ggplot2)
library(dplyr)
library(extrafont)
library(tidyr)
library(tidytext)
prayer <- read.csv("D:/prayer_breakfast/prayer_breakfast.csv")
{% endhighlight %}

American presidents have to navigate a number of traditions that sometimes blur the separation of church and state. As I’ve previously written, President Trump struggled to find ministers to pray at his inauguration. And thanks to that terrific show The West Wing, many are aware of the “Red Mass,” a Catholic Church service held annually which is attended by several justices of the Supreme Court and other high ranking government officials. However, the event that has the potential to provide the most consternation to the President occurs every February –the “National Prayer Breakfast.” The event is hosted by the United States Congress, but is organized by a group called the Fellowship Foundation. It’s been an annual tradition for 65 years and every prayer breakfast has featured an appearance and a short speech by the President of the United States. 

The Prayer Breakfast is often an unremarkable Washington event, however that changed this year. Donald Trump made national news by asking the nearly 3,500 attendees to pray for the ratings of the television show “Celebrity Apprentice” after ratings have slid with Arnold Schwarzenegger hosting. That made me interested in how prior presidents have navigated the opportunity to speak on the record about matters of faiths. 

### Speech Length Over Time

Using the terrific resources available at the American Presidency Project hosted by the University of California at Santa-Barbara, I found the transcripts for every Prayer Breakfast address dating back to Nixon’s speech in 1970 (save for 1971 when Djupe was born). A good way to get a general sense of how Presidents approach the task is to look at the length of the speech. While the more recent speech transcripts indicate the time the speech began and concluded, that information is not available for the entire dataset. So,  I chose to use total word count in each speech as a proxy. 


{% highlight r %}
long <- read.csv("D:/prayer_breakfast/long.csv")

ggplot(long, aes(x=year, y=words)) + geom_line(colour = "black", size = 1) + geom_point(colour = "black") + 
  annotate("rect", xmin = 1969, xmax = 1975, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1975, xmax = 1975.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1975.45, xmax = 1979.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "blue")  +
  annotate("rect", xmin = 1979.45, xmax = 1989, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1989, xmax = 1992.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1992.45, xmax = 2000.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "blue")  + 
  annotate("rect", xmin = 2000.5, xmax = 2008.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  + 
  annotate("rect", xmin = 2008.45, xmax = 2016.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "blue")  + 
  annotate("rect", xmin = 2016.45, xmax = 2017.5, ymin = 0, ymax = Inf,  alpha = .2, fill = "red") + 
  ggtitle("                        Number of Words in Annual Prayer Breakfast Addresses") + 
  xlab("Year") + ylab("Number of Words") + 
  theme(text=element_text(size=16, family="KerkisSans"))
{% endhighlight %}

![center](/figs/prayer_bfast_writeup/unnamed-chunk-2-1.png)

There are a couple of notable findings here. In general, Presidents keep their speeches short. Very few of the speeches ran more than 800 words, which means that most speeches were less than ten minutes in length. This is especially evident among the Reagan and George H.W. Bush presidencies. Some of the most verbose speeches were given by Democrats, and Barack Obama’s speeches were especially lengthy.  In fact, the last seven speeches by President Obama were seven of the eight longest in the entire dataset. That bears some reflection. 

There are two diametrically opposed explanations for this finding. The first is that Obama was trying to overcompensate on matters of faith as a way to beat back his critics who consistently accused him of being a closeted Muslim. The other explanation is less pessimistic in that it asserts that Obama spoke at such length because he was truly at ease speaking of matters of faith. Which theory actually explains these findings may remain never be resolved, but I strongly suspect it’s the latter – he could pick much more high profile symbolic events than the National Prayer Breakfast. 

### Word Choice

In addition to look at length of address, it can be helpful to see what words are being used by Presidents. I chose a number of words that have a more general religious attachment, but also some words that are specific to Christian theology. 


{% highlight r %}
freq <- read.csv("D:/prayer_breakfast/freq.csv")
freq$speaker <- factor(freq$speaker, levels=c("Trump", "Obama", "Bush 43", "Clinton"))

ggplot(freq, aes(x=word, y=freq, group = speaker)) + geom_bar(aes(fill=speaker),stat="identity", position= "dodge")  + 
scale_fill_manual("speaker", values = c("firebrick1","dodgerblue", "firebrick4", "dodgerblue4")) +
ggtitle("       Religious Words Used by Each President at the Prayer Breakfast") +
xlab("Word") + ylab("Percentage of Total Words Spoken") + 
  theme(legend.title=element_blank()) + 
  theme(text=element_text(size=16, family="KerkisSans"))
{% endhighlight %}

![center](/figs/prayer_bfast_writeup/unnamed-chunk-3-1.png)

It is apparent that all recent Presidents were comfortable speaking in more general ways about religion, with occurrences of “faith”, “God”, and variations of “pray” happening on a consistent basis. What is also striking is that explicitly Christian words such as “Jesus,” “Christ,” and “Bible” are rarely used. This finding comports with previous scholarship, most notably,“The God Strategy” by Domke and Coe.They argue that politicians use deliberately vague and inclusive religious language as way to signal to their supporters what they believe, while also not alienating potential swing voters. 

### Word Clouds

I wanted to see how Obama's last Prayer Breakfast Address compared to Trump's first one. A simple way to do that is to create a comparison word cloud between the two. 


{% highlight r %}
compare <- head(prayer, 2)
docs <- Corpus(VectorSource(compare$text)) %>%
tm_map(removePunctuation) %>%
tm_map(removeNumbers) %>%
tm_map(tolower)  %>%
tm_map(removeWords, stopwords("english")) %>%
tm_map(stripWhitespace) %>%
tm_map(PlainTextDocument)
tdm <- TermDocumentMatrix(docs) %>%
as.matrix()
colnames(tdm) <- c("Trump","Obama")

par(mfrow=c(1,1))
comparison.cloud(tdm, random.order=FALSE, colors = c("indianred3","lightsteelblue3"),
title.size=1.5, max.words=200)
{% endhighlight %}

![center](/figs/prayer_bfast_writeup/unnamed-chunk-4-1.png)

What a comparison word cloud does is take the number of times one speaker said a specific word and subtract the same number of utterances by the other speaker. So, for example, Trump said "nation" several more times than Obama did. However, Obama said "pray" a lot more than Trump. The size of the word = the bigger disparity between the two. 


{% highlight r %}
commonality.cloud(tdm, random.order=FALSE, scale=c(5, .5),colors = brewer.pal(4, "Dark2"), max.words=200)
{% endhighlight %}

![center](/figs/prayer_bfast_writeup/unnamed-chunk-5-1.png)

Then we can use the commanility cloud to see what the two speakers said that were in common. Here "applause" shows up a lot because the two transcripts actually included the applause breaks. Beyond that both talked about "God" and "faith" a fair amount. This corroborates what was described earlier when talking about general religious words, not ones that target a specific type of religious faith. 

## Sentiment Analysis

I wanted to see if I could determine which speeches were the most positive and the least positive. The best way to do this using sentiment analysis. I used the "bing" sentiments scoring system to classify each word in the speech as positive or negative. Then I subtracted the positive words from the negative words and plotted. 


{% highlight r %}
prayer$text <- as.character(prayer$text)

tidy_speech <- prayer %>% 
  unnest_tokens(word, text)

data("stop_words")
cleaned_speech <- tidy_speech %>%
  anti_join(stop_words)

bing <- get_sentiments("bing")

speech_sentiment <- tidy_speech %>%
  inner_join(bing) %>%
  count(year,  sentiment) %>%
  spread(sentiment, n, fill = 0) %>%
  mutate(sentiment = positive - negative)

ggplot(speech_sentiment, aes(x=year, y=sentiment)) + geom_line(colour = "black", size = 1) + geom_point(colour = "black") + 
  annotate("rect", xmin = 1969, xmax = 1975, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1975, xmax = 1975.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1975.45, xmax = 1979.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "blue")  +
  annotate("rect", xmin = 1979.45, xmax = 1989, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1989, xmax = 1992.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  +
  annotate("rect", xmin = 1992.45, xmax = 2000.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "blue")  + 
  annotate("rect", xmin = 2000.5, xmax = 2008.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "red")  + 
  annotate("rect", xmin = 2008.45, xmax = 2016.4, ymin = 0, ymax = Inf,  alpha = .2, fill = "blue")  + 
  annotate("rect", xmin = 2016.45, xmax = 2017.5, ymin = 0, ymax = Inf,  alpha = .2, fill = "red") + 
  ggtitle("                        Sentiment in Annual Prayer Breakfast Addresses") + 
  xlab("Year") + ylab("Total Positive Sentiment")
{% endhighlight %}

![center](/figs/prayer_bfast_writeup/unnamed-chunk-6-1.png)

Here one thing stands out above all the rest. Obama's 2014 address was the most positive overall speech by a wide margin. Although, his 2015 was the second most positive. It's important here to note that this is somewhat a function of total word count. The more words used, the more likely a speech is to drive up the sentiment score, but that is assuming that the additional words are overwhelmingly positive, which is not a given. A speech of 5000 words can still have a sentiment score of zero or even negative if it has a lot more negative words. 

There are some barely positive speeches in the bunch. Reagan's first speech in 1981 was particulary sour, along with his 1984 version. Bill Clinton's 1997 speech had an overall score of 7, which is third lowest. It's hard to explain why this is so low for Clinton. He had a terrific reelection campaign, where he beat Bob Dole in an easy fashion. The Monica Lewinsky scandal was still months away. It's hard to explain. 

## Conclusion

Pivoting back to Trump now. It is no secret that President  Trump struggles in a religious environment. He tried to put money in the Communion plate during a church service in Iowa. He incorrectly referred to the book of Second Corinthians will giving a speech at Liberty University. But he will have to continue to give speeches to religious audiences during the next four years of his presidency. Observers would be wise to note how much he tries to use his words to appeal to evangelicals or whether he stays broad and generic in his rhetoric. There’s little doubt that Trump talking about religion will be of continual interest to people of faith during his term as president. 

All the data and coding for this project are available on [GitHub](https://github.com/ryanburge/prayer_breakfast)

