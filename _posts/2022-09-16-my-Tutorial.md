---
layout: post
title: "How to fit a simple linear regression model in R"
date: 2022-09-16
author: Tanner Markham
description: A brief overview of the procedures and assumptions that need to be met
image: /assets/images/regression.png
---

# What is linear regression?
Linear regression is a widely used statistical technique that is applied in situations where you'd like to model a relationship between two sets of variables. Simply put, you use regression to determine if one or more predictor variables have a strong relationship to some sort outcome variable. In the case of simple linear regression, you are looking at the relationship between only one predictor variable and it's associated outcome variable. In this post, we will go over the process of answering a data question using simple linear regression!

### Understand the data
Understanding the data is absolutely essential when do any sort of analytics. If you don't know the data, how can you know what questions to ask and how to interpret results? Trying to answer those questions will help you quickly realize that it's just not possible. In this example, we will be using a data set called stopping distance. The stopping distance data set has two compares distance in feet, with speed in MPH. These values are specific to automobiles, and we would like to test for a relationship between how fast a car goes and how many feet it takes for the car to come to a full stop. This is useful in determining what speed limits should be on the roads! Let's take a look at some values and summary statistics to try and better understand what we're looking at. The packages that are used for this analyis are tidyverse, readr, ggfortify, and car. First step is a quick read_table function with a summary.

   View(stop)              | summary(stop) 
:-------------------------:|:-------------------------:
![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/dataset.png)  |  ![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/summary.png)

After inspecting the data, we want to make sure that we have properly defined the predictor variable and the outcome variable. Looking back at the data, we remember that the idea is to see if there is any sort of relationship between the speed of the car and the distance it takes to stop. Think for a moment about which variable you'd like to use to predict the other. If you picked speed as your predictor and distance as the outcome, you'd be correct. Let's plot the relationship using a ggplot scatter plot and see what we find.

