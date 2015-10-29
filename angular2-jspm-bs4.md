# ng2-jspm-bs4-quickstart

## Static file serving

- slc loopback
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

---

## Angular 2 with JSPM

- add to `.gitignore`: `client/jspm_packages`
- `npm install --save jspm`
- `./node_modules/.bin/jspm init`
- public files in `client`
- transpiler `typescript`
- add to `package.json`

```json
"scripts": { "postinstall": "./node_modules/.bin/jspm install" }
```

- install angular 2 `./node_modules/.bin/jspm install angular2`

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
import 'zone.js'
import 'reflect-metadata'
import { Component, bootstrap } from 'angular2/angular2';

@Component({
  selector: 'app',
  template: '<h1>Hello {{ name }}</h1>'
})
class AppComponent {
  name: string
  constructor () {
    this.name = 'Calle'
  }
}

bootstrap(AppComponent)
```

---
#Angular 2 Router

- modify `client/app/bootstrap.ts`:

```javascript
import 'reflect-metadata'
import 'zone.js'
import { bind, bootstrap } from 'angular2/angular2';
import { routerBindings, LocationStrategy, HashLocationStrategy } from 'angular2/router'

import { AppCmp } from 'app/components/app-cmp'

bootstrap(AppCmp, [
  routerBindings(AppCmp),
  bind(LocationStrategy).toClass(HashLocationStrategy)
])
```

- add `client/app/components/app-component.ts`:

```javascript
import { Component } from 'angular2/angular2'
import { ROUTER_DIRECTIVES, RouteConfig } from 'angular2/router'
import { PageCmp } from './page-cmp'

@Component({
  selector: 'app',
  template: `<a [router-link]="['/Page']">Page</a><br>
             <router-outlet></router-outlet>`,
  directives: [ROUTER_DIRECTIVES]
})
@RouteConfig([
  { path: '/page', as: 'Page', component: PageCmp }
])
export class AppCmp {
  name: string
  constructor () {
    this.name = 'Calle'
  }
}
```

- add `client/app/components/page-cmp.ts`:

```javascript
import { Component } from 'angular2/angular2'

@Component({
  selector: 'page',
  template: `<h1>Page</h1>`
})
export class PageCmp {}

```

---

## Bootstrap 4 alpha

- `npm install --save-dev git+https://git@github.com/twbs/bootstrap.git#v4.0.0-alpha`
- `npm install --save-dev gulp gulp-sass`

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
"scripts": { "postinstall": "./node_modules/.bin/gulp sass" }
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
