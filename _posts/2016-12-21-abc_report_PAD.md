---
layout: post
title: ""ABC Clergy Survey"
date: 2016-12-21
output:
  html_document
share: true
categories: blog
excerpt: "How do ABCUSA clergy feel about theology and politics?"
tags: [rstats]
---

## Introduction

In the fall of 2016, Dr. Ryan Burge and Dr. Paul Djupe administered an online survey of clergy from the American Baptist Churches of the USA denomination. This survey was open from October 25th until November 7th. The opportunity to take the survey was offered through an email correspondence and the Qualtrics survey platform was used to pose the questions and record the results. In total 1314 emails were sent, but 289 bounced back. The total number of clergy who began the survey was 246, however there was a significant amount of roll-off - there were some questions where the number of respondents dropped to 168.

A disclaimer must be attached to these results. This survey is not necessarily a representative sample of the clergy of the ABCUSA. This is for two primary reasons: sample size and the way that this survey was conducted. According to Wikipedia, the ABCUSA has 5,402 congregations. We received responses from less than 3% of all the pastors in the denomination. It's likely that if another random sample of 200 pastors answered this survey the answers would be different. The second reason is that this sample is not random, but a collection of all congregations for which we could connect an email address. Small congregations are typically less technologically advanced and therefore don't have an email address publicly available. The pastors that we sampled are highly educated and pastor churches that have an average attendance that is much higher than what one would assume. This is displayed below

![center](/figs/abc_report_PAD/unnamed-chunk-13-1.png)

The sample that we collected was highly educated - nearly 80% of the sample completed at least some coursework at the graduate level. 

In regard to age, responses ranged from 29 years old to 85 years old with an average age of 56.


## Church Characteristics

![center](/figs/abc_report_PAD/unnamed-chunk-14-1.png)

The average attendance figures for the sample was 115 people. The median attendance reported however was lower at 85, this is due to the fact that there were a number of churches in the sample with attendance up to 550 individuals. A finding worth noting is that there was no statistical relationship in the data between the age of the clergy and the size of their congregation or the amount of education of the respondent and church attendance.

![center](/figs/abc_report_PAD/unnamed-chunk-15-1.png)

Many of the churches in the sample did not report experiencing what the rest of the ABCUSA has been undergoing: a significant decline in members. In fact, only 18% of the clergy surveyed reported that their church had declined in size in the last year and 34% reported an increase in average attendance. Almost half the sample reported no change. Over three quarters of pastors said that they didn't feel competition from other churches for members.

When asked to describe the economic class of their members, nearly 60% of pastors said that their congregation was primarily middle class, 15% said their congregation was upper middle class, and 25% said their congregation was lower middle class.

We asked clergy to assess if their congregation was different than the community on a number of components including racial, political, and religious dimensions. The vast majority of pastors thought that their congregation stood somewhat apart from the community.

## Church Outreach

We posed a series of questions regarding how the church has tried to reach out to potential new members. They were asked if the church had sponsored an outreach event, used different worship materials, had a recruitment committee, or mailed letters or flyers to attract membership.

![center](/figs/abc_report_PAD/unnamed-chunk-16-1.png)

The results indicate that churches are attempting multiple outreach activities. Two thirds of respondents said that their church had held an outreach event and nearly half the sample indicated that they had made changes to their style of worship to be more attractive to potential new members.

## ABC Involvement

We asked pastors a series of questions regarding how much involvement the ABCUSA should have on social and political issues, fostering debate about these issues inside the religious body, and lobbying the government in regard to these issues. 

![center](/figs/abc_report_PAD/unnamed-chunk-17-1.png)

This results indicate that pastors want to see a more active denominational leadership. However, it is worth noting how support changes in light of which presidential candidate the respondent supported. The mean support score for Clinton voters was .74 (read this as 74% supportive of an active denomination), but the mean score for Trump voters was .51, with Gary Johnson voters splitting the difference at .67.

## Political Issues

![center](/figs/abc_report_PAD/unnamed-chunk-18-1.png)

In regard to general party affiliation, the sample leans slightly to the Republican Party. The distribution somewhat resembles a bell curve insofar as the most extreme options were the least likely to be chosen. The largest group were those who chose "Lean Republican." How would this translate to their choice for president in 2016?


![center](/figs/abc_report_PAD/unnamed-chunk-19-1.png)

We asked respondents who they chose to vote for in the 2016 presidential campaigns - 41% chose Hillary Clinton as their candidate while 30% chose Trump, however nearly as many clergy (29%) chose something else, whether it be not voting or casting a ballot for a third party candidate.

We followed up by asking what was the most important factor in deciding who they were going to vote for in the election. The most popular response items were: temperament, judicial appointments, and abortion.

![center](/figs/abc_report_PAD/unnamed-chunk-20-1.png)

We asked clergy to guess how their church would vote in the 2016 presidential election. The vertical dashed lines indicate the mean guess for each of the two candidates. The average vote share for Clinton was 52.2% and the average vote for Trump was 43%. What is worth reflection is that pastors who said that they would personally vote for Trump are more likely to perceive that their congregations would vote for Trump. In fact Trump voting clergy believe that, on average, 67% of their congregations are Trump supporters as well. The same pattern is true for Clinton supporting clergy, who speculate that 68% of their congregants are like them politically. It seems that Democratic clergy lead Democratic congregation, and the same is true for Republican pastors.

