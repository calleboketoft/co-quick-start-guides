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
  entry: {
    'vendor': './app/vendor',
    'app': './app/main'
  },
  output: {
    path: __dirname,
    filename: './dist/[name].bundle.js'
  },
  resolve: {
    extensions: ['', '.js', '.ts']
  },
  devtool: 'source-map',
  module: {
    loaders: [
      {
        test: /\.ts/,
        loaders: ['ts-loader'],
        exclude: /node_modules/
      }
    ]
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin(/* chunkName= */"vendor", /* filename= */"./dist/vendor.bundle.js")
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
