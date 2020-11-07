---
layout: post
title:  "Hands-on Prediction"
crawlertitle: "The IBM stock"
summary: "Prediction ML on the IBM stock price"
date:   2017-05-28 11:09:47 +0700
categories: Hands-on
tags: 'Hands-on'
author: Felipe
---

Following up on our last talk on Time Series, we decided to implement machine learning techniques to forecast the IBM stock price. 

The first step was to get the historical data (for the last 5 years). We obtained this via the Yahoo Finances webpage. We then prepared the data to be fed into an LSTM neural network. We implemented the LSTM network using the Keras library.

<center>
<img src="{{ '/assets/images/27-05-2017/photo_1.jpg' | prepend: site.baseurl }}" alt=""> 
<img src="{{ '/assets/images/27-05-2017/photo_2.jpg' | prepend: site.baseurl }}" alt=""> 
</center>