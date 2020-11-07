---
layout: post
title:  "Simulation versus Optimization Methods for Estimation"
crawlertitle: "simulation estimation"
summary: "We explore using simulation versus optimization methods for estimation"
date:   2017-08-02 11:09:47 +0700
categories: posts
tags: ['Bayesian and Simulation']
author: Yue
---

# Topic Outline

1. Bayesian VS. Frequentist Perspective on Interpretation of Probability
---

* **Frequentist:** long term frequency of event happening in recurring experiment

* **Bayesian:** a measure of belief in confidence about an event happening
  - This belief is based on individuals and different observers may come out different beliefs
  - Individuals' different beliefs do not change what the outcome will come out to be
  - You gather evidences to form and update beliefs
  - Prior Probability: initial belief $P(A)$
  - Posterior Probability: updated belief given evidences $P(A|X)$


2. Why Simulation Methods and When
---
* (1) Simulation versus Asymptotic Assumed Optimization Methods

  - the latter uses asymptotic assumptions in population properties) for closed-form solutions while the former uses available observed available samples

* (2) Concept of Simulation

  - you have observed realizations from some target distribution
  - you try to build a pseudo-random number generator that mimics the underlying generation process of the observed realization

* (3) Types of Simulation

  - Resampling: bootstrap, jacknife, permutation


3. Non-parametric Bayesian versus MCMC
---

# Resampling
___

Parametric Bootstrap
---

Bayesian Bootstrap
---

* purpose: estimate distribution for parameter

* Example:
(1) observed samples: array([ 3.34135494,  4.03678023,  0.4305536 ,  4.11717223]), which is actually from exponential(7)
(2) plot density
(3) need to simulation distribution of mean

# Case Studies

1. Click-through Rate
---
[Kaggle Click-through Rate Prediction](https://www.kaggle.com/c/avazu-ctr-prediction)

Reference
---
(1)[http://www.unc.edu/~carsey/teaching/ICPSR-2011/Sim%20Slides%202%20Handout.pdf]

(2) [Simulation-based Estimation](http://www.people.virginia.edu/~sns5r/resint/simulstf/simuljelt.pdf)
