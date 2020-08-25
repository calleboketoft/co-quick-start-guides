# Module Loader: SystemJS

Minimal example of loading TypeScript modules using SystemJS

- Create new project `myproject` and run `npm init -y` in there
- Install node modules `npm install --save-dev systemjs plugin-typescript express`
- Create minimal server

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

- Create `systemjs.config.js`

```javascript
SystemJS.config({
  transpiler: 'ts',
  packages: {
    'app': {
      defaultExtension: 'ts'
    },
    'ts': {
      'main': 'plugin.js'
    },
    'typescript': {
      'main': 'lib/typescript.js',
      'meta': {
        'lib/typescript.js': {
          'exports': 'ts'
        }
      }
    }
  },
  map: {
    'ts': 'node_modules/plugin-typescript/lib/',
    'typescript': 'node_modules/typescript/'
  }
})
```

- Create `index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Angular 2 ES6 App</title>
  </head>
  <body>
    <script src="node_modules/systemjs/dist/system.js"></script>
    <script src="systemjs.config.js"></script>
    <script>
      SystemJS.import('./app/main')
    </script>
  </body>
</html>
```

- Create folder `app` and file `app/main.ts`

```javascript
import {app} from './app'
app()
```

- Create file `app/app.ts`

```javascript
export function app () {
  console.log('ok')
}
```
