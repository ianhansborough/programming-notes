# NodeJS Language Notes
_Reference: <https://www.udemy.com/understand-nodejs/learn/v4>_

# Vocabulary

# Concepts

Node is written in C++
V8 is also written in C++

## The V8 Javascript Engine
A javascript engine is a program that converts javascript code into something the computer processor can understand. It should follow the ECMAScript standard on how the language should work and what features it should have.

The V8 engine is written in C++, it can run as a standalone, but it can also be embedded into other C++ programs. 
Essentially, what V8 is doing is taking in javascript and compiling it down to machine code.
By embedding V8 into a C++ application, you can ADD features to Javascript by writing additional C++ hooks that trigger when certain phrases are typed in the js code. 

Adding features like this is extremly powerful and it is what has allowed NodeJS to become a reality. This is the reason you can now code full stack in JS!  

# Modules
A reusable block of code whose existance does not accidentally impact other code
NodeJS was the first to impliment reusable code modules.

## CommonJS Modules 
An agreed upon standard for how code modules should be structured. 

# Building a Module

You can only access functions and varibles from a required module that you choose to expose from within the module via the `module.exports` property.

```js
//greet.js
var greet = function() {
	console.log('hello');
}

module.exports = greet;
```

```js
// app.js
var greet = require('./greet.js'); // filename can be ./greet or ./greet.js

greet();

```


The require function will look for a filename with a .js extension, but if it can't find it it will look for a falder with the given name and a file called `index.js` within that folder. 

#Module Patterns

```js
// greet1.js
module.exports = function() {
	console.log('greet');
}
// greet2.js
module.exports.greet = console.log('greet');

//greet3.js 
module.exports = function() {
	var greet = console.log('greet3');
	return {
		greet: greet
	};
}

var greet = require('./greet1');
var greet2 = require('./greet2').greet;
var greet3 = require('./greet3').greet;

```


# Native Modules

The NodeJS API has a bunch of native modules. some, such as `console` are global. Others must be required within the context of your files. See below...

```js
var util = require('util');
var name = 'Ian';
var greeting = util.format('Hello, %s', name);
util.log(greeting);
```

#Events and the event emmiter

There are two different kinds of events in NodeJS

* system events - come from the C++ core `libuv`;
* custom events = come from the JS core, events that you can create for yourself, from `Event Emitter`;

## Building an event emitter

```js
// app.js
var Emitter = require('./emitter');

var emtr = new Emitter();

emtr.on('greet', function() {
	console.log('greet event has been emitted!');
});

// must be called manually, it does not actually "listen"
emtr.emit('greet');

```

```js
// emitter.js

function Emitter() {
	this.events = {};

	Emitter.prototype.on = function(type, listener) {
		//the basic process to fake listening here
		// it to create an object, with a property for each event being listened to
		// each property is an array of functions
		// i.e what happens when an event is triggered

		this.events[type] = this.events[type] || [];
		this.events[type].push(listener);
	}

	Emitter.prototype.emit = function(type) {
		// this searches your events object for the event that was emitted
		// if it finds it, this function will
		// loop over all functions in it's array
		// and execute all of them
		if (this.events[type]) {
			this.events[type].forEach(function(listener) {
				listener();
			})
		}
	}
}
module.exports = Emitter;

```

# Object.create

```js 
var person = {
	firstname: '', 
	lastname: '',
	greet: function() {
		return this.firstname + this.lastname;
	}
}

// creates a new object that inherits from the person object's properties
var john = Object.create(person);


```

# JS Aside: .call and .apply 

```js
var obj = {
	name: 'John Doe',
	greet: function() {
		console.log(` hello ${ this.name }`);
	}

obj.greet();
// using .call overrides the 'this' variable with whatever you pass in
obj.greet.call({ name: 'Jane Doe' }, otherParam1, otherParam2);
// using .apply overrides 'this' and takes other params as an array instead of comma seperated
obj.greet.apply({ name: 'Jane Doe' }, [otherParam1, otherParam2]);
}
```

# JS Aside: ES6 classes 

```js
'use strict';
// extends here allows for prototype inheritance from Human, see constructor
class Person extends Human {
	// constructor properties are the equivelent of properties on the constructor function
	constructor(firstname, lastname) {
		// inherit prototype chain from Human
		super()
		this.firstname = firstname;
		this.lastname = lastname;
	}
	// class methods get placed on the constructor function prototype
	greet() {
		console.log('Hello ' + this.firstname);
	}
}
var john = new Person('john', 'doe');
```

# Inheriting from the Event Emitter

```js 
var EventEmitter = require('events');
var util = require('util');

function Greetr() {
	// this makes sure that event emitter properties are placed on Greetr's 'this' object
	EventEmitter.call(this);
	// placing additional properties on the same 'this' object
	this.greeting = 'Hello World';
}
// this says Greetr inherits everything on EventEmitter's prototype chain
util.inherit(Greetr, EventEmitter);

Greetr.prototype.greet = function(data) {
	console.log(this.greeting + ': ' + data);
	this.emit('greet', data);
}

var greetr1 = new Greetr();

greetr1.on('greet', function(data) {
	console.log('Someone Greeted!: ' + data);
});

greetr1.greet('Tony');

```

# Libluv
this is another c++ library that was built primarily for Node. It's purpose is to handle I/O and thus events between NodeJS and the operating system of the server. It has it's own event loop that is constantly checking for incoming events from the OS. when it detects one of these events, it fires a designated callback which then gets inserted at the top of the V8 queue to be executed next. Keep in mind that the V8 engine is still synchronous, Libluv gives it the ability to handle events without blocking up the queue.

# Buffers in Node

```js 
// notice that Buffer is global
var buf = new Buffer('Hello', 'utf8');
console.log(buf);
// outputs '48 65 6c 6c 6f' hex for each letter's UTF-8 value
console.log(buf.toString());
console.log(buf.toJSON());
```

# Files and fs

```js
var fs = require('fs');

// this will synchronously read in the file, good for config stuff but will bloc up the queue
var greet = fs.readFileSync(__dirname + '/greet.txt', 'utf8');
console.log(greet);

// this will ansyc. read in the file and call the callback when it's finished
// will return a buffer object by default, or a string if an encoding
// is specified as the second parameter
var greet2 = fs.readFile(__dirname + 'greet.txt', 'utf8', function(err, data) {
	console.log(data);
});
```

# Streams
Data is split into 'chunks' and streamed
`Stream` is an abstract class that inherits from node's event emitter.

```js
var fs = require('fs');
// second param is the options object
// highWaterMark is the size of the buffer in bytes
var readable = fs.createReadStream(__dirname + '/greet.txt', { encoding: 'utf8', highWaterMark: 16 * 1024});

var writable = fs.createWriteStream(__dirname + '/greetCopy.txt');

readable.on('data', function(chunk) {
	console.log(chunk.length);
	writable.write(chunk);
});
```

# Pipes
Connecting two streams by writing to one stream what is being read from the other.
In Node you pipe from a readable stream to a writable stream. 

Piping allows you to more efficiently transmit data between streams 

```js
var fs = require('fs');
var zlib = require('zlib');

var readable = fs.createReadStream(__dirname + '/greet.txt', { encoding: 'utf8', highWaterMark: 16 * 1024});
var writable = fs.createWriteStream(__dirname + '/greetCopy.txt');
var compressed = fs.createWriteStream(__dirname + '/greet.txt.gz');

// this is a transform stream that is both readable and writable
// it takes in data, compresses it, and spits it out the other end
// two way streams allow you to chain pipe function calls
var gzip = zlib.createGzip();

readable.pipe(gzip).pipe(compressed);
```



