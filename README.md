gulp-sourcemapper
=================
A gulp sourceMap pre and post processor plugin

This is based off of [gulp-sourcemaps] and to a lesser extent [gulp-concat-sourcemap] both of which had issues and in my opinion did not work correctly. See foot notes if your interested in the details. 
###Install it 
```
npm install gulp-sourcemapper --save-dev
```
###What it does
```sourcemapper.attach([options])``` attaches a sourceMap stub to any sourcfile that is processed processed. 
```sourcemapper.addMapfile()``` attaches the finalized sourceMap to the gulp pipeline or optionally appends it directly to the end of the sourcefile. 
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
The plugins inbetween the sourcemapper.attach() sourcemapper.addMapFile() should be sourceMap aware to take advantage the attached sourceMapped property. 

The only plugins I have tested it with are gulp-concat-sourcmapper, gulp-uglify, and gulp-uglifyjs. 
###attach([options]) 
A number of options are avialble for the attach() pre-processor, the defaults are listed below
```
options = {
  sourceMapLink : [true]  | false,
  sourceEmbed   : [false] | true,
  sourceRoot    : ""      | A URL prefix
  relativeIdx   : 0       | URL path segments to remove 
}
```
#####sourceMapLink
If set to true this will create an external sourceMap with the name of the file that is output via sourcemapper.addMapFile() with a ```.map``` appended to the name, in the exampe this woudl resolve to ```app.js.map``` 
`
#####gulp-concat-sourcemapper 
A modied version of floridoos gulp-concat variant that is NPM hosted and supports sourceMap's as well as addressing issues I had with his version of the plugin.

#####gulp-uglify & gulp-uglifyjs
gulp-uglify already contains support for sourceMaps and has been tested and works well. 

[gulp-sourcemaps]:https://github.com/floridoo/gulp-sourcemaps
[gulp-concat-sourcemap]:https://www.npmjs.org/package/gulp-concat-sourcemap
