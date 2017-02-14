## TypeScript watcher using Gulp

The command `tsc -w` has been working extremely slow for me so I decided to use gulp for the watcher instead.

- `npm install --save-dev gulp gulp-shell`
- add `gulpfile.js`

```javascript
var shell = require('gulp-shell')
gulp.task('tsc', shell.task([
  'npm run typescript'
]))

gulp.task('watch', () => {
  gulp.watch(paths.typescripts, ['tsc'])
})
```

- add to `package.json` scripts

```json
"gulp:watch": "gulp watch"
```

- run with `npm run gulp-ts:watch`
- Note: when using gulp for compilation, the TypeScript files are under `sources` in chrome dev tools
