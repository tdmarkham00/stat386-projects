---
layout: post
title: "How to fit a simple linear regression model in R"
date: 2022-09-16
author: Tanner Markham
description: A brief overview of how to check assumptions required for linear model and how to use remedial measures if those assumptions aren't met.
image: /assets/images/regression.png
---

# What is linear regression?
Linear regression is a widely used statistical technique that is applied in situations where you'd like to model a relationship between two sets of variables. Do you remember the equation y = mx + b? It might be something you haven't seen since you were in grade school, but this is the equation of a line. m represents the slop of that line, and b is the value of y when x is zero. At it's core, linear regression is a line! This line is special, because it's the line that is placed as perfectly as possible in between all of the data that is being analyzed. In this tutorial, we'll go over how to fit this line and use it to determine if one or more predictor variables have a strong relationship to some sort outcome variable. In the case of simple linear regression, you are looking at the relationship between only one predictor variable and it's associated outcome variable. In this post, we will go over the process of answering a data question using simple linear regression!

## Understand the data
Understanding the data is absolutely essential when do any sort of analytics. If you don't know the data, how can you know what questions to ask and how to interpret results? Trying to answer those questions will help you quickly realize that it's just not possible. In this example, we will be using a data set called stopping distance. The stopping distance data set has two compares distance in feet, with speed in MPH. These values are specific to automobiles, and we would like to test for a relationship between how fast a car goes and how many feet it takes for the car to come to a full stop. This is useful in determining what speed limits should be on the roads! Let's take a look at some values and summary statistics to try and better understand what we're looking at. The packages that are used for this analyis are tidyverse, readr, ggfortify, and car. First step is a quick read_table function with a summary.

   View(stop)              | summary(stop) 
:-------------------------:|:-------------------------:
![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/dataset.png)  |  ![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/summary.png)

After inspecting the data, we want to make sure that we have properly defined the predictor variable and the outcome variable. Looking back at the data, we remember that the idea is to see if there is any sort of relationship between the speed of the car and the distance it takes to stop. Think for a moment about which variable you'd like to use to predict the other. If you picked speed as your predictor and distance as the outcome, you'd be correct. Let's plot the relationship using a ggplot scatter plot and see what we find.

![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/scatter.png)

The scatter plot gives us a bit of an idea that there is some sort of relationship between distance and speed. Now it's time to fit that regression line!

## Creating the intial model
Creating an initial regression model in R is very easy, and can be done with the following code: model <- lm(Distance ~ Speed, data = stop). We make use of the lm() command, which takes two inputs. The first input is our Y variable and X variable seperated by the ~. In our case, we are comparing distance against speed so it looks like Distance ~ Speed. The second input just names the data set being used, or in this case the stop data set. Remember y = mx + b from earlier? Here's what it looks like after fitting this model.

![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/model1.png)

It's also important to save the residuals and fitted values that are generated from the model for use in checking assumptions. That can be done as such: stop$Fitted <- model$fitted.values, stop$Residuals <- model$residuals

## Checking assumptions
Now that we've had a chance to understand the data and define our variables, the next step is to make sure that we can actually use this model. There are six guidelines that help us determine how useful this model is. We can use the acronymn LINEAR to better understand what the assumptions are. We will also go over diagnosing problems and then fixing them.

L - Linear relationship between X and Y

I - Independence of residuals

N - Normally distributed residuals centered at zero

E - Equal variance for residuals across all values of X

A - All observations are being described

R - Required additional predictor variables

Now to discuss each on more specifically and how to deal with them!

#### Linear relationship between X and Y
Because we are doing linear regression, this assumption is vital and can't be ignored. Like most of these assumptions, we can use visual tools to diagnose a problem. For this assumption we can refer back to our scatter plot first. We will also look at another plot called the residual vs fitted plot. We won't go into too much details on these plots, mostly focusing on how to interpret them in this context.

Scatter Plot              | Residual Vs Fitted
:-------------------------:|:-------------------------:
![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/scatter.png)  |  ![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/resvsfitted.png)

You can notice that the scatter plot has a bit of a curve to it, indicating that there is not a totally linear relationship between our variables. The residual vs fitted plot is also helpful to check for linearity. If the data were linear, the blue line would run perfectly horizontal across the x-axis. In this case there is a parabolic shape, indicating a problem with linearity. In order to fix linearity issues, it's better to consider transforming X, and then Y if problems persist. We'll go over that more towards the end.

#### Independence of residuals
There is no particular "test" for checking that this assumption is met, you moreso have to think about where your data came from. If you're confident that each observation of your data has been taken independently and doesn't influence other observations, you can usually mark this assumption as met. When it isn't met, there can be bias that isn't accounted for that influences results

#### Normally distributed residuals centered at zero
Checking that the residuals are normally distributed is important for making sure that any confidence intervals that we generate from this model are accurate. There are three good ways to visualize normality, and those are histograms, box plots, and a normal probability plot. Here are some examples with their code required

Histogram              | Boxplot |  Normality Q-Q Plot
:-------------------------:|:-------------------------:|:-------------------------:
![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/hist.png)  |  ![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/box.png) | ![Figure](https://github.com/tdmarkham00/stat386-projects/raw/main/assets/images/qq.png)
