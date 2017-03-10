#Strict Mode
_reference: <https://www.w3schools.com/js/js_strict.asp>_

Strict mode is declared by adding `"use strict";` to the beginning of a script or a function.
Declared at the beginning of a script, it has global scope (all code in the script will execute in strict mode).

##What does it do?
Strict mode prevents you from using undeclared variables and will cause the compiler to throw an error if it sees one.

##Why?
Strict mode makes it easier to write "secure" Javascript.
It changes previously accepted "bad syntax" into real errors.

#Source Map
_reference: <http://blog.teamtreehouse.com/introduction-source-maps>_

##What is it?
A source map provides a way of mapping code in a compressed file back to it's original position in a source file. 
This means that you can easily debug your application even after your assets have been optimized.

##Why?
Using source maps allow developers to maintain a straightforward debugging environment while at the same time optimiing their site for performance. 

#JSONP
_reference: <https://en.wikipedia.org/wiki/JSONP>_

##What is it? 
Stands for JSON with padding.

##Why is it needed? 
the html `<script>` element is allowed to execute content retrieved from foreign origins. Services relying on pure JSON were not able to share data across domain before the adoption of CORS. For example, a request to a foriegn object may return a response as a JSON object, but attempts to use this data across origins will result in a syntax error.

```html
<script
	src="http://serer.com/request/foo/1">

</script>
```

The reson for this is that the browser will download the script and will interpret the response object as a code block, essentially rendering this: 

```html
<script>
	{
		prop1: "val",
		prop2: "val2",
		prop3: "val3"
	}
</script>
```

Even if this block is interpreted as a javascript object literal, there is no variable declaration, so it can't be used by Javascript running in the browser. This will throw a syntax error. 

##How does JSONP work?
JSONP works by returning the JSON data wrapped in a piece of Javascript code, usually a function call. The "wrapped paylaod" can then be interpreted by the browser. This works because a function that has already been defined inside of the browser's js can be used to access the payload of the JSONP request. * Note it is required for the server and client to agree on the name of this function, usually this is done by including a `jsonp` or `callback` query string parameter like this: 

```html
<script type="application/javascript"
        src="http://server.example.com/Users/1234?callback=parseResponse">
</script>
```

##Summary
JSONP allows browsers to work around the same-origin-policy via script element injection. this means injecting a script into the DOM via jQuery or other library that has a `src` attribute pointing to a cross origin site that returns a 'padded' response as a js block.
