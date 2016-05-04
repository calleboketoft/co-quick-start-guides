# Angular 2 Module for Publishing

Here's a complete module following this guide: [co-selectable-items](https://github.com/calleboketoft/co-selectable-items)

## Skeleton

- mkdir `myproj`
- cd `myproj`
- `git init`
- `npm init -y`
- `npm install --save @angular/core @angular/compiler @angular/common @angular/platform-browser @angular/platform-browser-dynamic rxjs@5.0.0-beta.6 zone.js@0.6.12 reflect-metadata es6-shim systemjs`
- `npm install --save-dev concurrently typescript typings express ghooks`

- create file `.gitignore`

```bash
.DS_Store
node_modules
*.log
```

- create file `server.js`

```javascript
var port = 3000
var staticDir = './'
var express = require('express')
var app = express()
app.use(express.static(staticDir))
var server = app.listen(port, () => {
  console.log('serving at: ' + port)
})
```

- create file `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  "exclude": [
    "node_modules",
    "typings/main",
    "typings/main.d.ts"
  ]
}
```

- add scripts and ghooks config to package.json

```json
"scripts": {
  "start": "node server",
  "build": "npm run tsc",
  "prepublish": "npm run build",
  "tsc": "tsc -p .",
  "tsc:watch": "tsc -p . -w",
  "typings": "typings",
  "postinstall": "typings install"
},
"config": {
  "ghooks": {
    "pre-commit": "npm run build"
  }
},
```

- add file `typings.json`

```json
{
  "ambientDependencies": {
    "es6-shim": "registry:dt/es6-shim#0.31.2+20160317120654",
    "jasmine": "registry:dt/jasmine#2.2.0+20160412134438"
  }
}
```

## Component example

- create file `index.html`

```html
<html><head><meta http-equiv="refresh" content="0; URL='/src'" /></head></html>
```

- create folder `src`
- create file `src/systemjs.config.ts`

```javascript
declare var System
System.config({
  baseURL: '/',
  warnings: true,
  map: {
    'src': 'src',
    '@angular': '/node_modules/@angular',
    'rxjs': 'node_modules/rxjs'
  },
  packages: {
    'src': {defaultExtension: 'js'},
    'rxjs': {defaultExtension: 'js'},
    '@angular/common': {defaultExtension: 'js', main: 'index.js'},
    '@angular/compiler': {defaultExtension: 'js', main: 'index.js'},
    '@angular/core': {defaultExtension: 'js', main: 'index.js'},
    '@angular/http': {defaultExtension: 'js', main: 'index.js'},
    '@angular/platform-browser': {defaultExtension: 'js', main: 'index.js'},
    '@angular/platform-browser-dynamic': {defaultExtension: 'js', main: 'index.js'},
    '@angular/router': {defaultExtension: 'js', main: 'index.js'},
    '@angular/router-deprecated': {defaultExtension: 'js', main: 'index.js'},
    '@angular/testing': {defaultExtension: 'js', main: 'index.js'},
    '@angular/upgrade': {defaultExtension: 'js', main: 'index.js'},
  }
})
```

- create file `src/index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Angular 2 ES6 App</title>
  </head>
  <body style="margin-top: 50px;">
    <app>Loading...</app>
    <script src="../node_modules/angular2/bundles/angular2-polyfills.js"></script>
    <script src="../node_modules/systemjs/dist/system.js"></script>
    <script src="systemjs.config.js"></script>
    <script>
      System.import('./example/bootstrap')
    </script>
  </body>
</html>
```

- create folder `src/example`
- create file `src/example/bootstrap.ts`

NOTE: bootstrapping code is separated from example so that the example code
can be used as a component by itself in a separate repo.
```javascript
import {bootstrap} from '@angular/platform-browser-dynamic'
import {AppCmp} from './app-cmp'
bootstrap(AppCmp)
```

- create file `src/example/app-cmp.ts`

```javascript
import {Component} from '@angular/core'
@Component({
  selector: 'app',
  template: `
    <h1>Angular 2</h1>
  `
})
export class AppCmp {}
```

- The base of the app is ready to view!
- compile tsc `npm run build` and start server `npm start` to view in browser `http://localhost:3000`


## Component

- create component folder `src/my-component`
- create component file `src/my-component/my-component-cmp.ts`

```javascript
import {Component} from '@angular/core'
@Component({
  selector: 'my-component',
  template: `<p>My Component</p>`
})
export class MyComponentCmp {}
```

- import component to `src/example/app-cmp.ts` and enable it

```javascript
import {Component} from '@angular/core'
import {MyComponentCmp} from '../my-component/my-component-cmp'
@Component({
  directives: [MyComponentCmp],
  selector: 'app',
  template: `<my-component></my-component>`
})
export class AppCmp {}
```

## Testing - Unit tests

- Install dependencies `npm install --save-dev jasmine-core karma karma-chrome-launcher karma-jasmine karma-systemjs`
- Initialise Karma `./node_modules/.bin/karma init`
- Update `karma.config.js`:

