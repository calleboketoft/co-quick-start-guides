##Quick start client - JSPM and TypeScript

**Prerequisites**

Node.js must be installed [https://nodejs.org/]()

`http-server` should be installed, in case you don't have another static file serving solution you prefer `>npm install -g https-server`

___

Open a terminal and do the following steps:

- `>mkdir myNewProject && cd myNewProject`

- `>npm init`

- `>npm install jspm@beta --save-dev`

- `>./node_modules/jspm/jspm.js init`

Answer with these details when JSPM init asks:

- public folder path: `src`

- transpiler: `typescript`

create `.gitignore`:

```bash
node_modules
src/jspm_packages
.DS_Store
```

create `tsconfig.json`:

```javascript
{
  "compilerOptions": {
    "target": "ES5",
    "module": "commonjs",
    "sourceMap": true
  }
}
```
open up the jspm config file `src/config.js`

add package configuration that says that all TypeScript files under the `app` dir end with `.ts`

```javascript
System.config({
  "packages": {
    "app": {
      "defaultExtension": "ts"
    }
  }
})
```

create `src/index.html`:

```html
<html>
  <head>
    <script src="jspm_packages/system.js"></script>
    <script src="config.js"></script>
  </head>
  <body>
    <script>System.import('app/bootstrap')</script>
  </body>
</html>
```

create `src/app/bootstrap.ts`

```javascript
console.log('bootstrapped!')
```

start a http server in `src`, `>http-server`, open in browser and check console

## Adding Angular 1.x and UI Router

`>./node_modules/jspm/jspm.js install angular angular-ui-router` to install the libs

Add the following code to `bootstrap.ts`

```javascript
import angular from 'angular'
import app from './app_module'

angular.element(document).ready(() => {
  var appContainer = document.querySelector('body')
  var uiViewEl = document.createElement('div')
  uiViewEl.setAttribute('ui-view', '')
  appContainer.appendChild(uiViewEl)
  angular.bootstrap(appContainer, [app.name], {
    strictDi: true
  })
})
```

Create the file `src/app/app_module.ts`

```javascript
import angular from 'angular'
import 'angular-ui-router'

class RootController {
  welcome = 'JSPM, TypeScript, and Angular!'
}

export default angular.module('app', ['ui.router'])
.config(appConfigFun)
.controller('RootController', RootController)

appConfigFun.$inject = ['$stateProvider']
function appConfigFun ($stateProvider) {
  $stateProvider.state('root', {
    url: '',
    controller: 'RootController as vm',
    template: '<h2>{{::vm.welcome}}</h2>'
  })
}
```


