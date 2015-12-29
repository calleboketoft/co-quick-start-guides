# Webpack TypeScript React

https://github.com/TypeStrong/ts-loader

- create dir for project `mkdir myproj`
- `cd myproj`
- `npm init -y`
- `npm install -D webpack webpack-dev-server typescript ts-loader`
- create `webpack.config.js`

```javascript
module.exports = {
  entry: './src/app.ts',
  output: {
    filename: './public/bundle.js'
  },
  resolve: {
    // Add `.ts` and `.tsx` as a resolvable extension.
    extensions: ['', '.webpack.js', '.web.js', '.ts', '.tsx', '.js']
  },
  module: {
    loaders: [
      // all files with a `.ts` or `.tsx` extension will be handled by `ts-loader`
      { test: /\.tsx?$/, loader: 'ts-loader' }
    ]
  }
}
```

- create `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
  },
  "files": []
}
```

- create dir `src` and `public`
- create `src/app.ts`

```javascript
console.log('works')
```

- create `public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<body>
  <script src="bundle.js"></script>
</body>
</html>
```

- add script to `package.json`

```json
{
  "scripts": {
    "start": "webpack-dev-server --content-base public/"
  }
}
```

- now the whole thing is ready
- `npm start`
- navigate in browser to http://localhost:8080/webpack-dev-server/
- the server compiles and live-reloads on changes