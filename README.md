gulp-sourcemapper
=================
A gulp sourceMap pre and post processor plugin

This is based off of [gulp-sourcemaps] and to a lesser extent [gulp-concat-sourcemap] both of which had issues and in my opinion did not work correctly. See foot notes if your interested in the details. 

###Install it 
```
npm install gulp-sourcemapper --save-dev
```
###What it does
The pre-processor portion attaches a sourceMap to the file that is being processed, the post-processor portion finalizes the sourceMap and turns it into a stream for that can be saved to disk or further processed. 
```javascript
var gulp       = require('gulp');
var concat     = require('gulp-concat-sourcemapper');
var uglify     = require('gulp-uglify');
var sourcemaps = require('gulp-sourcemapper');

gulp.task('buildJS', function() {
  gulp.src('dev/js/*.js')
    .pipe(sourcemaps.attach())          // Attach sourceNap all files that pass throughm
      .pipe(concat('app.js'))
      .pipe(uglify())
    .pipe(sourcemaps.addMapFile())      // Create a new gulp file and add it into the pipeline 
    .pipe(gulp.dest('production/js'));
});
```

The plugins inbetween the sourcemapper.attach() sourcemapper.addMapFile() should be sourceMap aware to take advantage the attached sourceMapped property. 

The only two plugins I have tested it with are gulp-concat-sourcmapper and gulp-uglify, others may or may not work. 

#####gulp-concat-sourcemapper 
Currenltly gulp-concat does not support sourceMap's and gulp-concat-sourcemap does not support uglify and has other issues as well. The version of gulp-concat provided by floridoo does have sourceMap support but was not avialble via the NPM at the time of this post and also did not work the way I needed it to ( see notes ) 

My solution was to modify the version of floridoos gulp-concat in order to provide an NPM version that supported sourceMap's and solved the issues I felt were present in that plugin. 

#####gulp-uglify
gulp-uglify already contains support for sourceMaps and has been tested and works well. 

###attach([options) 
A number of options are avialble for the attach() pre-processor, the defaults are listed below

```
options = {

  sourceMapLink : [true]  | false,
  sourceEmbed   : [false] | true,
  sourceRoot    : ""      | A URL prefix
  relativeIdx   : 0       | URL path segments to remove 

}
```


[gulp-sourcemaps]:https://github.com/floridoo/gulp-sourcemaps
[gulp-concat-sourcemap]:https://www.npmjs.org/package/gulp-concat-sourcemap
