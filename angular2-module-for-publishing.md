# Angular 2 Module for Publishing

## Skeleton

- create folder `myproj` for project
- cd `myproj`
- git init
- npm init (specify version 0.0.1)
- npm install --save-dev angular2 systemjs typescript express
- mkdir `src`
- create file `.gitignore`

```bash
.DS_Store
src/example/**/*.js
src/example/**/*.map
node_modules
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
    "tsc": "tsc -p src", // add -w for watching
    "start": "node server"
  }
}
```

## Component example

- cd `src`, mkdir `example`
- create file `src/index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <script src="../../node_modules/systemjs/dist/system.src.js"></script>
    <script src="../../node_modules/angular2/bundles/angular2.dev.js"></script>
    <script src="../../node_modules/angular2/bundles/router.dev.js"></script>
    <script>
      System.config({
        packages: {
          'example': { defaultExtension: 'js' },
          'my-component': { defaultExtension: 'js' }
        }
      })
      System.import('example/app')
    </script>
  </head>
  <body>
    <app>Loading...</app>
  </body>
</html>
```

- create file `example/app.ts`

```javascript
import {Component, bootstrap} from 'angular2/angular2'
@Component({
  selector: 'app',
  template: '<h1>Angular 2</h1>'
})
class AppComponent { }
bootstrap(AppComponent)
```

- the skeleton is ready! Compile `npm run tsc` and start serving `npm start`

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

- import component to `src/example/app.ts` and enable it

```javascript
import { MyComponentCmp } from '../my-component/my-component-cmp'

@Component({
  directives: [MyComponentCmp],
  template: `<my-component></my-component>`
})
```

- compile tsc and open server to view

## Publishing

- `package.json` script `prepublish` specifies what should be run before publishing

```json
"scripts": {
  "prepublish": "npm run tsc"
}
```

there are two ways of specifying which files to include in the NPM package:

- `.npmignore` lists which files to ignore

```bash
test
examples
.gitignore
```

- `package.json` files property lists which filest to include

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

## Testing

- TODO