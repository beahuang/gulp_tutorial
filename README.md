An Introduction to Gulp.js
==========================
1. Why Gulp?
2. Basic Linux Refresher
3. Getting Started
4. Installation
5. Project Setup
6. Installing a Plugin
7. Creating a Task
8. Watch Task
9. Compiling with Gulp
10. Local Server and Live Reloading
11. Image Optimization

Why Gulp?
---------
Gulp is a Node based JavaScript task runner integral to speed up development times for web developers everywhere. Tasks that would take five minutes every hour can be automated with a script and take seconds in the background while you do other things. The configuration of the files are easy to read and your builds run faster than the other leading JavaScript task runner around.

Basic Linux Refresher
---------------------
Using Gulp.js you need to know how your way around the terminal. Gulp is run in the command line so all the commands that I'm running should be run there. Since we are assuming that the reader of this tutorial is already a full-time developer, I will only give a short commands refresher.

`cd "/dir"` : Changes the current working directory to the 'dir' directory specified

`ls` : Lists all the files in the current working directory  

`mv filepath filepath` : Moves the first file to the second file path

`mkdir filepath` : Makes a directory at the filepath specified  

`rm filepath` : removes file is at the specified filepath

**File matching with symbols:**  

`*` : Wildcard that matches any pattern in the directory. Ex. `*.js`
matches all js files in the directory.  

`**/*`: Similar to the * pattern that matches any file in the root folder and any child directories.  

`!`: Excludes the pattern from its matches, which is useful if you had to exclude a file from a matched pattern.  

`*.+(.js | .jsx)`: The plus + and parentheses () allows Gulp to match multiple patterns, with different patterns separated by the pipe | character.

Getting Started
---------------
Gulp is written off Node so in order to install Gulp, you first need to install Node. Node.js can be downloaded for Windows, Mac and Linux at nodejs.org/download/. Once installed, open your command line tools and write the following command:

`node - v`

If that works, then type in

`npm - v`

If both work correctly, then you have installed both `node` and its package manager `npm` that comes with it. If not, then you did not install correctly and should retry the above steps again.

Installation
---------------
To install Gulp.js we have to use node's package manager `npm`. To do this we type the following command:

`sudo npm install -g gulp`

`sudo` gives your command root privileges which is necessary to install most scripts
`npm install` is telling the package manager that you want to install something
`-g` is an optional flag that you give so that you install gulp in your whole system rather than just the directory you're currently in. Important if you want to use gulp in multiple directories for different projects.

Project Set Up:
-----------------
We will now create a directory called test and navigate to it.
The basic file structure of this tutorial's project will be as follows:
```
+-- gulpfile.js
+-- node_modules/
+-- package.json
+-- src
|   +-- scripts/
|   +-- styles/
|   +-- images/
+-- dist/
|   +-- scripts/
|   +-- styles/
|   +-- images/
+-- index.html
```

Your file structure can be different but make sure all your file paths are correct while writing tasks.  

Every task that we're going to write is going to be written inside of gulpfile.js. Gulp doesn't do anything by itself so in order to start writing tasks we need to install some plugins.

Installing a Plugin
----------------
You can either install every plugin manually or you can install them all at once by writing a package.json

To install the `jshint` plugin manually run:
`npm install gulp-jshint`

The npm init command creates a package.json file for your project which stores information about the project, like the dependencies used in the project (Gulp is an example of a dependency).

`npm init` : will automatically create a package.json for you.


Using a package.json:
```
{
  "name": "Project Name",
  "version": "1.0.0",
  "description": "Example Package.json",
  "main": "",
  "scripts": {
    "test": "echo \"Error no test specified" && exit 1"
  },
  "author": "Your Name",
  "license": "ISC",
  "devDependencies": {
    "gulp": "^3.90",
    "gulp-concat": "^2.3.3",
    "gulp-jshint": "^1.7.1",
    "gulp-rename": "^1.2.0",
    "gulp-sass": "^0.7.2",
    "gulp-uglify": "^0.3.1"
  }
}
```
In order to install from a package.json, write the plugin name in the devDependencies section and what version you want.
If you don't know what version you need, use the `^1.2.3` to signify you want the most recent major update after the version given.
Feel free to update the name, description and author with your information if you want.

