#Protractor
_resource: <http://www.protractortest.org/#/>_

end to end testing for AngularJS

##Main Benefits
* built on top of WebDriverJS (better user-like simulation)
* Supports Angular-specific locator strategies
* Can automatically wait until pending tasks are finished to execute next step

##Setup
use npm to install Protractor globally with: 

```
npm install -g protractor
```

This will install two command line tools, `protractor` and `webdriver-manager`. Try running `protractor --version` to make sure it's working. 

The `webdriver-manager` is a helper tool to easily get an instance of a Selenium Server running. Use it to download the necessary binaries with:

```
webdriver-manager update
```

Now start up the server with:

```
webdriver-manager start
```

This will start up a Selenium Server and will output a bunch of info logs. Your Protractor test will send requests to this server to control a local browser.

The server is located at `http://localhost:4444/wd/hub`

##Writing a test

Protractor needs two files to run, a *spec file* and a *configuration file*. 

This example test navigates to an example AngularJS application and checks it's title:

```js
// spec.js
// this is an example for a hypothetical calculator app
describe('Protractor Demo App', function() {
	it('should have a title', function() {
		browser.get('http://juliemr.github.io/protractor-demo')

		expect(browser.getTitle()).toEqual('Super Calculator');
	});
});
```

NOTE: `the describe` and `it` syntax above is from the Jasmine framework. `browser` is a global created by Protractor, which is used for browser-level commands such as navigation with `browser.get`.

Now create the configuration file:

```js
// conf.js
exports.config = {
	framework: 'jasmine',
	seleniumAddress: 'http://localhost:4444/wd/hub',
	specs: ['spec.js']
}
```

The configuration tells Protractor where your test files (`specs`) are, and where to talk to your Selenium Server (`seleniumAddress`). It specifies that we will be using Jasmine for the test framework. It will use the defaults for all other configuration. Chrome is the default browser. 

Now run the test with

```
protractor conf.js
```

##Interacting With Elements

```js
// spec.js
// this is an example for a hypothetical calculator app
describe('Protractor Demo App', function() {
	it('should add one and two', function() {
		browser.get('http://juliemr.github.io/protractor-demo/');
		element(by.model('first')).sendKeys(1);
		element(by.model('second')).sendKeys(2);

		element(by.id('gobutton')).click();

		expect(element(by.binding('latest')).getText())
			.toEqual('3');
	})
})
```

This uses the globals `element` and `by`, which are also created by Protractor. The `element` function is used for finding HTML elements on the webpage. It returns an ElementFinder object, which can be used to interact with the element or get information from it. 

## Writing multiple scenarios

```js
// spec.js
describe('Protractor Demo App', function() {
  var firstNumber = element(by.model('first'));
  var secondNumber = element(by.model('second'));
  var goButton = element(by.id('gobutton'));
  var latestResult = element(by.binding('latest'));

	// beforeEach is called here to pull the navigation 
	// out of each 'it' statement

  beforeEach(function() {
    browser.get('http://juliemr.github.io/protractor-demo/');
  });

  it('should have a title', function() {
    expect(browser.getTitle()).toEqual('Super Calculator');
  });

  it('should add one and two', function() {
    firstNumber.sendKeys(1);
    secondNumber.sendKeys(2);

    goButton.click();

    expect(latestResult.getText()).toEqual('3');
  });

  it('should add four and six', function() {
    // Fill this in.
    expect(latestResult.getText()).toEqual('10');
  });
});
```

## Changing the Configuration

capabilities determine how many environments are tested and which environments those are.

```js
// conf.js
exports.config = {
  framework: 'jasmine',
  seleniumAddress: 'http://localhost:4444/wd/hub',
  specs: ['spec.js'],
  multiCapabilities: [{
    browserName: 'firefox'
  }, {
    browserName: 'chrome'
  }]
}
```

## Lists of Elements

use `element.all` to deal with a list of multiple elements. This will return an ElementArrayFinder:

```js
// this code inside a describe function callback
var history = element.all(by.repeater('foo in foobar'));

// can also call last, first, etc.
expect(history.count()).toEqual(3);
```



## Locators

`element` takes one parameter, a *Locator*, which describes how to find the element. The `by` object creates Locators. The above example uses three types of locators:

* `by.model('first')` to find the element with `ng-model="first"`.
* `by.id('gobutton')` to find the element with the given id. This finds `<button id="gobutton">`.
* `by.binding('latest')`to find the element bound to the variable `latest`. This finds the span containing `{{latest}}
* `exactBinding`
* `by.css('.myclass')` to find an element using a css selector.
* `by.linkText('linkToSite')` locates link elements whose visible text matches a given string.
* `by.buttonText('Click me')` to find a button by text
* `by.partialButtonText('Click')` to find a button by partial text
* `by.repeater('cat in pets').row(n)` to find the nth indexed repitition of the specified ng-repeat
* `exactRepeater`
* `addLocator`
* `cssContainingText`
* `options`
* `deepCss`
* `className`
* `linkText`
* `js`
* `name`
* `partialLinkText`
* `tagName`
* `xpath`

NOTE: When using a css selector as a locator, you can use the shortcut $() notation:

```js
$('.myclass');

// these two are the same 

element(by.css('.myclass')); 
```


## Actions

The `element()` function returns an ElementFinder object. The ElementFinder knows how to locate the DOM element using the locator you passed in as a parameter, but it has not actually done so yet. It will not contact the browser until an action method has been called.

The most common action methods are: 

```js
var el = element(locator);

// Click on an element
el.click();

// Send keys to the element (usually an input)
el.sendKeys('my text');

// Clear the text in an element (usually an input).
el.clear();

// Get the value of an attribute, for example, get the value of an input
el.getAttribute('value');
```

NOTE: the full list of actions is available [here](http://www.protractortest.org/#/api?view=webdriver.WebElement).

Since all actions are asynchronous, all action methods return a promise. So, to log the text of an element, you would do something like:

```js
var el = element(locator);
el.getText().then(function(text) {
	console.log(text);
});
```

