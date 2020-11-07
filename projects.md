---
bg: "tag.jpg"
layout: page
title: "Projects"
crawlertitle: "Projects we have done"
permalink: /projects/
summary: "Projects dones in Our community"
active: projects
---

We are an active, friendly community of data scientists based in Toronto. Trying to utilize Machine Learning techniques to solve real-world problems. 

Project Jam Weekend
---
On our project jam weekends, you are welcome to drop by and:

* pitch machine learning/data mining project you are working on: find collaboration/teaming opportunity
* short-form present your project at the end of the session
* decide the capstone/presentation date for the project and a broader community of audiences/potential business partners will be invited to attend 

or join 

Existing Projects
---

1. build toronto election ward demo database (background data for people) + integrate activity/behavior data (automatic streaming of tweets and 311 calls (city of toronto open data)

  * #### Integrated Application Goals to achieve for beneficiaries (real estate developers, small business owners, home finders/renters, city urban planning, city social service providers, MPs-election)
    
    (1) identify business development opportunities to benefit real estate developers, small business owners to open business, home finders, home renters (factors include: rent increase rate, house price rate, building types)
    
    (2) identify the face/and changing face/service request needs of the election-ward and link it to election results to benefit MPs
      - demographics, populatioin densities (demo factor): status quo and changing trend overtime
      - economic factors: unemployment rate: status quo and changing trend overtime
      - livelihood: 311 social service requests + tweets: status quo and chaning trend overtime
      - crime rate: status quo and changing trend overtime
    
    (3) identify urban change: for urban planners , city officials
  
  * #### data sources:
  
    (1) background data 
        
        - statscan census
        
        - (ward profiles toronto city)[(https://www1.toronto.ca/wps/portal/contentonly?vgnextoid=2394fe17e5648410VgnVCM10000071d60f89RCRD]
    
  * #### steps/stages: 
  
    (1) build mongodb db hosted on the cloud for the background data (), rent prices, house prices 
    
    ```
    
    **achieving:** Identify where different immigrant communities settle in GTA: (a) the status quo before the latest cencus (2) reflect the change over time (3) link it to change of election results if any + change in rent + change in house prices + change to social services request 
    
    - BeautifulSoup to scrape Stats Canada web site.
    - integrate data for mapping: ggmaps
    - integrated interactive map hosted on the cloud: explore bokeh or others
    
    Reference: http://benrifkind.github.io/Mapping-Stats-Canada-Data-with-R-part-1-of-3/
    
    ```
    
    (2) integrate (automatic streaming from 311 calls json format from website) + tweets (json format) into mongodb
    
    (3) make user interface on the mongodb for data visualization (python/d3.js)
    
    (4) predictive model for election result and others/sentiment analysis/etc.  

  * #### project lead: 
  
  ``` 
  
  Harriet,  
  
  Patrick (DATA SCIENCE AD ANAYSIS) : step 2 - automatic streaming of 311 json from city of toronto website
  
  Anna : step 2 for tweets, 3, 4 
  
  Martin: Step 2 for tweets 
  
  Holly: Step 4
  
  Ayazhan Zhakhan: step 4
  
  Ashraf Ghonaim
  
  
  
  ```
    
    
  * #### capstone date:
  
    Step 1 + 2 : Capstone date - tentative Sep 30th
  
  * #### code base / project page:
  
  * #### To-do Issues:
  
    (1) find a free cloud hosted mondb db
    
    (2) get a list of issues/topics for sraped tweets: 
    
      - use help of news report website
      
      - generate keywords for filter tweets
      
      
      
      
      
      
  


Jam Days 
---
August 19, 2017:

```

projects pitched:
 1. build toronto election ward demo database (background data for people) + integrate activity/behavior data (automatic streaming of tweets and 311 calls (city of toronto open data)
 2. driveless cars
 3. Kaggle image processing

```

August 25, 2017:

```

project pitched:

1. predictive model to identify terrorist groups responsible for an terrorist event

* data source
* GitHub repo
* slack channel
* dropbox
* project leads: Ayazhan Zhakhan, Ashraf Ghonaim, Raul Samayoa
* accomplishments: 

2. build toronto election ward demo database (background data for people) + integrate activity/behavior data (automatic streaming of tweets and 311 calls (city of toronto open data)

* slack channel: https://torontods.slack.com/messages/G6S24FRT9/details/
* GitHub repo
* project team: Harriet, Anna, Ayazhan Zhakhan, Ashraf Ghonaim
* accompolishments:
  
  (a) scrape statscan census data
  
  Things to be done:
  (a) inflow statscan census data into local mongodb
  
  ref: install mongodb on local windows: https://docs.mongodb.com/getting-started/shell/tutorial/install-mongodb-on-windows/
  
  (b) migrate it into a cloud-hosted mongodb

```

### More Information

Our list of past and future events can be found in our [meetup site](https://www.meetup.com/Toronto-Machine-Learning-Book-Club/).

Email to ask to join or speak.



### Contact us

[datascientistswithoutbordersto@gmail.com](mailto:datascientistswithoutbordersto@gmail.com)



----

This blog was created using the (awersome) static site generator [Jekyll][jekyll], and it is base on the theme found [here][jekyll-new].

[jekyll-new]: https://github.com/jglovier/jekyll-new
[jekyll]: http://jekyllrb.com/
