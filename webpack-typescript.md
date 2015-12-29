# Webpack TypeScript React

https://github.com/TypeStrong/ts-loader

- create dir for project `mkdir myproj`
- `cd myproj`
- `npm init -y`
- `npm install -D webpack webpack-dev-server typescript ts-loader tsd`

- create `webpack.config.js`

```javascript
module.exports = {
  entry: './src/app.tsx',
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
      {
        test: /\.tsx?$/,
        loader: 'ts-loader',
        exclude: '/node_modules/'
      }
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
  "files": [
    "./typings/tsd.d.ts",
    "./src/app.tsx"
  ]
}
```

- create `tsd.json`

```json
{
  "version": "v4",
  "repo": "borisyankov/DefinitelyTyped",
  "ref": "master",
  "path": "typings",
  "bundle": "typings/tsd.d.ts",
  "installed": {
  }
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
    "start": "webpack-dev-server --content-base public/",
    "webpack": "webpack --progress --colors",
    "webpack:watch": "webpack --progress --colors --watch",
    "postinstall": "tsd reinstall --overwrite --save"
  }
}
```

- now the whole thing is ready
- `npm start`
- navigate in browser to http://localhost:8080/webpack-dev-server/
- the server compiles and live-reloads on changes

## Add React

- add `postinstall` script to `package.json`

```json
"postinstall": "tsd reinstall --overwrite --save"
```

- update `tsconfig.json`

```json
{
  "compilerOptions": {
    "jsx": "react"
  },
  "files": [
    "./typings/react/react.d.ts",
    "./typings/tsd.d.ts",
    "./src/app.tsx"
  ]
}
```

- update `tsd.json`

```json
{
  "installed": {
    "react/react.d.ts": {
      "commit": "b3aeee64552e324581fdbe0926f4a96e981e451b"
    }
  }
}
```