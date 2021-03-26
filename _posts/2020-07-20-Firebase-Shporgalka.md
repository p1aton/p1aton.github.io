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