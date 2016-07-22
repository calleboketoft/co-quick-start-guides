# Smallest possible Angular 2 starter

An attempt at defining the minimal possible work to get a development project for Angular 2 up and running. Mainly usable for small experiment projects and such. I don't want to use any CLI or anything like that since I want to know all the things that happen.

- create dir `myproj`
- go to `myproj` and `npm init -y`
- install deps `npm install --save-dev @angular/core @angular/compiler @angular/common @angular/platform-browser @angular/platform-browser-dynamic rxjs@5.0.0-beta.6 zone.js@0.6.12 reflect-metadata es6-shim systemjs plugin-typescript`
- create `.gitignore`:

```bash
node_modules
.DS_Store
```

- create `index.html`:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Angular 2 ES6 App</title>
  </head>
  <body style="margin-top: 50px;">
    <app><div style="text-align: center;">Loading...</div></app>
    <script src="node_modules/es6-shim/es6-shim.min.js"></script>
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <script src="systemjs.config.js"></script>
    <script>
      System.import('./client-src/main').catch(function(err) {console.error(err)})
    </script>
  </body>
</html>
```

- create `systemjs.config.js`:

```javascript
System.config({
  baseURL: '/',
  warnings: true,
  transpiler: 'ts',
  map: {
    // In-browser transpilation
    'typescript': 'node_modules/plugin-typescript/node_modules/typescript',
    'ts': 'node_modules/plugin-typescript/lib/',

    // App packages
    '@angular': 'node_modules/@angular',
    'rxjs': 'node_modules/rxjs',
  },
  packages: {
    // In-browser transpilation
    'typescript': {
      main: 'lib/typescript.js',
      meta: {'lib/typescript.js': {exports: 'ts'}}
    },
    'ts': {'main': 'plugin.js'},

    // App packages
    'client-src': {defaultExtension: 'ts'},
    'rxjs': {defaultExtension: 'js'},
    '@angular/common': {defaultExtension: 'js', main: 'index.js'},
    '@angular/compiler': {defaultExtension: 'js', main: 'index.js'},
    '@angular/core': {defaultExtension: 'js', main: 'index.js'},
    '@angular/platform-browser-dynamic': {defaultExtension: 'js', main: 'index.js'},
    '@angular/platform-browser': {defaultExtension: 'js', main: 'index.js'}
  }
})

```

- Create dir `client-src` and file `client-src/main.ts`

```javascript
import {bootstrap} from '@angular/platform-browser-dynamic'
import {Component} from '@angular/core'
@Component({
  selector: 'app',
  template: `<h1>Hey there</h1>`
})
class AppComponent {}
bootstrap(AppComponent).catch(err => console.error(err))
```

- Now serve files from `myproj` and open in browser