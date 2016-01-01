---
layout: post
title:  "First steps with Ring"
date:   2016-01-01 18:00:00
categories: ring clojure
---
# What is Ring ?
[Ring](https://github.com/ring-clojure/ring) is a HTTP server abstraction. 
In other words, it is a web application library for Clojure. It provides the following :

# How to setup Ring in a Clojure project ?

#### Using leiningen
First, include Ring dependency to our Leiningen project definition file (project.clj). 	
{% highlight clojure %}
(defproject blog "1.0.0-SNAPSHOT"
  :description "FIXME: write"
  :dependencies [[org.clojure/clojure "1.7.0"]
                 [ring/ring-core "1.4.0"]
                 [ring/ring-jetty-adapter "1.4.0"]])
{% endhighlight %}
* ring-core : namespace that contains main ring functions for HTTP abstraction (parameters, cookies...)
* ring-jetty-adapter : Convert ring handlers into a running jetty web server

#### Using boot
To do


{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
