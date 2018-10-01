#$http 
_reference: <https://code.angularjs.org/1.5.11/docs/api/ng/service/$http>_

the `$http` serice is a core AngularJS service that facilitates communication with the remote HTTP servers via the browser's XMLHttpRequest object or via JSONP.

##General Usage
the `$http` service is a function that takes a single _congiguration object_ argument that is used to generate an http request and returns a _promise_.

```js
$http({
	method: 'GET',
	url: '/someURL'
	}).then(function successCallback(response) {
		//this callback will be called asynchronously
		//when the response is available
	}, function errorCallback(response) {
		//this callback will be called asynchronously if an error occurs
		//or the server returns a response with an error status
	})
```

The response object has these properties: 
* data – `{string|Object}` – The response body transformed with the transform functions.
* status – `{number}` – HTTP status code of the response.
* headers – `{function([headerName])}` – Header getter function.
* config – `{Object}` – The configuration object that was used to generate the request.
* statusText – `{string}` – HTTP status text of the response.

Note: A response code between 200-299 indicates a success and will trigger the success callback. Any code outside of that will trigger the error callback. a status of _-1_ usually means the request was aborted 

##Shortcut methods
Shortcut methods are avaialble for all major types of requests. These methods require a url to be passed as the first argument. POST and PUT methods require a data object as the second argument, and all shortcut methods hava an optional config object that can be passed as the last argument. 

```js
$http.get('/someURL', config).then(successCallback, errorCallback)
$http.post('/someURL', data, config).then(successCallback, errorCallback)
```

other shortcut methods:
* $http.get
* $http.head
* $http.post
* $http.put
* $http.delete
* $http.jsonp
* $http.patch

##Setting HTTP Headers

