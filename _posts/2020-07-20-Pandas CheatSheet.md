---
layout: post
title:  "Pandas CheatSheet"
crawlertitle: "This is our first post"
summary: "Firebase шпаргалка"
date:   2020-09-09 23:09:47 +0700
categories: posts
tags: ['Продуктовая аналитика']
#author: Felipe
---


Шпаргалка по синтаксису Pandas.

### Creating DataFrames

{% highlight python %}
df1 = pd.DataFrame(
    {"a":[4 ,5, 6],
     "b":[7, 8, 9],
     "c":[10, 11, 12]},
     index = [1, 2, 3])

df2 = pd.DataFrame(
    [[4, 7, 10],
     [5, 8, 11],
     [6, 9, 12]],
     index=[1, 2, 3],
     columns=['a', 'b', 'c'])
{% endhighlight %}

### multiindex dataframe

{% highlight python %}
df3 = pd.DataFrame(
    {"a":[4 ,5, 6],
     "b":[7, 8, 9],
     "c":[10, 11, 12]},
     index = pd.MultiIndex.from_tuples(
             [('d',1),('d',2),('e',2)],
              names=['n','v']))
{% endhighlight %}









