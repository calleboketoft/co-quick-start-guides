# Angular 2 Module for Publishing

## Skeleton

- create folder `myproj` for project
- cd `myproj`
- git init
- npm init (specify version 0.0.1)
- npm install --save-dev angular2 systemjs typescript live-server
- mkdir `src`
- create file `.gitignore`

```bash
src/**/*.js
src/**/*.map
node_modules
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
    "start": "live-server --open=src"
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

- TODO
- prepublish compilation
- npm publish
- parts to ignore for the npm package

## Testing

- TODO