```javascript
frameworks: ['systemjs', 'jasmine'],
files:['src/test/unit/*.spec.js'],
systemjs: {
  configFile: 'src/systemjs.config.js',
  // list of files to serve (will not automatically be loaded)
  serveFiles: [
    'src/**/*',
    'node_modules/**/*'
  ],
  // list of files to insert <script> tag for
  includeFiles: [
    'node_modules/angular2/bundles/angular2-polyfills.js'
  ],
  config: {
    transpiler: null,
    paths: {
      'systemjs': '/node_modules/systemjs/dist/system.js',
      'system-polyfills': '/node_modules/systemjs/dist/system-polyfills.js',
      'es6-module-loader': '/node_modules/es6-module-loader/dist/es6-module-loader.js'
    },
  }
}
```

- create folder `src/test` and `test/unit`
- create file `src/test/unit/my-component.spec.ts`:

```javascript
import {MyComponentCmp} from '../../src/my-component/my-component-cmp'

describe('MyComponent', () => {
  it('Should be defined', () => {
    expect(MyComponentCmp).toBeDefined()
  })
})
```

- add test script to `package.json`:

```json
"scripts": {
  "test-unit": "karma start",
  "test": "npm run test-unit"
}
```

- run tests `npm run test-unit`
- add testing to `ghooks` config in `package.json`:

```json
"config": {
  "ghooks": {
    "pre-commit": "npm run build",
    "pre-push": "npm run build && npm run test-unit"
  }
}
```

## Testing - E2E tests

- `npm install -D protractor gulp`
- add the file `protractor.conf.js`:

```javascript
exports.config = {
  baseUrl: 'http://localhost:3000/src/',
  specs: ['src/test/e2e/**/*.spec.js'],
  directConnect: true,
  exclude: [],
  multiCapabilities: [{
    browserName: 'chrome'
  }],
  allScriptsTimeout: 110000,
  getPageTimeout: 100000,
  framework: 'jasmine2',
  jasmineNodeOpts: {
    isVerbose: false,
    showColors: true,
    includeStackTrace: false,
    defaultTimeoutInterval: 400000
  },
  // ng2 related configuration
  // useAllAngular2AppRoots: tells Protractor to wait for any angular2 apps on
  // the page instead of just the one matching `rootEl`
  useAllAngular2AppRoots: true
}
```

- add the file `gulpfile.js`:

```javascript
var gulp = require('gulp')
var path = require('path')
var child_process = require('child_process')
var express = require('express')
var http = require('http')
var server = http.createServer(express().use(express.static(__dirname)))

gulp.task('protractor-run', ['serve-files'], (done) => {
  var argv = process.argv.slice(3) // forward args to protractor
  child_process.spawn(getProtractorBinary('protractor'), argv, {
    stdio: 'inherit'
  }).once('close', done)
})
gulp.task('serve-files', (done) => server.listen(3000, done))
gulp.task('test-e2e', ['protractor-run'], (done) => server.close())

function getProtractorBinary(binaryName){
  var winExt = /^win/.test(process.platform)? '.cmd' : ''
  var pkgPath = require.resolve('protractor')
  var protractorDir = path.resolve(path.join(path.dirname(pkgPath), '..', 'bin'))
  return path.join(protractorDir, '/' + binaryName + winExt)
}
```

- create folder `src/test/e2e`
- create file `src/test/e2e/my-component.page-object.ts`

```javascript
// globals from protractor
declare var element, by

export class MyComponentPageObject {
  public myComponentEl = element(by.tagName('p'))
}
```

- create file `test/e2e/my-component.spec.ts`:

```javascript
// globals from protractor
declare var describe, it, expect, beforeEach, browser

import { MyComponentPageObject } from './my-component.page-object'

describe('MyComponentPageObject' , () => {
  beforeEach(() => {
    browser.get('/')
  })

  let pageObject = new MyComponentPageObject()
  it('should be a text in the paragraph', () => {
    expect(pageObject.myComponentEl.getText()).toEqual('My Component')
  })
})
```

- add tasks for e2e tests into `package.json`:

```json
"scripts": {
  "postinstall": "npm run install_webdriver",
  "install_webdriver": "node_modules/protractor/bin/webdriver-manager update",
  "test-e2e": "npm run tsc && gulp test-e2e"
}
```

- Install `webdriver` by `npm run install_webdriver`
- Run the e2e tests `npm run test-e2e`

## Publishing

- Specify main file in `package.json`

```json
{
  "main": "src/my-component/my-component-cmp.js"
}
```

- `package.json` script `prepublish` specifies what should be run before publishing

```json
"scripts": {
  "prepublish": "npm run build"
}
```

there are two ways of specifying which files to include in the NPM package:

- `.npmignore` lists which files to ignore

```bash
src/test
examples
.gitignore
```

- `package.json` files property lists which files to include

```json
"files": [
  "src/my-component/"
]
```

- When everything is ready, publish the package `npm publish`

## TypeScript watcher using Gulp

The command `tsc -p src -w` has been working extremely slow for me so I decided to use gulp for the watcher instead.

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
