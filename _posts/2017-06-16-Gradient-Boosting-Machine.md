---
layout: post
title:  "Gradient Boosting"
crawlertitle: "= Gradient + Boosting"
summary: "We summarized the ideas behind gradient boosting"
date:   2017-06-16 11:09:47 +0700
categories: posts
tags: ['Meetings Resources and summary']
author: Yue
---

Gradient boosting is a popular idea to improve the efficiency of weak learners.


---
1. Boosting: Sequentially adding weak learners at each stage to compensate existing weak learners.

2. Gradient: Identify shortcomings of exisiting weak learners.

---

## Algorithm

* At iteration i: model is $$F_i(x_1)$$, response is $$y_1$$, residual is $$y_1 - F_i(x_1)$$

(1) let $$h_i(x_1)$$ = $$y_1 - F_i(x_1)$$

(2) fit regression tree $$h$$ to data $$(x_1, y_1 - F_i(x_1)), (x_2, y_2 - F_i(x_2))$$ - Boosting concept to compensate the shortcomings of existing weak learners

(3) Gradient concept: 
        
Loss function $$L(y, F(x)) = (y-F(x))^2$$, 

Cost function $$J = \sum L(y_i, F(x_i))$$, treat $$F(x_i)$$ as a parameter and take derivatives:

$$\frac{dJ}{dF(x_i)} = F(x_i) - y_i$$, which is the negative residual - shortcoming, hence update $$F(x_i)$$ by the gradient:


$$F(x_i) = F(x_i) - \frac{dJ}{dF(x_i)} = F(x_i) - (F(x_i) - y_i)$$

