gulp-sourcemapper
=================
A gulp sourceMap pre and post processor plugin

This is based off of [gulp-sourcemaps] and to a lesser extent [gulp-concat-sourcemap] neither of which met my needs. 
###Install it 
```
npm install gulp-sourcemapper --save-dev
```
###What it does
```sourcemapper.attach([options])``` attaches a sourceMap stub to any sourcfile that is piped through it.
```sourcemapper.addMapfile()``` attaches the finalized sourceMap to the gulp out pipe or optionally appends it directly to the end of the sourcefile. 
```javascript
var gulp         = require('gulp');
var concat       = require('gulp-concat-sourcemapper');
var uglify       = require('gulp-uglify');
var sourcemapper = require('gulp-sourcemapper');

gulp.task('buildJS', function() {
  gulp.src('dev/js/*.js')
    .pipe(sourcemapper.attach())        // Attach a sourceNap stub to all source files piped through 
      .pipe(concat('app.js'))           // Concat will fill in the sourceMap contents for each source file 
                                        // and then create a single sourceMap before it pipes the results out
      .pipe(uglify())                   // uglify will take the concatenated file and update the sourceMap to 
                                        // account for the minification process 
    .pipe(sourcemaps.addMapFile())      // Adds the map file to the output or appends it to the sourcefile  
    .pipe(gulp.dest('production/js'));
});
```
The plugins inbetween the ```sourcemapper.attach()``` and ```sourcemapper.addMapFile()``` should be sourceMap aware to take advantage of the attached sourceMap property attached to the file. 

The only plugins I have tested it with are ```gulp-concat-sourcmapper```, ```gulp-uglify```, and ```gulp-uglifyjs```. 
###attach([options]) 
A number of options are avialble for the attach() pre-processor, the defaults are listed below
```
options = {
  sourceMapExt  : [true]  | String | false,
  sourceMapLink : [true]  | false
  sourceAttach  : [false] | true,
  sourceRoot    : ""      | A URL prefix
  relativeIdx   : 0       | URL path segments to remove 
}
```
#####sourceMapExt
Default is true. This will create an external sourceMap using the name of the file that is piped into sourcemapper.addMapFile() but with a ```.map``` appended to the name, in the exampe this woudl resolve to ```app.js.map``` 
If set to a string this will create an external sourceMap using the value of the string to create the name of the sourceMap file. A relative path can be prefixed to the name in which case gulp.dest() will use nodejs's path.join logic to resolve the final destination. 

If set to false the sourceMap contents will be directly appended to the end of the sourceFile in base64 format.  
#####sourceMapURL
This option is only processed if sourceMapExt is true or a string, otherwise it is ignored since the sourceMap will be appended to the source file 

Default is true. Appends an annotation to the end of the sourcefile that links to the location of the sourceMap map for this file on an accessible server.  This allows browsers to download the sourcemap for in browser debugging.  

Setting this to false will skip the annotation ( not reccomended unless you have a good reason ) 
#####sourceAttach 
Default true; This will add your the unmodified source code into the sourceMap so that the server does not need to download the source code when debugging.  

This is the safest way to make sure that source code is always available for debugging without worrying about hosting it on your server, but will make the sourceMaps much bigger.

Generally if you want anyone to debug your code, from anywhere, then attaching the source code makes sense.

False will skip adding the source code into the sourceMap the browser instead will depend on the location URL's in the sourceMap of all your source files.
#####relativeIdx
Each source file that is referened in your sourceMap will have a sources entry that gives the relative path of that file. 

e.g. if you were using the following directory structure and grabbing the ```app.js``` and ```lib.js`` files when building your sourceMap the default entries would look something like) 

```[js/app.js,js/lib.js] ```

```
+public
  --js
      app.js
      lib.js
```
When a browser tried to download those files it would try and get them from http://mysite.com/js/app.  

By setting relativeIdx to something greater than one you are telling the plugin to start stripping leading directory's off your realtive path.

```realitiveIdx : 1 ```

Would modify the source links to ```[app.js,lib.js]``` and try and download them from http://mysite.com/app.js etc. 

If you wanted to strip all path information and just use the names you can set this to an arbitraily high number like 999. #####sourceRoot


###Assoicated Gulp Plugin's

#####gulp-concat-sourcemapper 
A modied version of floridoos gulp-concat variant that is NPM hosted and supports sourceMap's as well as addressing issues I had with his version of the plugin.

#####gulp-uglify & gulp-uglifyjs
gulp-uglify already contains support for sourceMaps and has been tested and works well. 

[gulp-sourcemaps]:https://github.com/floridoo/gulp-sourcemaps
[gulp-concat-sourcemap]:https://www.npmjs.org/package/gulp-concat-sourcemap