![center](/figs/abc_report_PAD/unnamed-chunk-21-1.png)

We presented respondents with a number of statements about the presidential election and then asked them to respond on a scale from "strong agree" to "strongly disagree." A significant portion of the sample expressed serious reservations about the impact Donald Trump is going to have on Christian witness going forward. What is more fascinating is that one third of clergy who say that they are going to vote for Trump also agree that support for him will hurt Christian witness and 30% "neither agree or disagree" with the statement. It would appear that there were serious reservations about Trump even among his supporters, however that apparently did not sway their votes.


![center](/figs/abc_report_PAD/unnamed-chunk-22-1.png)

There was overwhelming disagreement with the statement: "Moving a conservative agenda is worth any price of supporting Trump." Over 70% of the sample disagreed with that statement, even one third of Trump voters.

![center](/figs/abc_report_PAD/unnamed-chunk-23-1.png)

We also asked clergy if they think that the relationship between evangelicalism and the Republican Party is a healthy one. The clergy responded to this question with an ovewhelmingly negative attitude. Just 8% of the total sample agreed that the relationship was a positive one. Over 80% of Hillary Clinton supporters had a negative view of the link between evangelicalism and Republican politics.

![center](/figs/abc_report_PAD/unnamed-chunk-24-1.png)

We asked clergy, "How much do you trust the federal government to do what's right?" The answers were predictably pessimistic with 70% saying that they have a distrustful opinion of the government. This is actually higher than the numbers in the general population, though. There was a considerable difference on the question by political affiliation as can be seen below. Trump voters were much more distrustful of government than Clinton supporters were. 

![center](/figs/abc_report_PAD/unnamed-chunk-25-1.png)

## Clergy Political Activity

![center](/figs/abc_report_PAD/unnamed-chunk-26-1.png)

We wanted to get a handle on how willing clergy were to engage in political activities. We asked if they would be willing to wear a political button, put a bumper sticker on their car, put a political sign in their yard, participate in a demonstration, sign a petition that would appear in the local newspaper, or write a letter to their U.S. representative.

It seems that clergy are much more willing to take a less visible action than one that has higher visibility. For example, wearing a button in public, placing a sign in their yard, and putting a bumper sticker on their car were activities not frequently engaged in, while signing a petition and writing a letter were much more pervasive.


![center](/figs/abc_report_PAD/unnamed-chunk-27-1.png)

We were also interested in how clergy saw themselves and how they perceived the congregation perceived their role as political representatives. Over two thirds of the clergy had been contacted by their membership regarding political issues. However, only about a quarter of the respondents said that they had passed those concerns on to a government official. It is noteworthy that slightly more pastors feel that they are a political representative than perceive that their congregation sees them as a representative.

![center](/figs/abc_report_PAD/unnamed-chunk-28-1.png)

This is also evident in another set of questions where we asked clergy which political activities that they have engaged in. The most popular action was voting, followed by encouraging their congregation to vote.

## Clergy Theology

We presented clergy a series of six statements to assess their religious theology. For each statement, respondents were offered response options ranging from "Strongly Disagree" to "Strongly Agree." The most interesting way to look at this analysis is by dividing the sample up into those who would vote for Clinton and those who would vote for Trump.

![center](/figs/abc_report_PAD/unnamed-chunk-29-1.png)

As is apparent, there is a tremendous difference between the theology of these two groups. In all six cases, Clinton voters are more liberal on theological issues. The largest gaps appear on statements regarding biblical literalism and male authority. It's clear that political liberalism is also related to religious liberalism among ABCUSA clergy. But how do ABC clergy see themselves?

![center](/figs/abc_report_PAD/unnamed-chunk-30-1.png)

In one section of the survey we asked respondents to check boxes next to terms that they believed described them. As the graph below indicates, over half of the sample identified as evangelical. The other labels do not seem to resonate as much as about half as many chose ecumenical. It's interesting to note that 38 clergy were comfortable with the "conservative" label and 32 identified as "liberal."

## Concluding Thoughts

The ABCUSA clergy seem to be fairly divided. While this sample size is small, there are clear divisions between conservative clergy and liberal clergy on both political issues as well as theological issues. It's apparent that clergy do a good job of "sorting themselves out," however. These results indicate that conservative clergy are shepherding conservative congregations and the same is true for liberal clergy. 

When it comes to questions regarding how the ABCUSA should use this data, it seems that if a pastor was a liberal then they desire for a denomination that is more active politically and socially. Conservative pastors are less convinced that an engaged denomination is the best course of action.

Again, we must caution a reader about interpreting these results in a broad manner. These results describe the sample that we received, which as noted above is a highly educated group from a very small number of churches.

## Contact Information

If you would like to discuss this data further feel free to contact Dr. Ryan Burge (<rpburge@eiu.edu>) or Dr. Paul Djupe (<djupe@denison.edu>).

Dr. Burge's work is available on [his personal website](www.ryanburge.net)

Dr. Djupe's work is available on [his personal website](http://pauldjupe.com/)

All the data and coding for this project are available on [GitHub](https://github.com/ryanburge/abc_clergy)
