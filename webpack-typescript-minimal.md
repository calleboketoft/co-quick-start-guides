Minimal webpack project with typescript

- > mkdir proj
- > cd proj && npm init -y
- > npm install --save-dev typescript ts-loader webpack webpack-cli html-webpack-plugin html-loader clean-webpack-plugin webpack-dev-server
- Add `webpack.config.js`:

```javascript
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: path.resolve(__dirname, './src/index.ts'),
  devtool: 'inline-source-map',
  devServer: {
    contentBase: path.join(__dirname, 'dist'),
    port: 9000,
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        exclude: /node_modules/,
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js'],
  },
  output: {
    filename: 'bundle.[hash].js',
    path: path.resolve(__dirname, 'dist'),
  },
  plugins: [
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      template: path.resolve(__dirname, './src/index.html'),
    }),
  ],
};

```

Add `tsconfig.json`:

```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": false,
    "module": "es6",
    "target": "es5",
    "allowJs": false,
    "sourceMap": true
  }
}
```

Add scripts to `package.json`:

```json
  "scripts": {
    "build": "webpack",
    "start": "webpack serve"
  },
```

Add `src/index.html`:

```html
<html>
  <head></head>
  <body></body>
</html>
```

Add `src/index.ts`:

```typescript
console.log('test');
```
