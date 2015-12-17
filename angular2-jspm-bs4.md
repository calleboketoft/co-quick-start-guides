# ng2-jspm-bs4-quickstart
This guide is my personal start for each new Angular 2 project. A guide by one of the core team members can be found here: https://gist.github.com/robwormald/429e01c6d802767441ec

- npm init -y
- `.gitignore`:

```bash
.DS_Store
node_modules
*.log
client/jspm_packages
```

## Static file serving

### Alt 1: Express

- `npm install -D express`
- `server.js`:

```javascript
var port = 3000
var staticDir = './client'
var express = require('express')
var app = express()
app.use(express.static(staticDir))
var server = app.listen(port, () => {
  console.log('serving at: ' + port)
})
```

- add to package.json:

```json
"scripts": {
  "start": "node server.js"
}
```

### Alt 2: Loopback

- `slc loopback`
- remove all files that look redundant from the generated package
- remove `server/boot/root.js`
- add to `server/middleware.json`:

```json
"files": {
  "loopback#static": {
    "params": "$!../client"
  }
}
```

- add to package.json:

```json
"scripts": {
  "start": "node server/server.js"
}
```

---

## Angular 2 with JSPM

- `npm install --save jspm angular2`
- `./node_modules/.bin/jspm init`
- `server baseURL` : `client`
- transpiler `typescript`
- add to `package.json`

```json
"scripts": {
  "postinstall": "jspm install"
}
```

- install angular 2 for jspm

```bash
./node_modules/.bin/jspm install angular2
```

- create `client/tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  }
}
```

- add to `client/config.js`:

```javascript
System.config({
  typescriptOptions: {
    "module": "commonjs",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  }
})
```

- create `client/index.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Angular 2 ES6 App</title>
  </head>
  <body>
    <app>Loading...</app>
    <script src="jspm_packages/system.js"></script>
    <script src="config.js"></script>
    <script>System.import('app/bootstrap');</script>
  </body>
</html>
```

**-- Temporary fix for Angular beta 0 --**

- Angular 2 is also needed in node_modules for angular2-polyfills.js (hopefully temporarily only)
- `npm install angular2`
- copy the file `node_modules/angular2/bundles/angular2-polyfills.js` to the folder `client/vendor/`
- Add the line `<script src="vendor/angular2-polyfills.js"></script>` above the script tag for `system.js`

**-- End of fix --**

- add to `client/config.js`:

```javascript
System.config({
  packages: {
    "app": {
      "defaultExtension": "ts"
    }
  }
})
```

- add file `client/app/bootstrap.ts`:

```javascript
import { Component } from 'angular2/core'
import { bootstrap } from 'angular2/platform/browser'

@Component({
  selector: 'app',
  template: '<h1>{{title}}</h1>'
})
class AppComponent {
  public title: string
  constructor () {
    this.title = 'up'
  }
}

bootstrap(AppComponent)
```

- run the server `npm start` and open `localhost:3000` in a browser

All should be up and running!

---
## Angular 2 Router

- modify `client/app/bootstrap.ts`:

```javascript
import { bind } from 'angular2/core'
import { bootstrap } from 'angular2/platform/browser'
import { ROUTER_PROVIDERS, LocationStrategy, HashLocationStrategy } from 'angular2/router'

import { AppCmp } from './components/app-cmp'

bootstrap(AppCmp, [
  ROUTER_PROVIDERS,
  bind(LocationStrategy).toClass(HashLocationStrategy)
])
```

- add `client/app/components/app-cmp.ts`:

```javascript
import { Component } from 'angular2/core'
import { ROUTER_DIRECTIVES, RouteConfig } from 'angular2/router'
import { PageCmp } from './page-cmp'

@Component({
  selector: 'app',
  template: `
    <a [routerLink]="['/Page']">Page</a><br>
    <router-outlet></router-outlet>
  `,
  directives: [ROUTER_DIRECTIVES]
})
@RouteConfig([
  { path: '/page', as: 'Page', component: PageCmp }
])
export class AppCmp {
  public name: string
  constructor () {
    this.name = 'Calle'
  }
}
```

- add `client/app/components/page-cmp.ts`:

```javascript
import { Component } from 'angular2/core'

@Component({
  selector: 'page',
  template: `<h1>Page</h1>`
})
export class PageCmp {}

```

---

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
  "postinstall": "gulp sass"
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
---
## Build and serve

- install NPM and JSPM + build SASS `npm install`
- start static file serving `npm start`

---
## Compile into one script file

- add to `package.json`:

```json
"scripts": {
  "compile": "cd client && ../node_modules/.bin/jspm bundle-sfx -m app/bootstrap compiled.js && cd .."
}
```

- rename `index.html` to `index-src.html`
- create new `index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <link href="css/bootstrap.css" rel="stylesheet" />
    <title>Angular 2 ES6 App</title>
  </head>
  <body>
    <app>Loading...</app>
    <script src="compiled.js"></script>
  </body>
</html>
```

- add to `.gitignore`

```bash
client/compiled.js
client/compiled.js.map
```