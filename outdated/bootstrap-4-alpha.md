## Bootstrap 4 alpha

### Get from CDN

With cURL into cwd

- `curl -O https://cdn.rawgit.com/twbs/bootstrap/v4-dev/dist/css/bootstrap.css`

### Install with NPM

- `npm install --save-dev git+https://git@github.com/twbs/bootstrap.git#v4-dev`

- add to `client/index.html`:

```html
<head>
  <!-- Required meta tags always come first -->
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta http-equiv="x-ua-compatible" content="ie=edge">
  <link href="../node_modules/bootstrap/dist/css/bootstrap.min.css" rel="stylesheet" />
...
```

### Build SCSS with Gulp

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

- modify `client/index.html`:

```html
<link href="css/bootstrap.css" rel="stylesheet">
```

- add to `.gitignore`:

`client/css`
