---
layout: post
title:  "Hands-on Speech Recognition"
crawlertitle: "Querying on InstaKart"
summary: "We designed a simple query implementation for the InstaKart dataset"
date:   2017-06-03 11:09:47 +0700
categories: Hands-on
tags: 'Hands-on'
author: Felipe
---


The goal of this meetup was to setup a specch recognizer to answer queries from the [InstaKart dataset](https://tech.instacart.com/3-million-instacart-orders-open-sourced-d40d29ead6f2). We divide the 
problem into 3 parts, of which we have achieved only the first two.

<center>
<img src="{{ '/assets/images/03-06-2017/photo_1.jpg' | prepend: site.baseurl }}" alt=""> 
</center>


1. Find a speech recognition API and make it work. 
2. Query on a given product information. More specifically, recognize a product name and look for similar products in the dataset and return the location of the product.
3. Make the products returned in part 2 relevant. Beef up the speech recognition to get semantic meaning. 

The first part was more complicated than expected. Eventually we made it work, the idea is to download the [Python Speech Recognition API](https://pypi.python.org/pypi/SpeechRecognition/) and all its required dependencies, in particular we need pyaudio to get audio from a microphone. 

For the second part we download the InstaKart data and create methods that lookup for a key word among all the products. See more details [here](https://github.com/TorontoDataScientistsWithoutBorders/speech_recognition_InstaKart/blob/master/speech.ipynb).   