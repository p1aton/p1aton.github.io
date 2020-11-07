---
bg: "owl.jpg"
layout: post
title:  "Ensemble models for time series"
crawlertitle: "Ensemble models"
summary: "Ensemble models for time series"
date:   2017-05-20 20:09:47 +0700
categories: posts
tags: ['Meetings Resources and summary']
author: Felipe
---

In this short post, we summarize Yue's talk from May 19th and share some of the resources.

Time series data comes as a sequence of values $$y_1,y_2,\ldots$$. The goal of forecasting is to be able to predict the outcome at time $$t$$ if all the previous outcomes are known. Currently, there are many techniques for doing this; you can find an excellent summary in [this paper](link) (about 50 pages long). Coming back to our goal, we want to find a predictor (function) $$f$$ such that $$y_t=f(y_1,\ldots,y_{t-1})$$.

# Recursive Neural Networks 

RNN are useful to predict sequential data, they are powerful but training them can be hard. This problem is somehow fixed with the introduction of Lont Short Term Memory. They are powerful, like really powerful. We don't dwell into the explanations here, but refer the reader to (the excellent) [Colah's post](http://colah.github.io/posts/2015-08-Understanding-LSTMs/) for an explanation or the more technical [Andrej Karpathy Blog](http://karpathy.github.io/2015/05/21/rnn-effectiveness/). Luckily for us, some people have written wrappers that allow us to create an LSTM network with five lines of code using Keras, to see how check [Siraj's video](https://youtu.be/ftMq5ps503w) on using LSTM to predict the stock market. 

# Ensemble models

The idea is simple; we have some classifiers that by themselves aren't doing well, maybe together they can do better. This is usually understood as building strong classifiers out of weak classifiers. We can do this in different ways...

### Random Forest

The decision of the majority...

### Boosting

Penalizing bad decisions...


# Why linear regression shouldn't work and what to do about it.

The data is correlated... 
