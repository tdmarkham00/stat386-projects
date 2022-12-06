---
layout: post
title:  "EDA with IMDb Top 250"
date:   2022-11-15
author: Tanner Markham
description: Performing EDA and making visualizations on the IMDb Top 250 dataset
image: /assets/images/imdb.png
---

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/imdb-top-250.jpg" alt="IMDb top 250" style="width:500px;"/>

# Introduction
In today's world, we have access to limitless amounts of data. Data professionals have the responsibility of turning data into information that is usable and informative. There are two key words from the previous sentence that are important to highlight: usable and informative. This post will focus on EDA, which are steps that should be followed to make data usable and presentable. In order to provide examples, we will use data from the [IMDb Top 250 list](https://www.imdb.com/chart/top/), which was gathered in a [previous post](https://tdmarkham00.github.io/stat386-projects/2022/10/18/web-scraping.html). Follow along with the python notebook in [this repository](https://github.com/tdmarkham00/imdb-top250).

## What is EDA?
EDA stands for exploratory data analysis, which is the next step in the data analytics process after retrieving your data. The aim of EDA is to better understand our data and prepare it for more sophisticated analysis. EDA is extremely important because it is sort of the last line of defense against "garbage in, garbage out", meaning that this is our chance to catch any errors or inconsistencies in the data before it is used for more inferential statistics.

## Using EDA to understand the data
Think of the analytics process like building a house. 

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/rawmaterials.jpg" alt="Raw materials" style="width:400px;"/>

Pictured above are some materials that can be used to build a house. To people who are unfamiliar with construction, these raw materials don't really do much good. Just like these raw materials, unfamiliar data won't do an analyst much good because it's very difficult to build something without first understanding what you have and how it works. Similar to how a contractor would want to know that their tools work properly, or that they have the correct materials, those working with data need to ensure that they have the best data for the problem. EDA is sort of the bridge between sourcing materials and actually building our figurative home.

## EDA for IMDb Top 250
In a previous post, we sourced our materials needed for our project. The materials are data about the IMDb Top 250 movies. Before any sort of building begins, it's time to understand this data and make sure that it makes sense. Checking out the shape of the data and looking for things like NA values or duplicate rows is a good start.

### Verify that the data was properly sourced
Let's start by checking the shape of our data.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/shape-screenshot.png" alt="Screenshot" style="width:600px;"/>

As expected, we have the correct shape of data, so that looks good. It would be wise also to check for NA values and duplicates. We can see a count of NA values by column as such:

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/null-screenshot.png" alt="Screenshot" style="width:300px;"/>

Note that Rating, Director, Score, and BoxOffice all have at least one NA value. The box office column doesn't bother me too much because there are a lot of older movies that don't have any box office numbers. The other columns are a bit more concerning, and should be investigated. The investigation identified a problem in the scraping tool which was causing incorrect titles to be submitted to the API. After modifying the scraping tool, better results were obtained. This is a perfect example of how it's important to verify your data was gathered correctly.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/null2-screenshot.png" alt="Screenshot" style="width:300px;"/>

