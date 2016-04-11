## Bootstrap 4 alpha

- `npm install --save-dev git+https://git@github.com/twbs/bootstrap.git#v4-dev`
- `npm install --save-dev gulp gulp-sass`
- create `gulpfile.js`:

```javascript
var gulp = require('gulp')
var sass = require('gulp-sass')
gulp.task('sass', function () {
  gulp.src('./node_modules/bootstrap/scss/bootstrap.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./client/css'))
})
```

- add to `package.json`:

```json
"scripts": {
  "sass": "gulp sass"
}
```

- add to `.gitignore`:

`client/css`

- add to `client/index.html`:

```html
<head>
  <!-- Required meta tags always come first -->
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="x-ua-compatible" content="ie=edge">
  <link href="css/bootstrap.css" rel="stylesheet" />
...
```
