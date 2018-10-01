#Gulpjs Notes
_reference: <http://gulpjs.com>_

##What is Gulp
gulp is a toolkit for automating painful or time-consuming tasks in you development workflow, so you can stop messing around and build something. 

##Installation

####Install the `gulp` command

`npm install --global gulp-cli`


####Install `gulp` in your devDependencies

Run this command in your project directory:
`npm install --save-dav gulp`


####Create a `gulpfile

Create a file called `gulpfile.js` in your project root with these contents:

```js
var gulp = require('gulp');

gulp.task('default', function() {
	//place the code for your default tasks here
});
```


####Test is out

Run the gulp command in your project directory:

```
gulp
```

to run multiple tasks, you can use `gulp <task> <othertask>`


#Gulp Plugins

##gulp-sass
Sass plugin for Gulp.

####Installation
```
npm install gulp-sass --save-dev
```

####Basic Usage

Something like this will compile your Sass files: 

```js
'use strict';

var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function() {
	return gulp.src('./sass/**/*.scss')
		.pipe(sass().on('error', sass.logError))
		.pipe(gulp.dest('./css'));
});

gulp.task('sass:watch', function() {
	gulp.watch('./sass/**/*.scss', ['sass']);
});
```

You can also compile synchronously, doing something like this:

```js
gulp.task('sass', function() {
	return gulp.src('./sass/**/*.scss')
		.pipe(sass.sync().on('error', sass.logError))
		.pipe(gulp.dest('./css'));
});
```

####Options

pass in an options object as the parameter to the `sass` function call. 

```js
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
   .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
   .pipe(gulp.dest('./css'));
});
```

####Source Maps

`gulp-sass` can be used with `gulp-sourcemaps` to generate source maps for the Sass to CSS compilation. you do need to initialize `gulp-sourcemaps` prior to running `gulp-sass` and write the source maps after running `gulp-sass`.

```js
var sourcemaps = require('gulp-sourcemaps');
 
gulp.task('sass', function () {
 return gulp.src('./sass/**/*.scss')
  .pipe(sourcemaps.init()) 						// Initialize gulp-sourcemaps here
  .pipe(sass().on('error', sass.logError))
  .pipe(sourcemaps.write()) 					// Write sourcemaps here 
  .pipe(gulp.dest('./css'));  					// See the reference if you want to write source map to diff file.
});
```


