# [gulp](https://github.com/wearefractal/gulp)-rev [![Build Status](https://travis-ci.org/sindresorhus/gulp-rev.svg?branch=master)](https://travis-ci.org/sindresorhus/gulp-rev)

> Static asset revisioning by appending content hash to filenames
`unicorn.css` → `unicorn-098f6bcd.css`

Make sure to set the files to [never expire](http://developer.yahoo.com/performance/rules.html#expires) for this to have an effect.


## Install

```sh
$ npm install --save-dev gulp-rev
```


## Usage

```js
var gulp = require('gulp');
var rev = require('gulp-rev');

gulp.task('default', function () {
	return gulp.src('src/*.css')
		.pipe(rev())
		.pipe(gulp.dest('dist'));
});
```

*Options are intentionally missing as the default should work in most cases.*


### Original path

Original file paths are stored at `file.revOrigPath`. This could come in handy for things like rewriting references to the assets.


### Asset hash

The hash of each rev'd file is stored at `file.revHash`. You can use this for customizing the file renaming, or for building different manifest formats.


### Asset manifest

```js
var gulp = require('gulp');
var rev = require('gulp-rev');

gulp.task('default', function () {
	// by default, gulp would pick `assets/css` as the base,
	// so we need to set it explicitly:
	return gulp.src(['assets/css/*.css', 'assets/js/*.js'], {base: 'assets'})
		.pipe(gulp.dest('build/assets'))  // copy original assets to build dir
		.pipe(rev())
		.pipe(gulp.dest('build/assets'))  // write rev'd assets to build dir
		.pipe(rev.manifest())
		.pipe(gulp.dest('build/assets')); // write manifest to build dir
});
```

An asset manifest, mapping the original paths to the revisioned paths, will be written to `dist/rev-manifest.json`:

```json
{
	"css/unicorn.css": "css/unicorn-098f6bcd.css"
}
```

### Ignoring files
You can pass `ignore` array to exclude particular files:

```js
var gulp = require('gulp');
var rev = require('gulp-rev');

gulp.task('default', function () {
	return gulp.src(['build/assets/*.*'])
		.pipe(rev({
		    ignore: [/\.(png|jpe?g|gif)$/, '.html']
		}))
		.pipe(gulp.dest('build/assets'));  // write rev'd assets to build dir
});
```
Ignore can contain regular expressions or strings (as '.html') which will be converted to regular expressions.

### Integration

For more info on how to integrate **gulp-rev** into your app, have a look at the [integration guide](integration.md).


### Works with gulp-rev

- [gulp-rev-replace](https://github.com/jamesknelson/gulp-rev-replace)
- [gulp-rev-css-url](https://github.com/galkinrost/gulp-rev-css-url)


## License

[MIT](http://opensource.org/licenses/MIT) © [Sindre Sorhus](http://sindresorhus.com)
