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

### Reshaping Data

{% highlight python %}
df4 = pd.melt(df1, var_name='var', value_name='val')
df4.pivot(columns='var', values='val')
df5 = pd.concat([df1, df2], axis=1)
{% endhighlight %}

###  Sorting, drop, rename

{% highlight python %}
df1.sort_index(ascending=False)
df1.sort_values('a', ascending=False)
df1.drop(columns=['b', 'c'])
df1.rename(columns = {'a':'c1', 'b':'c2', 'c':'c3'})
{% endhighlight %}

### Subset Rows

{% highlight python %}
df = pd.concat([df1, df2])
df.a >= 5  # return bool series
df[df.a >= 5]  # extract rows that meet logical criteria
df.drop_duplicates()  # remove duplicate rows
df.head(2)  # select first 2 rows
df.tail(2)  # select last 2 rows
df.sample(frac=0.5)  # randomly select fraction of rows
df.sample(n=3)  # randomly select n rows
df.iloc[2:4] # select rows by position
df.nlargest(3, 'b')  # select and order top 3 entries of column b
df.nsmallest(3, 'c') # select and order bottom 3 entries of column c
{% endhighlight %}

### Subset Columns

{% highlight python %}
df.a
df['a']
df[['a', 'b']]
df.filter(regex='^b$') # slect columns whose name matches regex
df.loc[:, 'b':'c'] # select all columns between b and c
df.iloc[:, [1,2]]  # select columns in positions 1, 2 (first is 0)
df.loc[df['a'] > 5, 'b':'c']
{% endhighlight %}

### Summarize Data

{% highlight python %}
df['a'].value_counts()
len(df)
df['a'].nunique()
df.describe()  # basic descriptive statistics for each column
df.sum()
df.count()
df.median()
df.quantile([0.25, 0.75, 0.90])
df.apply(sum)
df.min()
df.max()
df.mean()
df.var()
df.std()
{% endhighlight %}

### Handling Missing Data

{% highlight python %}
foo = df4.pivot(columns='var', values='val')
foo.dropna()
foo.fillna(1)
{% endhighlight %}

### Make New Columns

{% highlight python %}
df.assign(d=lambda df: df.a*df.b)  # Compute and append new columns
df['d'] = df.a*df.b  # Add single column
{% endhighlight %}

### Group Data

{% highlight python %}
df.groupby(by="col") # Return a GroupByobject, grouped by values in column named "col".
df.groupby(level="ind") # Return a GroupByobject, grouped by values in index level named "ind".
{% endhighlight %}

### GroupBy Functions

{% highlight python %}
size() # Size of each group.
agg(function) # Aggregate group using function.
shift(1) # Copy with values shifted by 1.
rank(method='dense') # Ranks with no gaps.
rank(method='min') # Ranks. Ties get min rank
rank(pct=True) # Ranks rescaled to interval [0, 1].
rank(method='first') # Ranks. Ties go to first value.
shift(-1) # Copy with values lagged by 1.
cumsum() # Cumulative sum.
cummax() # Cumulative max.
cummin() # Cumulative min.
cumprod() # Cumulative product.
{% endhighlight %}

### Windows

{% highlight python %}
df.expanding() # Return an Expanding object allowing summary functions to be applied cumulatively.
df.rolling(n) # Return a Rolling object allowing summary functions to be applied to windows of length n.
{% endhighlight %}

### Plotting

{% highlight python %}
df.plot.hist() # Histogram for each column
df.plot.scatter(x='w',y='h') # Scatter chart using pairs of points
{% endhighlight %}

### Combine Data Sets

{% highlight python %}
#Standard Joins
pd.merge(adf, bdf,how='left', on='x1') # Join matching rows from bdfto adf.
pd.merge(adf, bdf,how='right', on='x1') # Join matching rows from adfto bdf.
pd.merge(adf, bdf,how='inner', on='x1') # Join data. Retain only rows in both sets.
pd.merge(adf, bdf,how='outer', on='x1') # Join data. Retain all values, all rows.

#Filtering Joins
adf[adf.x1.isin(bdf.x1)] # All rows in adfthat have a match in bdf.
adf[~adf.x1.isin(bdf.x1)] # All rows in adfthat do not have a match in bdf.

#Set-like Operations
pd.merge(ydf, zdf) # Rows that appear in both ydfand zdf(Intersection).
pd.merge(ydf, zdf, how='outer') # Rows that appear in either or both ydfand zdf(Union).
pd.merge(ydf, zdf, how='outer', indicator=True).query('_merge == "left_only"').drop(columns=['_merge']) # Rows that appear in ydfbut not zdf(Setdiff).
{% endhighlight %}

[pandas cheat sheet](https://pandas.pydata.org/Pandas_Cheat_Sheet.pdf)











