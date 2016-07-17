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
export function app () {
  console.log('ok')
}
```

- Create file `app/main.ts`

```javascript
import {app} from './app'
app()
```

- Create file `app/vendor.ts` to import vendor scripts resolve
```javascript
// Nothing here yet
```