After writing the package.json type:

`sudo npm install`

All the plugins you have indicated in devDependencies will show up in your `node_modules` directory.

Creating a Task
--------------
The first task that I will show you is the jshint task. This task will detect any errors and potential problems in your code and output in to the command line.

Below is the code in your gulp.js file that I will walk through:
```
// include gulp
var gulp = require('gulp');

// include jshint plugin
var jshint = require('gulp-jshint');

//JS hint task
gulp.task('jshint', function(){
  gulp.src('src/scripts/*.js')
    .pipe(jshint())
    .pipe(jshint.reporter('default'));
})
```

In order to use any of the plugins you need to write the following statement:

`var plugin name = require("plugin name")`

`gulp.task('someName')`: gives the task a name so when you want to run a task write `gulp someName` in the command line.

`gulp.src(path)`: specifies what files you want to run jshint on. In the above case it is all the files `*` with the extension .js

`.pipe`: is how Gulp.js flows the data. In this situation you are taking in all the JavaScript files and running the jshint function on all of them and then piping them to the default jshint reporter.

Now to run the above task write the following in the terminal:
`gulp jshint`

Hooray! You've written your first build task!

Watch Task
----------------
Now that you've seen one task, I am going to show you one of the cooler things that Gulp.js called the watch task:   

```
gulp.task('watch', function() {
  gulp.watch('src/scripts/*.js', ['jshint']);
});
```
Watch is a built-in gulp task that allows you to see if any of the files in the path directory given has any files that were changed. If any of the files were changed, run the `['jshint']` task from above.  

This is cool because that means you can run all the tasks that you want right after you save a document.

Compiling using Gulp
---------------------
Another thing we can do in Gulp.js is compiling preprocessors. For this example we're going to compile SASS into CSS:  

```
var sass = require('gulp-sass');

gulp.task('sass', function() {
  return gulp.src('src/styles/**/*.scss')
    .pipe(sass())
    .pipe(gulp.dest('dist/styles'))
});
```
Here, we are taking all the `.scss` files from styles and running the `sass()` function on to them and then piping the result of that, a css file, into a destination folder `dist/styles`.  

You can build off this by adding a line to the watch task that watches the styles source folder and runs the sass task on it.  

```
gulp.task('watch', function() {
  gulp.watch('src/scripts/*.js', ['jshint']);
  gulp.watch('app/scss/**/*.scss', ['sass']);
});
```

Local Server and Live Reloading
------------------
Another cool thing that can be done with Gulp.js is creating a local server to allow for live reloading. When you save a file, you can automatically see the changes in your browser without refreshing. This task will use the browser-sync plugin:  

```
var browserSync = require('browser-sync');

gulp.task('browser-sync', function() {
  browserSync({
    server: {
       baseDir: "./"
    }
  });
});
```
Since we're running a server, we need to let Browser Sync know where the root of the server should be. In our case, it's the `./` folder which is the present directory.

```
gulp.task('sass', function() {
  return gulp.src('src/styles/**/*.scss')
    .pipe(sass())
    .pipe(gulp.dest('dist/styles'))
    .pipe(browserSync.reload({
      stream: true
    }))
});
```
Here we want to run the browser-sync task before the watch task so that when we run gulp watch it'll start the server first and then watch for changes. We do this by putting `,['browser-sync'],` between the watch and the function() as seen below:  

```
gulp.task('watch', ['browser-sync'], function() {
  gulp.watch('src/scripts/*.js', ['jshint']);
  gulp.watch('src/styles/*.scss', ['sass']);
});
```

Now we can add a task that reloads the browser when any html file or JavaScript file is saved by adding an extra line to the watch for html and modifying the watch line for Javascript.

```
gulp.task('watch', ['browser-sync'], function() {
  gulp.watch('src/scripts/*.js', ['jshint', browserSync.reload]);
  gulp.watch('src/styles/*.scss', ['sass']);
  gulp.watch('*.html', browserSync.reload);
});
```


