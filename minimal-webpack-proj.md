Minimal webpack project with typescript

- >mkdir proj
- >cd proj && npm init -y
- >npm install --save-dev typescript ts-loader webpack webpack-cli html-webpack-plugin html-loader
- Add `webpack.config.js`:

```javascript
'use strict';

module.exports = {
    devtool: 'inline-source-map',
    entry: './src/index.ts',
    module: {
        rules: [
            {
                test: /\.tsx?$/,
                loader: 'ts-loader'
            },
            {
                test: /\.html$/,
                use: [
                    {
                        loader: 'html-loader',
                        options: { minimize: true }
                    }
                ]
            }
        ]
    },
    resolve: {
        extensions: [ '.ts', '.tsx', '.js' ]
    },
    plugins: [
        new HtmlWebPackPlugin({
            template: './src/index.html',
            filename: './index.html'
        })
    ]
};
```

Add `tsconfig.json`:

```json
{
  "compilerOptions": {
      "sourceMap": true
  }
}
```

Add scripts to `package.json`:
```json
  "scripts": {
    "build": "webpack --mode production",
    "watch": "webpack --mode development --watch"
  },
```

Add `index.html`:

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
