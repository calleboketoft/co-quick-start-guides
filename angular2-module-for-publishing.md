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
node_modules
npm-debug.log
*.log
src/jspm_packages
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
    "prepublish": "npm run tsc",
    "tsc": "tsc -p src",
    "watch": "tsc -p src -w",
    "start": "node server"
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
      System.import('example/bootstrap')
    </script>
  </head>
  <body>
    <app>Loading...</app>
  </body>
</html>
```

- mkdir `src/example`
- create file `src/example/bootstrap.ts`

```javascript
import { bootstrap } from 'angular2/angular2'
import { AppCmp } from './app-cmp'
bootstrap(AppCmp)
```

- create file `src/example/app.ts`

```javascript
import { Component } from 'angular2/angular2'
@Component({
  selector: 'app',
  template: '<h1>Angular 2</h1>'
})
class AppComponent { }
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
import { MyComponentCmp } from '../my-component/my-component-cmp'

@Component({
  directives: [MyComponentCmp],
  template: `<my-component></my-component>`
})
```

- compile tsc and open server to view

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
  "prepublish": "npm run compile"
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