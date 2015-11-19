# Angular 2 Module for Publishing

## Skeleton

- create folder `myproj` for project
- cd `myproj`
- git init
- npm init (specify version 0.0.1)
- npm install --save-dev angular2 typescript express ghooks jspm
- mkdir `src`
- create file `.gitignore`

```bash
.DS_Store
node_modules
npm-debug.log
*.log
jspm_packages
src/css
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

- create file `src/tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": true
  }
}
```

- add scripts to package.json

```json
{
  "scripts": {
    "start": "node server",
    "build": "npm run tsc",
    "prepublish": "npm run build",
    "tsc": "tsc -p src",
    "watch": "tsc -p src -w",
    "postinstall": "jspm install"
  },
  "config": {
    "ghooks": {
      "pre-commit": "npm run build"
    }
  }
}
```

## Component example

- create file `index.html`

```html
<html><head><meta http-equiv="refresh" content="0; URL='/src'" /></head></html>
```

- cd `src`
- create file `src/index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Angular 2 ES6 App</title>
  </head>
  <body style="margin-top: 50px;">
    <app>Loading...</app>
    <script src="../jspm_packages/system.js"></script>
    <script src="../jspm.config.js"></script>
    <script>
      System.import('./example/bootstrap')
    </script>
  </body>
</html>
```

- mkdir `src/example`
- create file `src/example/bootstrap.ts`

NOTE: bootstrapping code is separated from example so that the example code
can be used as a component by itself in a separate repo.
```javascript
import 'reflect-metadata'
import 'zone.js'

import { bootstrap } from 'angular2/angular2'
import { AppCmp } from './app-cmp'
bootstrap(AppCmp)
```

- create file `src/example/app-cmp.ts`

```javascript
import { Component } from 'angular2/angular2'
@Component({
  selector: 'app',
  template: '<h1>Angular 2</h1>'
})
export class AppCmp { }
```

Set up JSPM:

- `node_modules/.bin/jspm init`
 - config file: `jspm.config.js`
 - transpiler: `typescript`
- `jspm install angular2 reflect-metadata zone.js`
- Add config to `jspm.config.js`:

```javascript
System.config({
  typescriptOptions: {
    "module": "commonjs",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  packages: {
    "src": {
      "defaultExtension": "ts"
    },
    "test": {
      "defaultExtension": "ts"
    }
  }
})
```

- the skeleton is ready!
- Install and compile `npm install`
- Start serving `npm start`
- Open browser at `localhost:3000/src`
- optionally add `bootstrap.css` for styling

```html
<link rel="stylesheet" href="example/bootstrap.css">
```

## Component

- create component folder `src/my-component`
- create component file `src/my-component/my-component-cmp.ts`

```javascript
import { Component } from 'angular2/angular2'
@Component({
  selector: 'my-component',
  template: '<p>My Component</p>'
})
export class MyComponentCmp { }
```

- import component to `src/example/app-cmp.ts` and enable it

```javascript
import { Component } from 'angular2/angular2'
import { MyComponentCmp } from '../my-component/my-component-cmp'
@Component({
  directives: [MyComponentCmp],
  selector: 'app',
  template: '<my-component></my-component>'
})
export class AppCmp { }
```

- compile tsc and open server to view

## Testing - Unit tests

- Install dependencies `npm install -D jasmine-core karma karma-chrome-launcher karma-jasmine karma-jspm`
- Initialise Karma `node_modules/.bin/karma init`
- Update `karma.config.js`:

```javascript
config.set({
  jspm: {
    config: 'jspm.config.js',
    loadFiles: [
      'test/**/*.ts'
    ],
    serveFiles: [
      'src/my-component/*.ts'
    ]
  },
  frameworks: ['jspm', 'jasmine'],
  proxies: {
    '/src/': '/base/src/',
    '/test/': '/base/test/',
    '/jspm_packages/': '/base/jspm_packages/'
  },
  singleRun: true
})
```

- create folder `test` and `test/unit`
- create file `test/unit/my-component.spec.ts`:

```javascript
import 'reflect-metadata'
import 'zone.js'
import { MyComponentCmp } from '../../src/my-component/my-component-cmp'

describe('MyComponent', function () {
  it('Should be defined', function () {
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

- `npm install -D protractor`
- add the file `protractor.conf.js`:

```javascript
exports.config = {
  baseUrl: 'http://localhost:3000/src/',
  specs: ['test/e2e/**/*.spec.js'],
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
};
```

- create folder `test/e2e`
- create file `test/e2e/my-component.page-object.ts`

```javascript
export class MyComponentPageObject {
  public myComponentEl = element(by.tagName('p'));
}
```

- create file `test/e2e/my-component.spec.ts`:

```javascript
import { MyComponentPageObject } from './my-component.page-object'

describe('MyComponentPageObject' , () => {
  beforeEach(() => {
    browser.get('/')
    let pageObject = new MyComponentPageObject()
    it('should be a text in the paragraph', () => {
      expect(pageObject.myComponentEl.getText()).toEqual('My Component')
    })
  })
})
```

- add tasks for e2e tests into `package.json`:

```json
"scripts": {
  "postinstall": "jspm install && npm run webdriver",
  "webdriver": "node_modules/protractor/bin/webdriver-manager update",
  "tsc-e2e": "tsc -p test/e2e",
  "watch-tsc-e2e": "tsc -p test/e2e -w",
  "test-e2e": "protractor protractor.conf.js"
}
```

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
test
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

## Importing module using JSPM

- install `my-component` into angular 2 project by `jspm install npm:my-component`
- import into TypeScript file like this

```javascript
import { MyComponentCmp } from 'my-component'
```
