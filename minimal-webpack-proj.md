Minimal webpack project with typescript

- >mkdir proj
- >cd proj && npm init -y
- >npm install --save-dev typescript@next ts-loader@next webpack@next
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
            }
        ]
    },
    resolve: {
        extensions: [ '.ts', '.tsx', '.js' ]
    }
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
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <div id="wrapper"></div>

        <script src="dist/main.js"></script>
    </body>
</html>
```

Add `src/index.ts`:

```typescript
console.log('test');
```