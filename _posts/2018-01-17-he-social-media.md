---
title: "Twitter benchmarking analysis for higher education"
date: '2018-01-17'
excerpt: Project for a Scottish university to analyse their Twitter activity and engagement
layout: post
project: yes
tag:
- social media
- r
- analytics
- twitter
- higher education
comments: no
---

<center><strong>University social media analysis and benchmarking</strong></center>

<br>

I was approached by the marketing department of a Scottish university who were in the process of reviewing their social media strategy. I was asked to look at their Twitter activity to compare their use of the platform to that of their competitors.

To perform this research, I wanted to be able to extract and analyse Twitter timeline data in bulk, across multiple Twitter screen names. To do this, I first created a developer account in Twitter that would allow me to extract data via the API.

With a Twitter developer account created, I could then turn to the statistical programming language R to write the code to extract the data and perform the analysis.

Using the `rtweet` package for R, I was able to automate a process to extract the Twitter timelines for a list of institutions. These tweets were then assembled into a dataframe for analysis.

```
library(rtweet)
library(purr)

# prepare list of universities
institutions_list <- c("uni_a", "uni_b", "uni_c", ...)

# extract tweets
timeline_tweets <- map_df(institutions_list, get_timeline, n = 3200)

```

With the tweets now available in a convenient, local format, I began the analysis, beginning with comparisons of tweet frequency, original content vs retweets and summarising tweet engagement.

After looking at differences in engagement between the timelines of the chosen universities, I next performed sentiment analysis on the timeline tweets, as well as mentions of the universities on third party timelines, to look for differences in positive and negative sentiment in how universities were communicating and in how they were being discussed.

For this analysis, I used the Bing lexicon, as it provides a straightforward word set for looking at basic positive / negative word use. Sentiment analysis was performed using tidy data principles and the `tidytext` package for R.

```
# load required packages
library(dplyr)
library(tidytext)
library(tidyr)

# create tokenised one word per row dataset
tweet_words <-
  timeline_tweets %>%
  filter(isRetweet == FALSE) %>%
  unnest_tokens(word, text) %>%
  anti_join(stop_words)
 
# fetch sentiment lexicon and join to tokenised tweets  
bing_sent <- get_sentiments("bing")

tweet_words_bing <-
  tweet_words %>%
  inner_join(bing_sent)
  
# summarise net positive / negative sentiment (net_sent)  
tweet_sent_one <- tweet_words_bing %>%
  group_by(screenName, id, sentiment) %>%
  summarise(sent_count = n()) %>%
  spread(sentiment, sent_count) %>%
  ungroup()
  
tweet_sent_one[is.na(tweet_sent_one)] <- 0

tweet_sent_one <-
  tweet_sent_one %>%
  mutate(net_sent = positive - negative)
```

By looking at these engagement and sentiment summary statistics, as well as original content to retweet ratio and other features, the client was able to look at the performance of their Twitter activity against that of their competitors in a new level of detail and make informed decisions regarding their future use of the network.

In addition, network analysis and identification of the people who were most engaged revealed potential routes for future influencer marketing activity.
