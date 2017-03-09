#Gulpjs Notes
reference: [](http://gulpjs.com)

##What is Gulp
gulp is a toolkit for automating painful or time-consuming tasks in you development workflow, so you can stop messing around and build something. 

##Installation
_Install the `gulp` command_

`npm install --global gulp-cli`


_Install `gulp` in your devDependencies_

Run this command in your project directory:
`npm install --save-dav gulp`


_Create a `gulpfile`_

Create a file called `gulpfile.js` in your project root with these contents:

```js
var gulp = require('gulp');

gulp.task('default', function() {
	//place the code for your default tasks here
});
```


_Test is out_

Run the gulp command in your project directory:

```
gulp
```

to run multiple tasks, you can use `gulp <task> <othertask>`
