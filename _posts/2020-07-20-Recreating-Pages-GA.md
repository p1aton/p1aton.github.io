---
layout: post
title:  "GA запросы для анализа страниц"
crawlertitle: "This is our first post"
summary: "GA аналитика страниц"
date:   2020-11-29 23:09:47 +0700
categories: posts
tags: ['Веб аналитика']
#author: Felipe
---


Запросы для детального анализа страниц данных GA в GBQ.

### 01 Pages

{% highlight sql %}
SELECT
  hits.page.pagePath
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
  UNNEST(GA.hits) AS hits
GROUP BY
  hits.page.pagePath
{% endhighlight %}

### 02 Pageviews

{% highlight sql %}
SELECT
  hits.page.pagePath,
  COUNT(*) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
  UNNEST(GA.hits) AS hits
WHERE
  hits.type = 'PAGE'
GROUP BY
  hits.page.pagePath
ORDER BY
  pageviews DESC
{% endhighlight %}

### 03 Unique Pageviews

{% highlight sql %}
SELECT
  pagepath,
  COUNT(*) AS pageviews,
  COUNT(DISTINCT session_id) AS unique_pageviews
FROM (
  SELECT
    hits.page.pagePath,
    CONCAT(fullVisitorId, CAST(visitStartTime AS STRING)) AS session_id
  FROM
    `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
    UNNEST(GA.hits) AS hits
  WHERE
    hits.type = 'PAGE')
GROUP BY
  pagePath
ORDER BY
  pageviews DESC
{% endhighlight %}

### 04 Average Time on Page

{% highlight sql %}
SELECT
  hits.page.pagePath,
  COUNT(*) AS pageviews
FROM
  `bigquery-public-data.google_analytics_sample.ga_sessions_20170801`  AS GA,
  UNNEST(GA.hits) AS hits
WHERE
  hits.type = 'PAGE'
GROUP BY
  hits.page.pagePath
ORDER BY
  pageviews DESC
{% endhighlight %}

### 05 Exits

{% highlight sql %}
SELECT
  pagePath,
  SUM(exits) AS exits
FROM (
  SELECT
    hits.page.pagePath,
    CASE
      WHEN hits.isExit IS NOT NULL THEN 1
      ELSE 0
    END AS exits
  FROM
    `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
    UNNEST(GA.hits) AS hits)
GROUP BY
  pagePath
ORDER BY
  exits DESC
{% endhighlight %}

### 06 Total Time on Page

{% highlight sql %}
SELECT
  pagePath,
  pageviews,
  exits,
  total_time_on_page,
  CASE
    WHEN pageviews = exits THEN 0
    ELSE total_time_on_page / (pageviews - exits)
  END AS avg_time_on_page
