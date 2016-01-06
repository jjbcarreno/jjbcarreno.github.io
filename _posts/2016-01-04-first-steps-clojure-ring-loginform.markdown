---
layout: post
title:  "Ring tutorial : simple login form"
date:   2016-01-04 18:00:00
categories: ring clojure
---
## About this tutorial
This tutorial will only rely on Ring framework to implement a very simple login form / welcome page. It will enable you to understand a little bit better the following features that come up with Ring :

* Creating and using middlewares
* Using resources to render pages
* Submit and retrieve form parameters
* Work with session (memory)

## Step one : Understanding middlewares
A middleware enables to wrap a incoming request and response to include new features.

![ring wrapper]({{ site.url }}/assets/ring-wrapper.png)

For instance, let's create a wrapper to print request and response maps :
{% highlight clojure %}
;; Print request and response
(defn wrap-logger [handler]
  (fn [request]
    (println "========================= REQUEST BEGIN ========================")
    (println "Request:")
    (pprint request)
    (println "---------------------------------------------------")
    (let [response (handler request)]
      (println "Response :")
      (pprint response)
      (println "======================= REQUEST END ==========================")
      response)))

(def app
  (-> handler
    (wrap-logger)))
;; OR
;;(def app (wrap-logger handler))
{% endhighlight %}
Your app handler will now be intercepted by the wrapper that will now log request.

Ring provides built-in middlewares that we will use in this tutorial :

* ring.middleware.reload/wrap-reload : enables to hot deploy your source code modifications
* ring.middleware.params/wrap-params : parse form/query parameters and provide a map in the request to easily get them
* ring.middleware.sessoin/wrap-session : handle http session (in memory by default)

Let's use them all :

{% highlight clojure %}
;; declare a new namespace that will use middlewares
(ns login.core
	(:require [ring.adapter.jetty :refer [run-jetty]]
          [clojure.pprint :refer [pprint]]
          [ring.middleware.reload :refer [wrap-reload]]
          [ring.middleware.params :refer [wrap-params]]
          [ring.middleware.session :refer [wrap-session]]
          [ring.util.response :refer [status resource-response]]))
          
;; ... some code here
;; ... some code here

;; Our app will now be reloaded, wrap params, use sessions and log request/responses :
(def app
   (-> handler
     (wrap-reload)
     (wrap-logger)
     (wrap-params) 
     (wrap-session)))
{% endhighlight %}

## Step 2 :  Handling form submission / authentication
Let's now create a handler that will perform a simple authentication from a login form submission and redirect to a welcome page.

### Login form
The first step is to create a simple login form. Let's use an html file (login.html). Note that this file has to be placed on the classpath of your application :
{% highlight html %}
<html>
<head>
<title>Login Page</title>
</head>
<body>
	<form action="/login" method="post" >
		<label for="login">Login</label> <input size="20" type="text"
			name="login" /> <label for="password">Password</label> <input
			size="20" type="password" name="password" /> <input type="submit"
			value="Login" />
	</form>
</body>
</html>
{% endhighlight %}

Now let's define a function that will load this file and send it to the http response :
{% highlight clojure %}
;; Let's define a login form in html
(defn show-login-form []
  (resource-response "login.html"))
{% endhighlight %}

### Authentication function
authentication will use the request parameters, check authentication and populate a new attribute in the session (:username)
{% highlight clojure %}
;; Authenticates user. If user is authenticated, then session will contain :username.
(defn do-authenticate [params session]
  (if (and (= (params "login") "bob") (= (params "password") "M@rl3y"))
    (assoc session :username "bob")
    session))
{% endhighlight %}

### Handler 
{% highlight clojure %}
;; authorization function
(defn handler [{session :session params :form-params :as req}]
  (let [session (do-authenticate params session)
        username (:username session)]
    (if username
      (show-welcome-page username session)
      (show-login-form))))
{% endhighlight %}
