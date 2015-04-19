---
layout: post
title:  "AngularJS best practices - Services"
date:   2015-04-18 20:17:13
categories: AngularJS
---

If you are AngularJS for web applications or for mobile applications it is always better to get your own building blocks of codes that you can use in either ones. In this article I am introdusing the way I use to build my rusable code blocks.

The following is an AngularJS module that creates a service names ’shangHttp’, and as the name implies it is a broker to deal with your backend server and supplies you with data from your RESTful services.

{% highlight javascript %}
/* 
 * 
 */
var url = "http://www.mydomain.com/phpengine/";
var serv = angular.module('shangServices', ['ngCordova']);

serv.service('shangHttp', ['$http', function ($http) {
        return {
            mypost: function (resource, data) {
                return $http.post(url + resource, data);
            },
            myget: function (resource) {
                return $http.get(url + resource);
            }
        };
    }]);

{% endhighlight %}
Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
