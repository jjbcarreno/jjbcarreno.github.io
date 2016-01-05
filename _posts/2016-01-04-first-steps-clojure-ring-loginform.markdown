---
layout: post
title:  "Ring tutorial : simple login form"
date:   2016-01-04 18:00:00
categories: ring clojure
---
## About this tutorial
This tutorial will only rely on Ring framework to implement a very simple login form / welcome page. It will enable you to understand a little bit better the following features that come up with Ring :

* Creating and using wrappers
* Using resources to render pages
* Submit and retrieve form parameters
* Work with session (memory)

## Step one : Understanding wrappers
A wrapper enables to decorate a handler with new features.
![ring wrapper]({{ site.url }}/assets/ring-wrapper.png = 200x)

