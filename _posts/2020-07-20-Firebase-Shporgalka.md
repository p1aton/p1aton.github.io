---
layout: post
title:  "Firebase SQL шпаргалка"
crawlertitle: "This is our first post"
summary: "Firebase шпаргалка"
date:   2020-11-29 23:09:47 +0700
categories: posts
tags: ['Веб аналитика']
#author: Felipe
---


Шпаргалка по SQL запросам к Firebase.

### 01 Show distinct user_pseudo_id

{% highlight sql %}
SELECT
  distinct(user_pseudo_id)
FROM
  `firebase-public-project.analytics_153293282.events_20181003`
{% endhighlight %}

### 02 Table suffix

{% highlight sql %}
SELECT
  distinct(user_pseudo_id)
FROM
  `firebase-public-project.analytics_153293282.events_*` 
WHERE
  (_TABLE_SUFFIX between '20180719' and '20181122')
{% endhighlight %}

### 03 Demo and show a basic select * to look at data

{% highlight sql %}
SELECT
  *
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`
LIMIT
  100
{% endhighlight %}

### 04 Filter by event. Take a look at event_params.key with diff values

{% highlight sql %}
SELECT
  *
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`
WHERE
  event_name = "level_complete_quickplay"
{% endhighlight %}

### 05 Event name and key

{% highlight sql %}
SELECT
  *
FROM
  `firebase-public-project.analytics_153293282.events_20181003`,
  UNNEST(event_params) as param
WHERE
  event_name = "level_complete_quickplay"
  AND param.key = "firebase_screen_class"
{% endhighlight %}

### 06 Built-in functions

{% highlight sql %}
SELECT
  AVG(param.value.int_value) as avg_value,
  STDDEV(param.value.int_value) as stddev
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`,
  UNNEST(event_params) as param
WHERE
  event_name = "level_complete_quickplay"
  AND param.key = "value"
{% endhighlight %}

### 07 Group By Count

{% highlight sql %}
  SELECT
  count(param.value.int_value) as value,
  device.category
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`,
  UNNEST(event_params) as param
WHERE
  event_name = "level_complete_quickplay"
  AND param.key = "value"
 GROUP BY
  device.category 
{% endhighlight %}

### 08 Group By AVG

{% highlight sql %}
SELECT
  avg(param.value.int_value) as avg_value,
  device.category
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`,
  UNNEST(event_params) as param
WHERE
  event_name = "level_complete_quickplay"
  AND param.key = "value"
 GROUP BY
  device.category 
{% endhighlight %}

### 09 Getting all event params

{% highlight sql %}
SELECT
  event_name, param
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`,
  UNNEST(event_params) as param
WHERE
  event_name = "level_complete_quickplay"
{% endhighlight %}

### 10 To get one event param

{% highlight sql %}
SELECT
  param.value.int_value as value
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`,
  UNNEST(event_params) as param
WHERE
  event_name = "level_complete_quickplay"
  AND param.key = "value"
{% endhighlight %}

### 11 Can be written in the same way

{% highlight sql %}
SELECT
  event_name,
  (SELECT value.int_value FROM UNNEST(event_params) WHERE key = "value") AS value
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`
WHERE
  event_name = "level_complete_quickplay"
{% endhighlight %}

### 12 Still 18 rows, but with all parameters

{% highlight sql %}
SELECT
  event_name,
  (SELECT value.int_value FROM UNNEST(event_params) WHERE key = "value") as value,
  (SELECT value.string_value FROM UNNEST(event_params) WHERE key = "board") as board
 FROM
  `firebase-public-project.analytics_153293282.events_20181003`
WHERE
  event_name = "level_complete_quickplay"
{% endhighlight %}


### 13 Final query

{% highlight sql %}
SELECT
  AVG(value) as avg_value,
  MAX(board) as board_name
 FROM
  (SELECT
    event_name,
    (SELECT value.int_value FROM UNNEST(event_params) WHERE key = "value") as value,
    (SELECT value.string_value FROM UNNEST(event_params) WHERE key = "monster") as board
   FROM
    `firebase-public-project.analytics_153293282.events_20181003`
  WHERE
    event_name = "level_complete_quickplay")
GROUP BY
  board
ORDER BY
  avg_value desc
{% endhighlight %}

### 14 Actual screen progressions

{% highlight sql %}
SELECT
  screen_0,
  screen_1,
  COUNT(*) AS count
FROM (
  SELECT
    user_pseudo_id,
    event_timestamp,
    param.value.string_value AS screen_0,
    LEAD (param.value.string_value, 1) OVER (PARTITION BY user_pseudo_id ORDER BY event_timestamp ) AS screen_1
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`,
    UNNEST(event_params) AS param
  WHERE
    event_name = "screen_view"
    AND param.key = "firebase_screen_class"
  ORDER BY
    user_pseudo_id,
    event_timestamp )
WHERE
  screen_1 = "shop_menu" # IS NOT NULL
GROUP BY
  screen_0,
  screen_1
ORDER BY
  count DESC
{% endhighlight %}

### 15 A closed funnel with time constraints

Count the number of occurrences a user encountered a "start_event" event, and then the number of times
they encountered an "end_event" event after encountering the start event within a certain time window 

{% highlight sql %}
SELECT
  COUNTIF(funnel_start_time IS NOT NULL) AS funnel_begin_count,
  COUNTIF(funnel_end_time - funnel_start_time < 5 * 1000 * 1000) AS funnel_end_count
FROM (
  SELECT
    funnel_start_time,
    LEAD(funnel_end_time, 1) OVER (PARTITION BY user_pseudo_id ORDER BY event_timestamp) AS funnel_end_time
  FROM (
    SELECT
      event_name,
      IF (event_name = "level_start_quickplay",
        event_timestamp,
        NULL) AS funnel_start_time,
      IF (event_name = "level_end_quickplay",
        event_timestamp,
        NULL) AS funnel_end_time,
      user_pseudo_id,
      event_timestamp
    FROM
     `firebase-public-project.analytics_153293282.events_20181003`
    WHERE
      event_name = "level_start_quickplay"
      OR event_name = "level_end_quickplay"
#      AND _TABLE_SUFFIX BETWEEN '<yyyymmdd>'
#      AND '<yyyymmdd>'
    ORDER BY
      user_pseudo_id,
      event_timestamp) )
{% endhighlight %}



