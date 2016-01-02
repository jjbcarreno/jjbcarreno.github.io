---
layout: post
title:  "First steps with Ring"
date:   2016-01-01 18:00:00
categories: ring clojure
---
## What is Ring ?
[Ring](https://github.com/ring-clojure/ring) is a HTTP server abstraction. 
In other words, it is a web application library for Clojure. It consists in the following concepts :

* **Request** : map of data that represent the HTTP Request. It has a set of predefined keys but you can create custom ones.
* **Response** : It's a map too, It also has several keys defined by default. 
* **Handler** : Function that takes a request (a map) and returns a response (another map)
* **Middleware** : Enables to enhance a handler with new features. it takes a handler and returns a new handler.

## How to setup Ring in a Clojure project ?

### Using leiningen
First, include Ring dependency to your Leiningen project definition file (project.clj). 	
{% highlight clojure %}
(defproject myproject "1.0.0-SNAPSHOT"
  :description "A simple Ring App"
  :dependencies [[org.clojure/clojure "1.7.0"]
                 [ring/ring-core "1.4.0"]
                 [ring/ring-jetty-adapter "1.4.0"]])
{% endhighlight %}
2 namespaces are used in the above example :
* ring-core : contains main ring functions for HTTP abstraction (parameters, cookies...)
* ring-jetty-adapter : convert ring handlers into a running jetty web server

### Using boot
To do

## Creating your first "Hello World" application
Here is an piece of code (in src/myproject/core.clj): 
{% highlight clojure %}
;; (ns...) : It's used to declare a namespace. This namespace should match 
;; file's path, except that "/" is replace by "." and "_" becomes "-".
;; Example : src/my_project/core.clj would be my-project.core namespace.
(ns blog.core
;; ns Function can receive several arguments. :require is declared here so that 
;; we can use jetty adapter
  (:require [ring.adapter.jetty :as jetty]))

;; Hello world handler. It takes a "request" as parameter and returns 
;; a "response" map 
(defn myApp [request]
  { :status 200
    :headers {"Content-Type" "text/html"}
    :body "<h1>Hello World</h1>"})

;; Define a jetty server that will run on 8080
;; defonce will run a jetty server once evaluated
;; :join option is used to ensure that current thread blocks 
;; until server is loaded. 
(defonce server (jetty/run-jetty #'myApp {:port 8080 :join? false}))
{% endhighlight %}

Now, to run that example and test our web application, a repl can be used.
Just go to project root folder and run 'lein repl'. You should see the following.

{% highlight bash %}
$ lein repl
nREPL server started on port 62419 on host 127.0.0.1 - nrepl://127.0.0.1:62419
REPL-y 0.3.7, nREPL 0.2.10
Clojure 1.7.0
Java HotSpot(TM) 64-Bit Server VM 1.8.0_45-b14
    Docs: (doc function-name-here)
          (find-doc "part-of-name-here")
  Source: (source function-name-here)
 Javadoc: (javadoc java-object-or-class-here)
    Exit: Control+D or (exit) or (quit)
 Results: Stored in vars *1, *2, *3, an exception in *e

user=> (use 'myproject.core)
2016-01-02 08:57:13.420:INFO::nREPL-worker-0: Logging initialized @20703ms
2016-01-02 08:57:13.468:INFO:oejs.Server:nREPL-worker-0: jetty-9.2.10.v20150310
2016-01-02 08:57:13.509:INFO:oejs.ServerConnector:nREPL-worker-0: Started ServerConnector@50fa17de{HTTP/1.1}{0.0.0.0:8080}
2016-01-02 08:57:13.510:INFO:oejs.Server:nREPL-worker-0: Started @20793ms
nil
{% endhighlight %}