Image Optimization
------------------
This task will allow you to optimize your image size and cache the images that you've used and then move them into your destination folder. In order to do this we need to require both `gulp-imagemin` and `gulp-cache`. We need gulp cache because minifying images is a slow process so the more information we cache the faster the process.  

The code for this task is here:  

```
var imagemin = require('gulp-imagemin'),
    cache = require('gulp-cache');

gulp.task('images', function(){
  gulp.src('src/images/**/*')
    .pipe(cache(imagemin({ optimizationLevel: 3, progressive: true, interlaced: true })))
    .pipe(gulp.dest('dist/images/'));
});
```
`optimizationLevel`: The optimization levels 2 and higher (highest 7) enable multiple IDAT compression trials; the higher the level, the more trials.  

These options are for showing the user visual feedback as soon as possible when rendering:  

`progressive: true`: Lossless conversion to progressive which when rendered will render the low quality version -> high quality version.  

`interlaced`: Interlace gif for progressive rendering which when rendering a gif will display lines at the top of the image, then in the middle, then at the end, and will continue to fill in the blanks until the image is completely loaded.  

We can add this to our watch file so that we run optimize our images when we start the watch task.  

```
gulp.task('watch', ['browser-sync','images'], function() {
  gulp.watch('src/scripts/*.js', ['jshint']);
  gulp.watch('src/styles/*.scss', ['sass']);
  gulp.watch('*.html', browserSync.reload);
});
```

JavaScript Optimization
------------------
Now, lets optimize our JavaScript for production because no one wants to ship their code without first minifying and uglifying so that people can't copy what you've written.

Luckily we can do that with a task!

```
var concat = require('gulp-concat');
var uglify = require('gulp-uglify');

gulp.task('scripts', function(){
  return gulp.src('src/scripts/*.js')
    .pipe(concat('main.js'))
    .pipe(rename({suffix: '.min'}))
    .pipe(uglify())
    .pipe(gulp.dest('dist/scripts/'))
    .pipe(browserSync.reload({stream:true}))
});
```

`.pipe(concat('main.js'))`: This line concats all the src files into one file called `main.js`  
`.pipe(rename({suffix: '.min'}))`: This line renames main.js and adds .min to it so that it comes out as main.min.js  
`.pipe(uglify())`: Compresses and minifies the JavaScript code  

We can even add the scripts task to our watch task here:  
```
gulp.task('watch', ['browser-sync','images'], function() {
  gulp.watch('src/scripts/*.js', ['jshint', 'scripts']);
  gulp.watch('src/styles/*.scss', ['sass']);
  gulp.watch('*.html', browserSync.reload);
});
```


Grouping Tasks
--------------
Now, we've created a whole bunch of tasks that we can run but how do we run them all at once?

We've already done that in the watch task here:

```
gulp.task('watch', ['browser-sync','images'], function() {
  gulp.watch('src/scripts/*.js', ['jshint', 'scripts']);
  gulp.watch('src/styles/*.scss', ['sass']);
  gulp.watch('*.html', browserSync.reload);
});
```

However, we can change the name of the task to default and be able to run the task by just typing `gulp` instead of `gulp watch`

```
gulp.task('default', ['browser-sync','images'], function() {
  gulp.watch('src/scripts/*.js', ['jshint', 'scripts']);
  gulp.watch('src/styles/*.scss', ['sass']);
  gulp.watch('*.html', browserSync.reload);
});
```

Now all the processes we just wrote can be run with the single command `gulp`

Recap
--------
With this tutorial we've been able to:  
  1. Spin up a local web server
  2. Compile preprocessors
  3. Live reload all our changes  
  4. Optimize our images
  5. Optimize our JavaScript
  6. Run all our tasks with one Gulp command

Gulp has a whole slew of plugins to look through so with just a little more digging around you can automate even more processes you use daily!

References
----------
1. “Gulp.js: Getting Started.” Accessed February 8, 2016. https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md
