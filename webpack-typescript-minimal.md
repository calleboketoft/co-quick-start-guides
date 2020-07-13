Minimal webpack project with typescript

- > mkdir proj
- > cd proj && npm init -y
- > npm install --save-dev typescript ts-loader webpack webpack-cli html-webpack-plugin html-loader webpack-dev-server
- Add `webpack.config.js`:

```javascript
const HtmlWebPackPlugin = require("html-webpack-plugin");
const path = require("path");

module.exports = {
  entry: "./src/index.ts",
  devtool: "inline-source-map",
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    compress: true,
    port: 9000,
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        loader: "ts-loader",
        exclude: /node_modules/,
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: "html-loader",
            options: { minimize: true },
          },
        ],
      },
    ],
  },
  resolve: {
    extensions: [".ts", ".tsx", ".js"],
  },
  plugins: [
    new HtmlWebPackPlugin({
      template: "./src/index.html",
      filename: "./dist/index.html",
    }),
  ],
  output: {
    filename: "bundle.js",
    path: path.resolve(__dirname, "dist"),
  },
};
```

Add `tsconfig.json`:

```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "sourceMap": true,
    "noImplicitAny": true,
    "module": "commonjs",
    "target": "es5",
    "jsx": "react",
    "allowJs": true
  }
}
```

Add scripts to `package.json`:

```json
  "scripts": {
    "build": "webpack --mode production",
    "serve": "webpack-dev-server"
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
console.log("test");
```
