---
layout: post
title:  "Storytelling with IMDb Top 250"
date:   2022-12-06
author: Tanner Markham
description: Telling a data story using IMDb Top 250
image: /assets/images/imdb.png
---

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/imdb-top-250.jpg" alt="IMDb top 250" style="width:500px;"/>

# Introduction
Today, data is generated at a faster rate than ever before. According to [this 2018 article](https://www.forbes.com/sites/bernardmarr/2018/05/21/how-much-data-do-we-create-every-day-the-mind-blowing-stats-everyone-should-read/?sh=5f2abc7960ba) from Forbes, 90% of data today has been generated in the past two years. That was back in 2018, so I expect that number could be even higher based on how much data is generated every day. With data literally everywhere you look, it may seem impossible to understand it all and know what is useful. One of the end goals of any data process is coming up with some sort of deliverable that is easy to understand and more importantly, tells a story about the data that was used.

## Analytics process
If you've followed along with any of my previous posts you should be familiar with the [IMDb Top 250 dataset](https://www.imdb.com/chart/top/), which is a list of the Top 250 hightest rated movies on the IMDb website. The goal of this dataset was to demonstrate an end-to-end analytics process. The first step of course was gathering the data, and I showed how to do that using a combination of web scraping and API requests in [this post](https://tdmarkham00.github.io/stat386-projects/2022/10/18/web-scraping.html). After the data was gathered, I showed how to validate that the data was gathered correctly and went over basic EDA steps to help formulate questions that you might want to answer. You can see that post [here](https://tdmarkham00.github.io/stat386-projects/2022/11/15/my-project-3-copy.html). I've previously mentioned an interest in box-office performance, so now we can combine everyhing we've done so far to tell a story about box-office numbers with a simple visual. This visual was created using [Tableau](https://www.tableau.com), which is an amazing tool for visualizing data. Also feel free to check out the [github repository](https://github.com/tdmarkham00/imdb-top250) containing the code used throughout the process.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/Dashboard.png" alt="Screenshot" style="width:1000px;"/>

## What's the story?
In this visualization, I have broken down the box-office performance of these movies based on genre and MPAA rating. I also have a timeline of box office performance over the past 20 years or so. Before you make any sort of inference about data, it's important to note how the data was collected, so let's remind ourselves that this data is not necessarily representative of all movies, but just those found in the IMDb Top 250 list. That being said, what can we learn about this data? Which movies are the highest box-office performers? The visualization quickly tells you that PG-13/G/PG movies of some sort of Action/Animation genre are often the top performers? Why is that I wonder? In my opinion, this is attributed to a lot of family-friendly action franchises such as Marvel, Harry Potter, etc. Movie studios are becoming increasingly risk averse, so why bother thinking of original films when you can just churn out big-buget sequels and remakes that seemingly print money? I think it's pretty obvious to anyone who looks at movies playing in theaters to notice that you are seeing a lot the same types of movies. Why? The data suggests that these types of movies make a lot of money and aren't very risky. For better or for worse, leveraging data has transformed the movie industry.

# Conclusion
I hope that these posts have been informative and inspired you to go get your own data and begin asking questions. No matter what you are passionate about, I can almost guarantee that you can find data about the topic and tell your own stories. Good luck out there!
