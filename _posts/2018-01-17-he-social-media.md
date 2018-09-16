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

I was approached by the marketing department of a Scottish university who were in the process of reviewing their social media strategy. I was asked to look at their Twitter activity to compare their use of the platform to their competitors.

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


