---
layout: post
title:  "IMDb Top 250 Movies"
date:   2022-10-18
author: Tanner Markham
description: Getting data from the web using web scraping and API techinques!
image: /assets/images/imdb.png
---

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/imdb-top-250.jpg" alt="IMDb top 250" style="width:500px;"/>

# Introduction
One of the most wonderful things about statistics and data science, is that you can apply principles to essentially any topic that you like. One of my absolute favorite passtimes is movies, and today we're going to look at some ways that we can get a whole lot of data about some of the greatest movies of all time! One of the most popular online sources for movie information is none other than IMDb, or the internet movie database. IMDb has a curated list of the best movies ever, as determined by IMDb and its users. You can find that list [here](https://www.imdb.com/chart/top/). Unfortutely, the information available on this page is pretty limited, and you would have to select each movie individually to learn more. What if we could see a table view of the movie, its rating, its director and more? That would give us a lot more power to tell a story about these movies, and that's what we're going to learn right now. The python notebook and data can be found in [this repository](https://github.com/tdmarkham00/imdb-top250)


## Gathering Data
## Tools

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/webscraping.png" alt="Web scraping tools" style="width:500px;"/>

```
import pandas as pd
import requests
from bs4 import BeautifulSoup
from omdbapi.movie_search import GetMovie
```

In order to gather the data needed for this list, I used a combination of tools in Python. The most essential tools I used were the [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) and [requests](https://pypi.org/project/requests/). Requests allows you to send http requests to a url, and the BeautifulSoup allows you to create an html object in your python environment that you can parse through. Additionally, because IMDb requires an AWS account to access its API, I decided to use [OMDb](https://www.omdbapi.com/) API as a more accessible alternative. All of the setup instructions and documentation can be found in the hyperlinks above. Now that we have our tools ready, let's begin

## First Stop, IMDb
Because IMDb is not totally straightforward to use, our interaction with the actual IMDb website is purely for obtaining the list of movies that we need. Fortunately, this can be done through simple web scraping! Before you begin any web scraping project, please consider the privacy of the content that you are scraping and be respectful.

By combining both the requests and BeautifulSoup, you can interact with an html object in your python environment that can be parsed through to retrieve information. Creating a BeautifulSoup object looks something like this:
```
imdb_url = 'https://www.imdb.com/chart/top/' # url for top 250 list
imdb_r = requests.get(imdb_url)
imdb_soup = BeautifulSoup(imdb_r.text, 'lxml') # Creating soup object for scraping
```
After you have your "soup", you can inspect the source html until you find where the data you want is stored. In this case, the titles of the top 250 movies are stored in an element called td.TitleColumn. Using a ```soup.select()```, you can loop through that TitleColumn element and store the titles in a python list, we can call it Titles.

## So we have a list, now what?
We now have a list in python, but if we just wanted to see the list of movies we could have stopped after a visit the the IMDb website. Let's actually get some information about those movies! This is where OMDb comes in.

OMDb is a free API that provides a lot of information about these movies. In order to access this API, you just need to provide your email for authentication and you can get your API key. This key is private, so make sure not to publish it anywhere. The best thing to do is store it in a text file and save it in your environment as follows:
```
f = open("omdbkey.txt", 'r')
key = f.readline()
```

Now that your key is saved, you can call the OMDb API using ```GetMovie()``` function found in the omdbapi python library. This function takes your API key as its parameter, and gives you access to the API information. Save this call in an item called movie, ```movie = GetMovie(key)```. Because we will use this movie object frequently, it would be best to use a function to make an API call to get the information you want. Your function can look something like this:
```
def scrape(index, item):
    return movie.get_movie(title = Titles[index])[item]
```

This function takes arguments for an index and an argument for an item. The omdbapi uses a method called ```get_movie()``` which takes a movie title as an input. Remember that Titles list from before? This is where it proves its use. We can pass the Titles list at any given index to get_movie and that will in turn send the movie title at that index to the OMDb API. The item argument represents specific data that we want't whether that be runtime, genre, etc. Now that there is a good multi-use function available, we can use it to populate some new lists. For this project, I will be retrieving data about the release year, MPAA rating, runtime, genre, director and IMDb rating. We can use use list comprehension to populate these lists by looping through the titles and calling the scrape function from above.
```
# Populating lists for the rest of the desired columns

# Year
Year = [scrape(i, 'year') for i in range(0,250)]

# Rating
Rating = [scrape(i, 'rated') for i in range(0,250)]

# Runtime
Runtime = [scrape(i, 'runtime') for i in range(0,250)]

# Genre
Genre = [scrape(i, 'genre') for i in range(0,250)]

# Director
Director = [scrape(i, 'director') for i in range(0,250)]

# IMDB rating 
Score = [scrape(i, 'imdbrating') for i in range(0,250)]
```
These individual lists are now populated and ready to be combined into a pandas dataframe! Let's do that really quick!
```
top250 = pd.DataFrame({
    'Title' : Titles,
    'Year' : Year,
    'Rating' : Rating,
    'Runtime' : Runtime,
    'Genre' : Genre,
    'Director' : Director,
    'Score' : Score
})
```
There you have it, a pandas dataframe containing information about the top 250 movies according to IMDb!

# Conclusion
Statistics and data science are awesome, and allow you to tell amazing stories using data! People sometimes forget that actually getting the data can be a big chunk of the process. There are many ways to get data, and one of the coolest ways is getting data from the internet using APIs and web scraping tools. Hopefully this can give an idea of how the process works and get you excited about going and getting data for your own personal projects. Stay tuned for an additional post where we do some EDA and visualizations using this data! Now if you'll excuse me, I think I'd better go and find a good movie to watch.

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/movie.jpg" alt="Person watching a movie" style="width:700px; height:400px"/>
