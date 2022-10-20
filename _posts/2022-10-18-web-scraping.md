---
layout: post
title:  "IMDb Top 250 Movies"
date:   2022-10-18
author: Tanner Markham
description: Getting data from the web using web scraping and API techinques!
image: /assets/images/imdb.png
---

# Introduction
One of the most wonderful things about statistics and programming, is that you can apply principles to essentially any topic that you like. One of my absolute favorite passtimes is movies, and today we're going to look at some ways that we can get a whole lot of data about some of the greatest movies of all time! One of the most popular online sources for movie information is none other than IMDb, or the internet movie database. IMDb has a curated list of the best movies ever, as determined by IMDb and its users. You can find that list [here](https://www.imdb.com/chart/top/). Unfortutely, the information available on this page is pretty limited, and you would have to select each movie individually to learn more. What if we could see a table view of the movie, its rating, its director and more? That would give us a lot more power to tell a story about these movies, and that's what we're going to learn right now.


## Gathering Data
## Tools

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/webscraping.png" alt="" style="width:400px;"/>

In order to gather the data needed for this list, I used a combination of tools in Python. The most essential tools I used were the [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) and [requests](https://pypi.org/project/requests/). Requests allows you to send http requests to a url, and the BeautifulSoup allows you to create an html object in your python environment that you can parse through. Additionally, because IMDb requires an AWS account to access its API, I decided to use [OMDb](https://www.omdbapi.com/) API as a more accessible alternative. All of the setup instructions and documentation can be found in the hyperlinks above. Now that we have our tools ready, let's begin

## First Stop, IMDb