Also note that the BoxOffice and Runtime variables are string objects, so let's remove those characters and convert them to numeric values. It will also be useful to display our box office figures in terms of millions of dollars for more interpretable results. I'm also going to create a new column called decade, which might make working with the years a little bit easier later on.
```
for char in [',', '$']:
    top250['BoxOffice'] = top250['BoxOffice'].str.replace(char, '')

top250['BoxOffice'] = top250['BoxOffice'].astype(float)
top250['BoxOffice'] = round(top250['BoxOffice'] * .000001)


top250['Runtime'] = top250['Runtime'].str.extractall('(\d+)').unstack()[0][0].astype(int)
top250.rename({'Runtime' : 'Runtime (in minutes)', 'BoxOffice': 'BoxOffice (in millions USD)'}, axis=1, inplace=True)

conditions = [
    (top250['Year'] >= 1920) & (top250['Year'] < 1930),
    (top250['Year'] >= 1930) & (top250['Year'] < 1940),
    (top250['Year'] >= 1940) & (top250['Year'] < 1950),
    (top250['Year'] >= 1950) & (top250['Year'] < 1960),
    (top250['Year'] >= 1960) & (top250['Year'] < 1970),
    (top250['Year'] >= 1970) & (top250['Year'] < 1980),
    (top250['Year'] >= 1980) & (top250['Year'] < 1990),
    (top250['Year'] >= 1990) & (top250['Year'] < 2000),
    (top250['Year'] >= 2000) & (top250['Year'] < 2010),
    (top250['Year'] >= 2010) & (top250['Year'] < 2020),
    top250['Year'] >= 2020
]

values = ['1920s', '1930s', '1940s', '1950s', '1960s', '1970s', '1980s', '1990s', '2000s', '2010s', '2020s']
top250['Decade'] = np.select(conditions, values)
```
As part of understanding and cleaning the data, it can be important to do things like remove outliers or select a subset of variables, but that won't be necessary here. We're also going to leave the NA values as they are but that would be something you would want to consider when working with your own data.

Now that we understand our variables a bit better and have ensured that the data is clean. Let's explore some relationships between different variables.

### Using visualizations for EDA
People say a picture is worth a thousand words, and that can be true with EDA. Some of the next EDA steps will be done using visualizations. Before we begin, we need to make sure that we have the proper packages installed. There are a number of excellent visualization tools in Python, and today we'll be using matplotlib and seaborn. It's also helpful to have some sort of question in mind while making visualizations to help guide the process. One of the biggest reasons movies are made is to make money, so I'm going to focus on how the variables in our data relate to box office performance. 

```
import matplotlib.pyplot as plt
import seaborn as sns
```

First thing I'd like to do is understand the relationships between my numerical data, which can be done using a correlation matrix.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/correlation-matrix.png" alt="Screenshot" style="width:500px;"/>

If I wanted to use this data to build some sort of linear model, this correlation matrix quickly tells me that a simple linear regression with any of my numerical columns won't really be helpful for predicting anything.

If I were a movie producer, I might be curious about what factors I should consider to make the most money possible. One day two writers come to my office with a their amazing movie scripts: one is a more family friendly action film and the other is a gritty thriller. Based on experience, I can tell that they will both be well reviewed but I need to make sure I'm maximixing my profits. I can refer to other critically-acclaimed films to help make a decision.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/barplots.png" alt="Screenshot" style="width:1000px;"/>

Based on these well-reviewed movies, action/adventure/animated movies that are rated at most PG-13 tend to perform the best at the box office. This makes sense because those types of movies typically capture a wider audience. If I was prioritizing profits above all else, I may look at this and decide to produce the family friendly movie before the thriller.

Let's make use of that decades column that was created earlier and see how box office numbers are distributed across the last several decades.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/boxplot.png" alt="Screenshot" style="width:950px;"/>

I'm sure there are several reasons one might want to look at box office performance over the years. Again, I'm a movie producer who is trying to determine the next movie I should produce. It's been determined that the studio wants to move forward with some sort of nostalgia-type movie. This data is now useful because I can look back at which decade had high-performing movies and try to capture that audience again.

Although there was not much of a correlation between IMDb ratings and box office performance, we can still plot their relationship here to see how the distribution looks.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/jitter.png" alt="Screenshot" style="width:500px;"/>

This view might but be the most useful, but it's important to explore these types of things to help determine what type of information will be useful moving forward.

# Conclusion
As you might have realized, there are many, many ways to go about exploratory data analysis. The purpose of EDA is to help better understand the data and guide the analytics process as you learn more about the variables and their relationships. It's also a good checkpoint for verifying that you have good data and there aren't any issued stemming from the data collection. Going back to house-building, we spent time making sure our raw materials were flawless and we understand how the materials work together. We're now more prepared to begin drawing up some more concrete plans and building something! I hope that this post was informative for those interested in the EDA process!
