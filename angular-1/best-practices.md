# Angular Best Practices
_reference: <https://app.pluralsight.com/player?course=angularjs-patterns-clean-code&author=john-papa&name=angularjs-patterns-clean-code-m2>_

## Seperation of Concerns 
Seperation of concerns involves isolating files into very small chunks that do one thing, and do it well. This allows for more reusability and ease of testing/finding and isolating bugs. 

## The Rule of One

1 Component, 1 Role, 1 File

do one thing and do it well. For example, controllers should ONLY handle logic for a view, not handle things like logging and making API calls. 

It is best practice to break down applications into common (generic) modules for things like logging, and then also break down functionality (screens, flow, etc) into modules as well. All these modules are now reusable, and can be included into one large angular application. 

## Application Structure

### General Principles

General application structure is as follows:

`/app` angular code
`/content` assets
`/vendor` 3rd-party code
`/test` tests

It is better to sort large applications by feature instead of by type: 

```
//by feature
app/
	dashboard/
	layout/
	people/
	...

//by type
app/
	controllers/
	services/
	views/
```

by feature lends better to modularity

### the LIFT Principle

(In order of importance)
*L* Locating code is easy
*I* Identify the code at a glance; looking at a filename should quickly explain what it does
*F* Flat; keep the application as flat as possible; only as deep as you need
*T* Try not to repeat yourself (i.e no need to call a file session-view.html since the `.html` extension implies that it is a view. just call it session.html)

## Modularity in Angular

Good tobreak an app into many modules such that they are more reusable as opposed to building one big monolithic application

### Defining Multiple Module Dependencies

each moudle can be an app, services, or widgets (custom directives)
modules often depend on other modules

modules are just containers for AngularJS components

module getter/setter:

```js
//setter
var setApp = angular.module('myModule', [/* dependencies */]);

//getter
var getApp = angular.module('myModule');
```

below is an example of how you would create smaller modules and add them as dependencies to a larger, overall module:

```js
// sub modules, each of which can utilize their own dependencies
angular.module('myModule1');
angular.module('myModule2');
angular.module('myModule3');

//overall module
angular.module('overallModule', [
	//Angular Modules
	...,

	//3rd Party Module
	,,,.

	//Custom Modules
	'myModule1', 
	'myModule2', 
	'myModule3'
]);
```

You may notice above that the modules are broken into three main categories:
- AngularJS Modules: provides by the AngularJS framework
- 3rd party Modules: vendor modules taken from outside the project
- Custom Modules: modules that are written in the project

when breaking modules into packages, it is good to keep the actual definition of the module in a seperate file completely so it's easy to find and consume. It can be helpful to name it something like `myModule.module.js` so it's clear what it is.

## Declaring Controllers without an App on the Global Namespace

when creating modules, we don't want to pollute the global namespace. In order to avoid doing this, we can use an `IIFE` and an Angular module getter as seen below: 

```js
// NOTE: controller name is 'myController' and it 
// is a part of the 'myModule' module.

// IIFE prevents pollution of the global namespace
(function() {
	
	// get the module, and immediately bind a controller to it
	angular
		.module('myModule')
		.controller('myController', MyController);

	// can bind services to controller function like this
	// instead of passing an array to 'angularModule.controller's second argument

	MyController.$inject = [/* INJECT SEVICES HERE */]

	// controller logic can come last if declared as a function
	// this is due to hoisting

	function MyController( /* PLACE INJECTED SERVICES HERE */ ) {
		// controller logic goes here
		// a good structure for this is to place all the parameters 
		// that are going to be exposed at the top of the file,
		// and then define them further down
		// this enables you to quickly glance at the controller and 
		// determine what items are on it.

		// quickly shows all values on the controller
		// note: this is using the _controller as _ syntax,
		// could also have been done using '$scope' instead of 'this'

		// this seems to be the standard for avoiding issues while using 'this'
		//within a function call
		var vm = this;

		vm.someVar = 'foobar';
		vm.someOtherVar = false;
		vm.someFunc = someFunc;
		vm.otherFunc = otherFunc;


	}
})()
```

adding a watch with 'Controller as' syntax:

```js
...
var vm = this;

vm.title = 'The Title';

$scope.$watch(function() {
	return vm.title;
}, function(newVal, oldVal) {
	// some code that runs on change
	// of above value
});
```

you can also specify "Controller as" fro mthe configuration of a route, and can also use the propery `resolve` within `config` to bind variables to the controller. These will be received by specifying the name of the return value as a service on the controller:

```js
//example route
{
	url: '/some-path',
	config: {
		templateUrl:  '/path/to/templat.html',
		title: 'Some Title',
		controller: 'MyController',
		controllerAs: 'vm',
		resolve: {
			// 'someVal' will be available on MyController as a service that can be injected.
			// 'someVal.prop' will be equal to 'value'. keep in mind that resolve can only be used
			// when controller as is specified from the route. It will not work if specified from the template
			// Can also inject services (such as $q if it's an async call) into these values

			someVal: function( /* INJECTED SERVICES HERE */ ) {
				return {prop: 'value'}
			}
		}
	}
}
```

can also use "resolveAlways" to add a resolved value to every route. 