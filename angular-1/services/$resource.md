#$resource
_reference: <https://code.angularjs.org/1.5.10/docs/api/ngResource/service/$resource>_

special note: be sure to install and include `ngResource` as a dependency in your `app.module()` declaration.

##What is it?
A factory which creates a resource object that lets you interact with RESTful server-side data sources. 
The returned resource object has action methods which provide high-level behaviors without the need to interact with the low level $http service.

By default, trailing slashes will be stripped from the calculated URLs, which can post problems with server backends that do not expect that behavior. This can be disabled by configuring the `$resourceProvider` like this: 

```js
app.config(['$resourceProvider', function($resourceProvider) {
	//Don't strip the trailing slashes from the URLs
	$resourceProvider.defaults.scriptTrailingSlashes = false;
}]);
```

##Dependencies
* $http
* $log
* $q
* $timeout

##Usage
`$resource(url, [paramDefaults], [actions], options);`

##Arguments
* `url` - **string** - a parameterized URL template with parameters prefixed by `:` as in `/user/:username`
* `paramDefaults` (optional) - **object** - default value for url parameters. these can be overridden in the actions methods. If a parameter value is a function, it will be called every time a param value needs to be obtained for a request. 
* `actions`(optional) - **Object.<Object>=**
