---
layout: post
title:  "Переписываем SQL-запросы в Pandas"
crawlertitle: "This is our first post"
summary: "SQL-запросы в Pandas"
date:   2020-09-19 23:09:47 +0700
categories: posts
tags: ['Продуктовая аналитика']
#author: Felipe
---


Как переписать свои SQL-запросы в Pandas.


{% highlight python %}
import pandas as pd

#Reset Pandas Index
df.reset_index(drop=True, inplace=True)

#Simply add a columns with an easy calculation
df['total_cost'] = df['price'] * df['quantity']
{% endhighlight %}

### USEFUL COMMANDS

{% highlight python %}
df.head(10) # visualize top data
df.describe # describe statistics data
df.shape # shape of the dataframe

#Check number of null data
df.<nameofcolumn>.isna().sum()
df.<nameofcolumn>.isnull().sum()
{% endhighlight %}

### INSERT

{% highlight python %}
#create table heroes (id integer, name text);

#insert into heroes values (1, 'Harry Potter');
#insert into heroes values (2, 'Ron Weasley');
df1 = pd.DataFrame({'id': [1, 2], 'name': ['Harry Potter', 'Ron Weasley']})

#insert into heroes values (3, 'Hermione Granger');
df2 = pd.DataFrame({'id': [3], 'name': ['Hermione Granger']})

pd.concat([df1, df2]).reset_index(drop=True)
{% endhighlight %}

### UPDATE

{% highlight python %}
#update airports set home_link = 'http://www.lawa.org/welcomelax.aspx' where ident == 'KLAX'
airports.loc[airports['ident'] == 'KLAX', 'home_link'] = 'http://www.lawa.org/welcomelax.aspx'
{% endhighlight %}

### DELETE

{% highlight python %}
#delete from lax_freq where type = 'MISC'
lax_freq = lax_freq[lax_freq.type != 'MISC']
lax_freq.drop(lax_freq[lax_freq.type == 'MISC'].index)
{% endhighlight %}

### SELECT DATA

{% highlight python %}
#select * from airports
airports
#select * from airports limit 3
airports.head(3)
#select id from airports where ident = 'KLAX'
airports[airports.ident == 'KLAX'].id
#select distinct type from airport
airports.type.unique()

#select * from airports where iso_region = 'US-CA' and type = 'seaplane_base'
airports[(airports.iso_region == 'US-CA') & (airports.type == 'seaplane_base')]
#select ident, name, municipality from airports where iso_region = 'US-CA' and type = 'large_airport'
airports[(airports.iso_region == 'US-CA') & (airports.type == 'large_airport')][['ident', 'name', 'municipality']]
{% endhighlight %}

### SELECT DATA AND ORDER BY

{% highlight python %}
#select * from airport_freq where airport_ident = 'KLAX' order by type
airport_freq[airport_freq.airport_ident == 'KLAX'].sort_values('type')
#select * from airport_freq where airport_ident = 'KLAX' order by type desc
airport_freq[airport_freq.airport_ident == 'KLAX'].sort_values('type', ascending=False)
{% endhighlight %}

### TOP N RECORDS

{% highlight python %}
#select iso_country from by_country order by size desc limit 10
by_country.nlargest(10, columns='airport_count')
#select iso_country from by_country order by size desc limit 10 offset 10
by_country.nlargest(20, columns='airport_count').tail(10)
{% endhighlight %}

### SELECT NESTED IN / NOT IN

{% highlight python %}
#select * from airports where type in ('heliport', 'balloonport')
airports[airports.type.isin(['heliport', 'balloonport'])]
#select * from airports where type not in ('heliport', 'balloonport')
airports[~airports.type.isin(['heliport', 'balloonport'])]
{% endhighlight %}

### GROUP BY / COUNT / ORDER BY

{% highlight python %}
#select iso_country, type, count(*) from airports group by iso_country, type order by iso_country, type
airports.groupby(['iso_country', 'type']).size()
#select iso_country, type, count(*) from airports group by iso_country, type order by iso_country, count(*) desc
airports.groupby(['iso_country', 'type']).size().to_frame('size').reset_index().sort_values(['iso_country', 'size'], ascending=[True, False])
#select iso_country, type, count(*) from airports group by iso_country, type order by iso_country, type
airports.groupby(['iso_country', 'type']).size()
#select iso_country, type, count(*) from airports group by iso_country, type order by iso_country, count(*) desc
airports.groupby(['iso_country', 'type']).size().to_frame('size').reset_index().sort_values(['iso_country', 'size'], ascending=[True, False])
{% endhighlight %}

### HAVING

{% highlight python %}
#select type, count(*) from airports where iso_country = 'US' group by type having count(*) > 1000 order by count(*) desc
airports[airports.iso_country == 'US'].groupby('type').filter(lambda g: len(g) > 1000).groupby('type').size().sort_values(ascending=False)
{% endhighlight %}

### AGGREGATE FUNCTIONS: MIN, MAX, MEAN

{% highlight python %}
#select max(length_ft), min(length_ft), avg(length_ft), median(length_ft) from runways
runways.agg({'length_ft': ['min', 'max', 'mean', 'median']})
{% endhighlight %}

### JOIN

