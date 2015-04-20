---
layout: post
title:  "AngularJS + ionic - Generic Services"
date:   2015-04-18 20:17:13
categories: AngularJS ionic
---

If you are coding in [AngularJS][] for web applications or for mobile applications, it is always good to get your own building blocks of codes that you can use in either ones. In this article I am introdusing the way I use to build my rusable code blocks. The rule here is that if you want to shoft to new library then you have to do minor changes to your project to accomodate the new library. In this article we will show how to attach yourself loosley to using AngularJS `$http`, and make your project ready to detach from it and use any new service that may make your life easier.  

The following is an AngularJS module that creates services, the module name is `shangHttp`, and as the name implies it is a broker to deal with your backend server `DB` and supplies you with data from your RESTful services that you can right in your favourite servier side flavours (PHP, ASP.NET...etc).

The following code is a part of ionic project and you may find some ionic factories and services such as `$ionicLoading`.


{% highlight javascript linenos=table%}
/* 
* A gerneric module that deals with server side RESTful services
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

`shangHttp.mypost` is a javascript promise, we know it is a promise because it returns the result of `$http.post` and we know that `$http.post` is a promise, this makes our function `shangHttp.mypost` the same promise. If you are now familiar with promises please study it more on `jquery` website as it seems easier than it is in AngularJS.

Then we use our service in our controllers like so:

{% highlight javascript linenos=table%}
var controllers = module('shangControllers', ['ionic', 'shangServices']);
 
controllers.controller('FirstController', ['$scope', 'shangHttp','$ionicLoading', function ($scope, shangHttp, $ionicLoading) {
	$scope.items=[];
	$scope.getItems = function (id) {
		fd = {action:'dogetitems', item_id: id};
		$ionicLoading.show();
		shangHttp.mypost('getjson.php', fd)
			.success (function (data, status, headers, config){
				for (var a in data){
					$scope.items.insert(data[a]);
				}
			})
			.error(function(data, status, headers, config){
				$scope.error ='A problem happened while ...etc';
			});
	};
}
]);
controllers.constant('$ionicLoadingConfig', {
    template: '<i class="icon ion-load-c"></i><br/>Wassup...'
});

{% endhighlight %}

In the above module named `shangControllers` we reference the servcies module to use it. Then we use it in the the controller the same way as if we are using the origonal promise `$http.post`. But we have a level of felxibility here as we may want to use a third party promise to fetch server data, other than `$http.post`, in this case instead of editin each and every controller that is using `$http.post`  to use the new promise, we only change our service `shangHttp` to use the new RESTful third party module.


[ionic]:      	http://ionicframework.com
[angularjs]:  	http://angularjs.org/
