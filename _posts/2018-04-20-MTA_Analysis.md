---
layout: post
title: Fun with New York City MTA data
---

## The Data Science Journey

![alt text][logo]

[logo]: http://www.yardi.com/blog/wp-content/uploads/2013/10/MTA-logo-279x300.jpg "Logo Title Text 2"

This post will take you through the data science process, start-to-finish, including (especially!) the less glamorous parts. We will be working with publicly-available New York City MTA data. Our goal is to answer the following questions:

1. Which MTA stations have the most entries?
2. Which days of the week are busiest?
3. Which times of day are busiest?
4. Can we find anything interesting through a deep dive into one station?

My goal is to to provide as much insight as possible into my thought processes as I experienced them in real time. Let's begin our journey!

#### Tools of the trade

In this analysis, I will use Python and several well-known packages, including numpy, pandas, and matplotlib. I will complete the work in a Jupyter notebook, called ***ABCXYZ.ipynb*** which is available at my github repository [here UPDATE LATER](https://www.google.com).

#### Accessing the Data

The first stage of the data science journey often begins at the same point as many of life's questions: Google. Through some Googling, I was able to locate the [MTA data](http://web.mta.info/developers/turnstile.html). Clicking on a few of the links, I notice a few things, which may prove to be useful:

1. The data appears to be in a csv format, which can easily be imported into pandas as a dataframe.
2. Each file represents one week of ridership data
3. The URLs arethe same with the exception of six digits, which represent the date.

We may want to work with multiple weeks of data, so I will create a function that allows me to automatically pull data from the website, taking advantage of the predictable URL names. This function is called combine_mta_data in the Jupyter notebook and takes a valid start date and number of weeks of data as inputs.

#### Understanding the Data

To start, we will pull three weeks of data starting September 2, 2017. Once we have pulled the data, it is helpful to look at it in pandas. A few initial observations:

1. Entries and Exits appear to be cumulative.
2. It appears that we do not have any null values.
3. The data appears to be grouped at a greater level of granularity than station. The documentation found [here ](https://data.ny.gov/Transportation/Turnstile-Usage-Data-2016/ekwu-khcy) reveals the data is grouped by turnstile and collected every 4 hours.

#### Cleaning and Simplifying the Data

Considering this data in the context of our four questions above, there are a few operations we can perform to cleanup this data.

1. We can convert the DATE and TIME fields to a single datetime object and convert our dataframe to a time series.
2. There seem to be some inconsistencies with the TIME field as the entries are not always in neat four-hour increments. We can attempt to aggregate the entries so we have a reading for each station approximately every four hours.
3. There are entries labelled as "RECOVR AUD" in the DESC field. According to the documentation, these are missed audits which were recovered. The documentation mentions these are omitted when they are duplicates.
4. There are several duplicate entries which we should remove.
5. All of our analysis will be done at the station level, so we can eventually group the data by station and create a more manageable dataframe.
