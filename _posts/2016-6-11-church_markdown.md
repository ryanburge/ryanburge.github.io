---
layout: post
title: "Church Finances"
date: 2016-6-11
output:
  html_document
share: true
categories: blog
excerpt: "Doing Some Financial Analysis feautring Plot.ly and DataTable packages"
tags: [rstats]
---



## All Years Together

![center](/figs/church_markdown/unnamed-chunk-2-1.png)

Looking at just expenses here. January and February of 2016 were actually lower than average, as well as April. March was very high but that was somewhat due to the fact that we spent $3000 on a sound system. Without that it would have in normal range. May was the highest expense May that we have had since 2011. 

![center](/figs/church_markdown/unnamed-chunk-3-1.png)

Our total income is lower than normal, which is represented by the dashed line, but has been relatively consistent. 
![center](/figs/church_markdown/unnamed-chunk-4-1.png)

Net income is arrived at by taking the income and subtracting the expenses. 2016 has had two really bad months so far in terms of net income. But almost all months have a negative net income but it's usually much smaller. 

## Comparing Years

![center](/figs/church_markdown/unnamed-chunk-5-1.png)

These facet wraps just give a sense of how each year looks on its own. Expenses are all over the place. They can jump up and down pretty dramatically from month to month. 2015 is a good example of that. 

![center](/figs/church_markdown/unnamed-chunk-6-1.png)

Total income is much more stable over time. 2013 and 2014 for instance were very even. 2016 looks to be relatively the same. It's interesting to note that lost income in early 2015 because of weather seems to be made up for with a larger than normal April. 

![center](/figs/church_markdown/unnamed-chunk-7-1.png)

Looking at broad net income. Very few months over the last five+ years have had a positive net income. 2013 and 2014 look pretty similar with the trend moving toward zero near year end. 

## Average Month? 


{% highlight text %}
## Error in html_screenshot(x): Please install the webshot package (if not on CRAN, try devtools::install_github("wch/webshot"))
{% endhighlight %}

This chart is interactive, so you can mouse over each month to see the actual values of total income, total expenses, and net income. 

Which months are bad? Which are good? The start of the year is historically bad because of expenses being higher. Income is very stable for the first nine months of the year then goes up in November. Looking month by month, the only month which has averaged a net positive has been November, but October and December are close. 

Here's a quick data table to show that decline in a different format.  

{% highlight text %}
## Error in html_screenshot(x): Please install the webshot package (if not on CRAN, try devtools::install_github("wch/webshot"))
{% endhighlight %}


