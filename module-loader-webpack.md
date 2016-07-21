# Module Loader: Webpack

- create folder `myproj` and run `npm init -y` in there
- install node modules `npm install --save-dev typescript webpack webpack-dev-server awesome-typescript-loader`

- add `.gitignore`

```bash
node_modules
dist
```

- modify `package.json`

```json
  "scripts": {
    "build": "webpack",
    "start": "webpack-dev-server"
  },
```

- create `client-src/index.html`

```html
<html>
  <head>
    <title>Module Loader: Webpack</title>
  </head>
  <body>
</body>
  <script src="dist/vendor.bundle.js"></script>
  <script src="dist/app.bundle.js"></script>
</html>
```

- create `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "moduleResolution": "node",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  "exclude": [
    "node_modules",
    "typings/main",
    "typings/main.d.ts"
  ]
}
```

- Add `webpack.config.js`

```javascript
var webpack = require('webpack')

module.exports = {
  // Bundles
  entry: {
    'vendor': './client-src/app/vendor',
    'app': './client-src/app/main'
  },
  // Bundle output format
  output: {
    path: path.resolve(__dirname, 'client-src/dist/'),
    publicPath: '/dist/',
    filename: '[name].bundle.js'
  },
  // Load these files
  resolve: {
    extensions: ['', '.js', '.ts']
  },
  devtool: 'inline-source-map',
  devServer: {
    stats: 'minimal',
    contentBase: 'client-src/'
  },
  module: {
    loaders: [
      {
        // If file ending is .ts, use ts-loader
        test: /\.ts/,
        loaders: ['awesome-typescript-loader'],
        exclude: helpers.root('node_modules', 'test')
      }
    ]
  },
  plugins: [
    // Put common stuff in "vendor" so it's not included in "app"
    new webpack.optimize.CommonsChunkPlugin({
      names: ['app', 'vendor']
    })
  ]
}
```

- Create folder `client-src/app` and file `client-src/app/app.ts`

```javascript
export function App () {
  this.param = 'ok'
}
```

- Create file `client-src/app/main.ts`

```javascript
import {App} from './app'
var app = new App()
console.log(app.param)
```

- Create file `client-src/app/vendor.ts` to import vendor scripts resolve
```javascript
// Nothing here yet
```

## Unit test with Karma

- `npm install --save-dev karma karma-chrome-launcher karma-jasmine karma-webpack jasmine-core karma-sourcemap-loader`
- Add script `"test": "karma start karma.config.js"` to `package.json`
- Add file `karma.config.js`

```javascript
var webpackConfig = require('./webpack.config')

module.exports = function (config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine'],
    // each file acts as entry point for the webpack configuration
    files: ['test/**/*.spec.ts'],
    preprocessors: {
      // Specs need to be preprocessed by webpack and get sourcemaps
      'test/**/*.spec.ts': ['webpack', 'sourcemap']
    },
    webpack: {
      module: webpackConfig.module,
      resolve: webpackConfig.resolve,
      devtool: 'inline-source-map',
      // There's a bug in the sourcemap generation so this script is needed
      // https://github.com/webpack/karma-webpack/issues/13
      // https://github.com/webpack/karma-webpack/issues/109#issuecomment-224961264
      plugins: [
        new webpack.SourceMapDevToolPlugin({
          filename: null, // if no value is provided the sourcemap is inlined
          test: /\.(ts|js)($|\?)/i // process .js and .ts files only
        })
      ]
    },
    webpackMiddleware: {
      noInfo: true
    },
    reporters: ['progress'],
    browsers: ['Chrome'],
    singleRun: true,
    concurrency: Infinity
  })
}
```

- Add folder `test` and file `test/app.spec.ts`

```javascript
declare var describe, it, expect

import {App} from '../app/app'

describe('something', () => {
  it('should pass a dummy test', () => {
    var app = new App()
    expect(app.param).toEqual('ok')
  })
})
```

## Add typings

- `npm install typings --save-dev`
- `./node_modules/.bin/typings install dt~jasmine --global --save`
- Search for typings `./node_modules/.bin/typings search papaparse`

## Add Angular 2

- `npm install --save-dev @angular/core @angular/compiler @angular/common @angular/platform-browser @angular/platform-browser-dynamic rxjs@5.0.0-beta.6 zone.js@0.6.12 core-js`

- Update file `client-src/app/vendor.ts`

```javascript
// Polyfills
import 'core-js/es6'
import 'core-js/es7/reflect'
import 'zone.js/dist/zone'

// Angular 2
import '@angular/platform-browser'
import '@angular/platform-browser-dynamic'
import '@angular/core'
import '@angular/common'
```

- Update file `client-src/app/main.ts`

```javascript
import {bootstrap} from '@angular/platform-browser-dynamic'
import {AppComponent} from './app.component'
bootstrap(AppComponent)
  .catch(err => console.error(err))
```

- Add file `client-src/app/app.component.ts`

```javascript
import {Component} from '@angular/core'
@Component({
  selector: 'app',
  template: `
    <h1>Angular 2</h1>
  `
})
export class AppComponent {}
```

- Update file `client-src/index.html`

```html
<body>
  <app><div style="text-align: center;">Loading...</div></app>
  ...
</body>
```

