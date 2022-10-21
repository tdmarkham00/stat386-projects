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
One of the most wonderful things about statistics and data science is that you can apply their principles to any topic that you like. One of my absolute favorite passtimes is movies, and today we're going to look at some ways that we can get data about some of the greatest movies of all time! One of the most popular sources for movie information is none other than IMDb, or the internet movie database. IMDb and its users have curated a list of the best movies ever. You can find that list [here](https://www.imdb.com/chart/top/). The information available on the page is pretty limited, so you would have to select each movie individually to learn more. What if we could see a table view of the movie, its rating, its director and more? That would give us a lot more power to tell a story about these movies, and that's what we're going to learn right now. Follow along with the python notebook in [this repository](https://github.com/tdmarkham00/imdb-top250).


## Gathering Data
## Tools

<img src="https://raw.githubusercontent.com/tdmarkham00/stat386-projects/main/assets/images/webscraping.png" alt="Web scraping tools" style="width:500px;"/>

```
import pandas as pd
import requests
from bs4 import BeautifulSoup
from omdbapi.movie_search import GetMovie
```

In order to gather the necessary data, a combination Python tools will be used. [Requests](https://pypi.org/project/requests/) allows you to send http requests to a url, and [BeautifulSoup](https://pypi.org/project/beautifulsoup4/) allows you to store an html object in your python environment that you can parse through. Because IMDb requires an AWS account to access its API, I decided to use the [OMDb](https://www.omdbapi.com/) API as a more accessible alternative. All of the setup instructions and documentation can be found in the hyperlinks above. Now that the tools are ready, it's time to begin.

## First Stop, IMDb
Any interaction with the actual IMDb website is purely for obtaining the list of movies that are needed. Fortunately, this can be done through simple web scraping! Before beginning a web scraping project, please consider the privacy of the content that you are scraping and be respectful. By combining both the requests and BeautifulSoup libraries, you can store the html of a webpage and scrape through it to find information. Creating a BeautifulSoup object looks something like this:
```
imdb_url = 'https://www.imdb.com/chart/top/' # url for top 250 list
imdb_r = requests.get(imdb_url) # Sending request to the website
imdb_soup = BeautifulSoup(imdb_r.text, 'lxml') # Creating soup object for scraping
```
After you have your "soup", inspect the source html until you find the location of the data within the source code. In this case, the titles of the top 250 movies are stored in an element called td.TitleColumn. Using a ```soup.select()```, loop through that TitleColumn elements and store the titles in a python list, which can be called Titles.

## So we have a list, now what?
If anyone just wanted to see the list of top movies, they could have stopped after a visit the the IMDb website. Now it's time to actually get some information about those movies! This is where OMDb comes in.

OMDb is a free API that provides a lot of information about these movies. In order to access this API, just provide an email for authentication to get an access API key. This key is private, so make sure not to publish it anywhere. The best thing to do is store it in a text file and save it in your environment as follows:
```
f = open("omdbkey.txt", 'r')
key = f.readline()
```

Now that the key is saved, call the OMDb API using the ```GetMovie()``` function found in the omdbapi python library. This function takes the API key as its parameter, and gives access to the API information. Save this call in an item called movie, ```movie = GetMovie(key)```. Because this movie object will be used frequently, it would be best to create a function to make an API call to get the desired information. That function can look something like this:
```
def scrape(index, item):
    return movie.get_movie(title = Titles[index])[item]
```

This function takes arguments for an index and an argument for an item. The omdbapi uses a method called ```get_movie()``` which takes a movie title as an input. Remember that Titles list from before? This is where it proves its use. Pass the Titles list at any given index to get_movie and that will send the movie title at that index to the OMDb API. The item argument represents specific data, whether that be runtime, genre, etc. Now that there is a good multi-use function available, use it to populate some new lists. For this project, I will be retrieving data about the release year, MPAA rating, runtime, genre, director and IMDb rating. Use list comprehension to populate these lists by looping through the titles and calling the scrape function from above.
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
