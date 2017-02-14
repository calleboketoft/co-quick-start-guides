# Angular 2 Module for Publishing

Here's a complete module following this guide: [co-selectable-items](https://github.com/calleboketoft/co-selectable-items)

## Skeleton

- mkdir `myproj`
- cd `myproj`
- `git init`
- `npm init -y`
- `npm install --save-dev --save-exact @angular/{core,compiler,common,platform-browser,platform-browser-dynamic} rxjs zone.js reflect-metadata systemjs typescript express`

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
    "lib": ["es2015", "dom"],
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false,
    "suppressImplicitAnyIndexErrors": true
  },
  "exclude": [
    "node_modules"
  ]
}
```

- add scripts and ghooks config to package.json

```json
"scripts": {
  "start": "node server",
  "build": "tsc",
  "watch": "tsc -w",
},
"config": {
  "ghooks": {
    "pre-commit": "npm run build"
  }
},
```

## Component example

- create file `index.html`

```html
<html><head><meta http-equiv="refresh" content="0; URL='/client-src'" /></head></html>
```

- create folder `client-src`
- create file `client-src/systemjs.config.ts`

```javascript
declare var System
System.config({
  baseURL: '/',
  warnings: true,
  map: {
    '@angular/core': 'node_modules/@angular/core/bundles/core.umd.js',
    '@angular/common': 'node_modules/@angular/common/bundles/common.umd.js',
    '@angular/compiler': 'node_modules/@angular/compiler/bundles/compiler.umd.js',
    '@angular/platform-browser': 'node_modules/@angular/platform-browser/bundles/platform-browser.umd.js',
    '@angular/platform-browser-dynamic': 'node_modules/@angular/platform-browser-dynamic/bundles/platform-browser-dynamic.umd.js',
    'rxjs': 'node_modules/rxjs'
  },
  packages: {
    'client-src': { defaultExtension: 'js' },
    'rxjs': { defaultExtension: 'js' },
    '@angular/common': { defaultExtension: 'js' },
    '@angular/compiler': { defaultExtension: 'js' },
    '@angular/core': { defaultExtension: 'js' },
    '@angular/platform-browser': { defaultExtension: 'js' },
    '@angular/platform-browser-dynamic': { defaultExtension: 'js' }
  }
})
```

- create file `client-src/index.html`

```html
<!DOCTYPE html>
<html>

<head>
  <title>Web app</title>
</head>

<body style="margin-top: 50px;">
  <app>
    <div style="text-align: center;">Loading...</div>
  </app>
  <script src="../node_modules/zone.js/dist/zone.js"></script>
  <script src="../node_modules/reflect-metadata/Reflect.js"></script>
  <script src="../node_modules/systemjs/dist/system.src.js"></script>
  <script src="systemjs.config.js"></script>
  <script>
      System.import('./example/main').catch(function(err) {
        console.error(err)
      })
    </script>
</body>

</html>
```

- create folder `client-src/example`
- create file `app.module.ts`

```javascript
import { NgModule } from '@angular/core'
import { BrowserModule } from '@angular/platform-browser'
import { AppComponent } from './app.component'

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  bootstrap: [AppComponent],
})
export class AppModule { }
```

- create file `client-src/example/main.ts`

NOTE: bootstrapping code is separated from example so that the example code
can be used as a component by itself in a separate repo.

```javascript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic'
import { AppModule } from './app.module'

platformBrowserDynamic().bootstrapModule(AppModule)
```

- create file `client-src/example/app.component.ts`

```javascript
import {Component} from '@angular/core'
@Component({
  selector: 'app',
  template: `
    <h1>Angular 2</h1>
  `
})
export class AppComponent {}
```

- The base of the app is ready to view!
- compile tsc `npm run build` and start server `npm start` to view in browser `http://localhost:3000`


## Testing - Unit tests