{% highlight python %}
#select airport_ident, type, description, frequency_mhz from airport_freq join airports on airport_freq.airport_ref = airports.id where airports.ident = 'KLAX'
airport_freq.merge(airports[airports.ident == 'KLAX'][['id']], left_on='airport_ref', right_on='id', how='inner')[['airport_ident', 'type', 'description', 'frequency_mhz']]
{% endhighlight %}

### UNION ALL

{% highlight python %}
#select name, municipality from airports where ident = 'KLAX' union all select name, municipality from airports where ident = 'KLGB'
pd.concat([airports[airports.ident == 'KLAX'][['name', 'municipality']], airports[airports.ident == 'KLGB'][['name', 'municipality']]])
{% endhighlight %}

### PANDAS DATASET MANIPULATION

{% highlight python %}
#read dataset
data = pd.read_csv('https://s3-eu-west-1.amazonaws.com/shanebucket/downloads/uk-500.csv')
#set a numeric id for use as an index for examples.
data['id'] = [random.randint(0,1000) for x in range(data.shape[0])]

#Single selections using iloc and DataFrame
#Rows:
data.iloc[0] # first row of data frame (Aleshia Tomkiewicz) - Note a Series data type output.
data.iloc[1] # second row of data frame (Evan Zigomalas)
data.iloc[-1] # last row of data frame (Mi Richan)
#Columns:
data.iloc[:,0] # first column of data frame (first_name)
data.iloc[:,1] # second column of data frame (last_name)
data.iloc[:,-1] # last column of data frame (id)

#Multiple row and column selections using iloc and DataFrame
data.iloc[0:5] # first five rows of dataframe
data.iloc[:, 0:2] # first two columns of data frame with all rows
data.iloc[[0,3,6,24], [0,5,6]] # 1st, 4th, 7th, 25th row + 1st 6th 7th columns.
data.iloc[0:5, 5:8] # first 5 rows and 5th, 6th, 7th columns of data frame (county -> phone1).

#reset index using a name (text)
data.set_index("last_name", inplace=True)

#Select rows with index values 'Andrade' and 'Veness', with all columns between 'city' and 'email'
data.loc[['Andrade', 'Veness'], 'city':'email']
#Select same rows, with just 'first_name', 'address' and 'city' columns
data.loc['Andrade':'Veness', ['first_name', 'address', 'city']]

#Change the index to be based on the 'id' column
data.set_index('id', inplace=True)
#select the row with 'id' = 487
data.loc[487]

#Select rows with first name Antonio, # and all columns between 'city' and 'email'
data.loc[data['first_name'] == 'Antonio', 'city':'email']

#Select rows where the email column ends with 'hotmail.com', include all columns
data.loc[data['email'].str.endswith("hotmail.com")]  

#Select rows with last_name equal to some values, all columns
data.loc[data['first_name'].isin(['France', 'Tyisha', 'Eric'])]  

#Select rows with first name Antonio AND hotmail email addresses
data.loc[data['email'].str.endswith("gmail.com") & (data['first_name'] == 'Antonio')] 

#select rows with id column between 100 and 200, and just return 'postal' and 'web' columns
data.loc[(data['id'] > 100) & (data['id'] <= 200), ['postal', 'web']] 
 
#A lambda function that yields True/False values can also be used.
#Select rows where the company name has 4 words in it.
data.loc[data['company_name'].apply(lambda x: len(x.split(' ')) == 4)] 

#Selections can be achieved outside of the main .loc for clarity:
#Form a separate variable with your selections:
idx = data['company_name'].apply(lambda x: len(x.split(' ')) == 4)
#Select only the True values in 'idx' and only the 3 columns specified:
data.loc[idx, ['email', 'first_name', 'company']]
{% endhighlight %}

### OUTPUT PANDAS FUNCTIONS

{% highlight python %}
df.to_csv(...)  # csv file
df.to_hdf(...)  # HDF5 file
df.to_pickle(...)  # serialized object
df.to_sql(...)  # to SQL database
df.to_excel(...)  # to Excel sheet
df.to_json(...)  # to JSON string
df.to_html(...)  # render as HTML table
df.to_feather(...)  # binary feather-format
df.to_latex(...)  # tabular environment table
df.to_stata(...)  # Stata binary data files
df.to_msgpack(...)	# msgpack (serialize) object
df.to_gbq(...)  # to a Google BigQuery table.
df.to_string(...)  # console-friendly tabular output.
df.to_clipboard(...) # clipboard that can be pasted into Excel
{% endhighlight %}

### EASY PANDAS PLOT

{% highlight python %}
top_10.plot(
    x='iso_country', 
    y='airport_count',
    kind='barh',
    figsize=(10, 7),
    title='Top 10 countries with most airports')
{% endhighlight %}

### DEALING WITH TIME

{% highlight python %}
import datetime as dt

#convert string date to datetime
df['date'] = df.date.apply(lambda x: dt.strptime(x, '%Y-%m-%d'))

#convert datetime to string
df['date'] = df.date.apply(lambda x: dt.strftime(x, '%d-%m-%y'))

#easily convert pandas timestamp
df['time_new'] = pd.to_datetime(df.time).dt.strftime('%H:%M:%S')
{% endhighlight %}





[How to rewrite your SQL queries in Pandas, and more](https://medium.com/jbennetcodes/how-to-rewrite-your-sql-queries-in-pandas-and-more-149d341fc53e)











