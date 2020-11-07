---
layout: post
title:  "Ecommerce Analytics - Series 1: \n CLTV and Customer Segmentation"
crawlertitle: "cltv&segmentation"
summary: "We explore using data marketing for cltv prediction and customer segmentation"
date:   2017-07-29 11:09:47 +0700
categories: posts
tags: ['Analytics Posts']
author: Yue
---

General problems and strategies in Database Marketing
---
1. Customer retention, which sometimes can be even more important than customer acquisition. For example, according to research, a 5% increase in customer retention could increase company profitability from 25% to 85%. <sup>[\[1\]](#ref1)</sup>

Issues Tackeld in this post using [an open online retail dataset](http://archive.ics.uci.edu/ml/datasets/online+retail) <sup>[\[2\]](#ref2)</sup>
---
1. How predicting customer lifetime values helps you define customer segmentation - hence derives segment specific marketing strategies

(1) Prelimiary data exploration for customer segments using basic **RFM** implementation in Python

___
* **R** means **Recency**: it is the time from the last transaction of the customer to current time
* **F** means **Frequency**: it is how many already made transactions the customer has had overall  
* **M** means **Monetary value**: it is how much money the customer has contributed overall 

```
# import online retail dataset

import pandas as pd
import datetime as dt

df = pd.read_excel("Online_Retail.xlsx", sheetnames = "Online Retail", converters={'CustomerID': str})
df['ParsedInvoiceDate'] = df['InvoiceDate'].apply(lambda x: dt.datetime.strftime(x, "%Y-%m-%d"))
df.head()
```

<img src="/assets/images/online_retail_imported.png"/> 

```
df['ParsedInvoiceDateFrequency'] = df['InvoiceDate'].apply(lambda x: pd.to_datetime(x.date(), format="%Y-%m-%d"))
df['ParsedInvoiceDateRecency'] = df['InvoiceDate'].apply(lambda x: pd.to_datetime(x.date(), format="%Y-%m-%d"))

now = dt.datetime.now()
now = now.strftime("%Y-%m-%d")

data['Sales'] = data['UnitPrice']*data['Quantity']

# Group transactions into customer: 
# frequency(how many unique days a transaction happened), recency(last transaction until today), money(sum of all spend)
data = data.groupby(['CustomerID']).agg({'ParsedInvoiceDateFrequency': lambda x: x.nunique(),
                                         'ParsedInvoiceDateRecency': lambda x: pd.to_datetime(now, format="%Y-%m-%d") -   
                                         pd.to_datetime(x.dt.date.max(), format = "%Y-%m-%d"),
                                         'Sales': lambda x: sum(x)
                                        })
data.rename(columns = {"ParsedInvoiceDateFrequency": "Frequency", 
                       "ParsedInvoiceDateRecency": "Recency",
                       "Sales": "Monetary"
                      }, inplace=True)
data.head()
```

<img src="/assets/images/rfm_df.png"/> 


Reference
---
<a name="ref1">[1]</a>: [How to Model Your Cusotmers' Lifetime Value](http://www.internetrix.com.au/blog/how-to-model-customer-lifetime-value/)

<a name="ref2">[2]</a>: [Online Retail Data Set](http://archive.ics.uci.edu/ml/datasets/online+retail)

[“Counting Your Customers” the Easy Way: An Alternative to the Pareto/NBD Model](http://mktg.uni-svishtov.bg/ivm/resources/Counting_Your_Customers.pdf)