- Install dependencies `npm install --save-dev jasmine-core karma karma-chrome-launcher karma-jasmine karma-systemjs`
- Initialise Karma `./node_modules/.bin/karma init`
- Update `karma.config.js`, replace "frameworks" and "files", and add "systemjs":

```javascript
frameworks: ['systemjs', 'jasmine'],
plugins: [
  'karma-jasmine',
  'karma-chrome-launcher',
  'karma-systemjs'
],
files:['client-src/test/unit/*.spec.js'],
systemjs: {
  configFile: 'client-src/systemjs.config.js',
  // list of files to serve (will not automatically be loaded)
  serveFiles: [
    'client-src/**/*',
    'node_modules/**/*'
  ],
  // list of files to insert <script> tag for
  includeFiles: [
    'node_modules/es6-shim/es6-shim.min.js',
    'node_modules/zone.js/dist/zone.js',
    'node_modules/reflect-metadata/Reflect.js',
    'node_modules/systemjs/dist/system.src.js'
  ],
  config: {
    transpiler: null,
      paths: {
        'systemjs': '/node_modules/systemjs/dist/system.js'
      }
  }
}
```

- create folder `client-src/test/unit`
- create file `client-src/test/unit/my-page.spec.ts`:

```javascript
import {MyPageComponent} from '../../src/my-page/my-page.component'

describe('MyPageComponent', () => {
  it('Should be defined', () => {
    expect(MyPageComponent).toBeDefined()
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

#### Debugging Unit Tests
In order to debug the unit tests in browser, run the test command like this:

- `npm run test-unit -- --no-single-run --browsers Chrome`
- Chrome will open up with a page to start the tests.
- Click `debug` up right and a new tab will open.
- Open dev tools and press `sources`.
- In there you'll find the test source files

## Testing - E2E tests

- `npm install --save-dev protractor gulp`
- add the file `protractor.conf.js`:

```javascript
exports.config = {
  baseUrl: 'http://localhost:3000/src/',
  specs: ['client-src/test/e2e/**/*.spec.js'],
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

Running the tests manually would be done by first starting the development server at `localhost:3000` with the command `npm start` and then running the protractor tests towards that server with `./node_modules/.bin/protractor`. The following `gulp` solution makes it into just one command:

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
- create "page object" file `src/test/e2e/my-component.po.ts`

```javascript
import {
  browser, element, by, By, $, $$, ExpectedConditions
} from 'protractor/globals'

export let myPagePo {
  myPageEl: () => element(by.tagName('p'))
}
```

- create file `test/e2e/my-page.spec.ts`:

```javascript
import {
  browser, element, by, By, $, $$, ExpectedConditions
} from 'protractor/globals'

import {myPagePo} from './my-page.po'

describe('MyPagePageObject' , () => {
  beforeEach(() => {
    browser.get('/')
  })

  it('should be a text in the paragraph', () => {
    expect(myPagePo.myPageEl().getText()).toEqual('My Page')
  })
})
```

- add tasks for e2e tests into `package.json`:

```json
"scripts": {
  "install_webdriver": "node_modules/protractor/bin/webdriver-manager update",
  "test-e2e": "gulp test-e2e"
}
```

- Ensure latest webdriver is installed `npm run install_webdriver` and run the e2e tests `npm run test-e2e`

#### Debugging E2E Tests
- Add line `browser.pause()` anywhere in a spec file to pause the test in terminal.
- To run Protractor commands in the terminal, go to REPL by typing `repl`
- In REPL mode you can run commands such as `element(by.id('mything')).getText()`

- When a test needs to wait for the browser, this line might be of help: `browser.wait(() => authenticatePo.signInButton().isPresent(), 5000)`

#### Protractor Cheat Sheet

- [Cheat Sheet](https://gist.github.com/javierarques/0c4c817d6c77b0877fda)

## Publishing

- Specify main file in `package.json`

```json
{
  "main": "client-src/my-page/my-page.component.js"
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
  "client-src/my-page/"
]
```

- When everything is ready, publish the package `npm publish`
