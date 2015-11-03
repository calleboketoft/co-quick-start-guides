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
    "tsc": "tsc -p src",
    "start": "live-server --open=src/example"
  }
}
```

## Component example

- cd `src`, mkdir `example`
- create file `src/example/index.html`

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

- optionally add `bootstrap.css` for styling

```html
<link rel="stylesheet" href="example/bootstrap.css">
```

## Component

- create component folder `src/my-component`
