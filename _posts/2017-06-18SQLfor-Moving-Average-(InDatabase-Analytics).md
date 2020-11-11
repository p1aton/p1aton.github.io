---
layout: post
title:  "SQL for Moving Average - (In-database Analytics)"
crawlertitle: "sql analytics"
summary: "We explore time series analytics possiblitiy with SQL"
date:   2017-06-18 11:09:47 +0700
categories: posts
tags: ['Analytics Posts']
author: Yue
---




This short MySQL script shows an aproach to compute the moving average.


#### MySQL - 7 Day Moving Average

```SQL
      SELECT 
      FROM signups
      JOIN signups as sigups_past 
      ON signups_past.date between signup.date - 6 and signups.date
      GROUP BY 1, 2
```