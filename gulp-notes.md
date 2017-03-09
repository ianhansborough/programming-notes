#Gulpjs Notes
_reference: <http://gulpjs.com>_

##What is Gulp
gulp is a toolkit for automating painful or time-consuming tasks in you development workflow, so you can stop messing around and build something. 

##Installation

#####Install the `gulp` command

`npm install --global gulp-cli`


#####Install `gulp` in your devDependencies

Run this command in your project directory:
`npm install --save-dav gulp`


#####Create a `gulpfile

Create a file called `gulpfile.js` in your project root with these contents:

```js
var gulp = require('gulp');

gulp.task('default', function() {
	//place the code for your default tasks here
});
```


#####Test is out

Run the gulp command in your project directory:

```
gulp
```

to run multiple tasks, you can use `gulp <task> <othertask>`


#Gulp Plugins

##gulp-sass
Sass plugin for Gulp.

#####Installation
```
npm install gulp-sass --save-dev
```

#####Basic Usage

Something like this will compile your Sass files: 

```js
'use strict';
