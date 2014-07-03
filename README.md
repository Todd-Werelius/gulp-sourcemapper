gulp-sourcemapper
=================
A gulp sourceMap pre and post processor plugin

This is based off of [gulp-sourcemaps] and to a lesser extent [gulp-concat-sourcemap] both of which had issues and in my opinion did not work correctly. See foot notes if your interested in the details. 

###What does it do 
The pre-processor portion attaches a sourceMap to the file that is being processed, the post-processor portion finalizes the sourceMap and turns it into a stream for that can be saved to disk or further processed. 
```javascript
var gulp       = require('gulp');
var concat     = require('gulp-concat-sourcemapper');
var uglify     = require('gulp-uglify');
var sourcemaps = require('gulp-sourcemapper');

gulp.task('buildJS', function() {
  gulp.src('src/**/*.js')
    .pipe(sourcemaps.attach())
      .pipe(concat('app.js'))
      .pipe(uglify())
    .pipe(sourcemaps.addStream())
    .pipe(gulp.dest('dist'));
});
```

[gulp-sourcemaps]:https://github.com/floridoo/gulp-sourcemaps
[gulp-concat-sourcemap]:https://www.npmjs.org/package/gulp-concat-sourcemap
