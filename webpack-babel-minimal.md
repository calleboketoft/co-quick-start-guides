Minimal webpack project with babel

- >mkdir proj
- >cd proj && npm init -y
- >npm install --save-dev html-webpack-plugin html-loader babel-loader@next @babel/core @babel/preset-env webpack webpack-cli
- Add `webpack.config.js`:

```javascript
const HtmlWebPackPlugin = require('html-webpack-plugin');

module.exports = {
    devtool: 'inline-source-map',
    entry: './src/index.js',
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /(node_modules)/,
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env']
                    }
                }
            },
            {
                test: /\.html$/,
                use: [
                    {
                        loader: 'html-loader'
                    }
                ]
            }
        ]
    },
    resolve: {
        extensions: [ '.js' ]
    },
    plugins: [
        new HtmlWebPackPlugin({
            template: './src/index.html',
            filename: './index.html'
        })
    ]
};
```


Add scripts to `package.json`:
```json
  "scripts": {
    "build": "webpack --mode production",
    "watch": "webpack --mode development --watch"
  },
```

Add `src/index.html`:

```html
<html>
    <head></head>
    <body></body>
</html>
```

Add `src/index.js`:

```typescript
console.log('test');
```

now run command
```
npm run build
```

Installing a simple dev server:

```
npm install http-server --save-dev
```

Add to `package.json`
```
scripts: {
    ...
    "start": "http-server dist"
}
```