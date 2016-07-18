# Module Loader: Webpack

- create folder `myproj` and run `npm init -y` in there
- install node modules `npm install --save-dev typescript webpack webpack-dev-server ts-loader`

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

- create `index.html`

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
    'vendor': './app/vendor',
    'app': './app/main'
  },
  // Bundle output format
  output: {
    path: __dirname,
    filename: './dist/[name].bundle.js'
  },
  // Load these files
  resolve: {
    extensions: ['', '.js', '.ts']
  },
  devtool: 'source-map',
  module: {
    loaders: [
      {
        // If file ending is .ts, use ts-loader
        test: /\.ts/,
        loaders: ['ts-loader'],
        exclude: /node_modules/
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

- Create folder `app` and file `app/app.ts`

```javascript
export function App () {
  this.param = 'ok'
}
```

- Create file `app/main.ts`

```javascript
import {App} from './app'
var app = new App()
console.log(app.param)
```

- Create file `app/vendor.ts` to import vendor scripts resolve
```javascript
// Nothing here yet
```

## Unit test with Karma

- `npm install --save-dev karma karma-chrome-launcher karma-jasmine karma-webpack`
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
      'test/**/*.spec.ts': ['webpack']
    },
    webpack: {
      module: webpackConfig.module,
      resolve: webpackConfig.resolve
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