FROM (
  SELECT
    pagePath,
    COUNT(*) AS pageviews,
    SUM(IF(isExit IS NOT NULL,
        1,
        0)) AS exits,
    SUM(time_on_page) AS total_time_on_page
  FROM (
    SELECT
      fullVisitorId,
      visitStartTime,
      pagePath,
      hit_time,
      type,
      isExit,
      CASE
        WHEN isExit IS NOT NULL THEN last_interaction - hit_time
        ELSE next_pageview - hit_time
      END AS time_on_page
    FROM (
      SELECT
        fullVisitorId,
        visitStartTime,
        pagePath,
        hit_time,
        type,
        isExit,
        last_interaction,
        LEAD(hit_time) OVER (PARTITION BY fullVisitorId, visitStartTime ORDER BY hit_time) AS next_pageview
      FROM (
        SELECT
          fullVisitorId,
          visitStartTime,
          pagePath,
          hit_time,
          type,
          isExit,
          last_interaction
        FROM (
          SELECT
            fullVisitorId,
            visitStartTime,
            hits.page.pagePath,
            hits.type,
            hits.isExit,
            hits.time / 1000 AS hit_time,
            MAX(IF(hits.isInteraction IS NOT NULL,
                hits.time / 1000,
                0)) OVER (PARTITION BY fullVisitorId, visitStartTime) AS last_interaction
          FROM
            `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
            UNNEST(GA.hits) AS hits)
        WHERE
          type = 'PAGE')))
  GROUP BY
    pagePath)
ORDER BY
  pageviews DESC
{% endhighlight %}

### 07 Entrances

{% highlight sql %}
SELECT
  pagePath,
  SUM(entrances) AS entrances
FROM (
  SELECT
    hits.page.pagePath,
    CASE
      WHEN hits.isEntrance IS NOT NULL THEN 1
      ELSE 0
    END AS entrances
  FROM
    `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
    UNNEST(GA.hits) AS hits)
GROUP BY
  pagePath
ORDER BY
  entrances DESC
{% endhighlight %}

### 08 Bounce Rate

{% highlight sql %}
SELECT
  fullVisitorId,
  visitStartTime,
  pagePath,
  CASE
    WHEN hitNumber = first_interaction THEN bounces
    ELSE 0
  END AS bounces
FROM (
  SELECT
    fullVisitorId,
    visitStartTime,
    hits.page.pagePath,
    totals.bounces,
    hits.hitNumber,
    MIN(IF(hits.isInteraction IS NOT NULL,
        hits.hitNumber,
        0)) OVER (PARTITION BY fullVisitorId, visitStartTime) AS first_interaction
  FROM
    `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
    UNNEST(GA.hits) AS hits)
{% endhighlight %}

### 09 Sessions

{% highlight sql %}
SELECT
  fullVisitorId,
  visitStartTime,
  pagePath,
  CASE
    WHEN hitNumber = first_hit THEN visits
    ELSE 0
  END AS sessions
FROM (
  SELECT
    fullVisitorId,
    visitStartTime,
    hits.page.pagePath,
    totals.visits,
    hits.hitNumber,
    MIN(hits.hitNumber) OVER (PARTITION BY fullVisitorId, visitStartTime) AS first_hit
  FROM
    `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
    UNNEST(GA.hits) AS hits)
{% endhighlight %}

### 10 Bounce Rate

{% highlight sql %}
select
pagePath,
bounces,
sessions,
CASE
    WHEN sessions = 0 THEN 0
    ELSE bounces / sessions
  END AS bounce_rate
  from (
SELECT
  pagePath,
  SUM(bounces) AS bounces,
  SUM(sessions) AS sessions
FROM (
  SELECT
    fullVisitorId,
    visitStartTime,
    pagePath,
    CASE
      WHEN hitNumber = first_interaction THEN bounces
      ELSE 0
    END AS bounces,
    CASE
      WHEN hitNumber = first_hit THEN visits
      ELSE 0
    END AS sessions
  FROM (
    SELECT
      fullVisitorId,
      visitStartTime,
      hits.page.pagePath,
      totals.bounces,
      totals.visits,
      hits.hitNumber,
      MIN(IF(hits.isInteraction IS NOT NULL,
          hits.hitNumber,
          0)) OVER (PARTITION BY fullVisitorId, visitStartTime) AS first_interaction,
      MIN(hits.hitNumber) OVER (PARTITION BY fullVisitorId, visitStartTime) AS first_hit
    FROM
      `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
      UNNEST(GA.hits) AS hits))
GROUP BY
  pagePath)
ORDER BY
  sessions DESC
{% endhighlight %}

### 11 Exit rate

{% highlight sql %}
SELECT
  pagePath,
  pageviews,
  exits,
  CASE
    WHEN pageviews = 0 THEN 0
    ELSE exits / pageviews
  END AS exit_rate
FROM (
  SELECT
    pagepath,
    COUNT(*) AS pageviews,
    SUM(exits) AS exits
  FROM (
    SELECT
      hits.page.pagePath,
      CASE
        WHEN hits.isExit IS NOT NULL THEN 1
        ELSE 0
      END AS exits
    FROM
      `bigquery-public-data.google_analytics_sample.ga_sessions_20170801` AS GA,
      UNNEST(GA.hits) AS hits
    WHERE
      hits.type = 'PAGE')
  GROUP BY
    pagePath)
ORDER BY
  pageviews DESC
{% endhighlight %}