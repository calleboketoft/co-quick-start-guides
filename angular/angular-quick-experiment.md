# Angular 2 Quick Experiment

## Skeleton

- mkdir `myproj`
- cd `myproj`
- `npm init -y`
- `yarn add --exact --dev @angular/{core,compiler,common,platform-browser,platform-browser-dynamic} rxjs zone.js core-js typescript webpack webpack-dev-server awesome-typescript-loader`

- Add scripts to `package.json`:

```json
    "build": "webpack",
    "watch": "webpack-dev-server"
```

- create file `.gitignore`

```bash
.DS_Store
node_modules
*.log
client/dist
```

- create file `tsconfig.json`

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "lib": ["es2015", "dom"],
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false,
    "suppressImplicitAnyIndexErrors": true
  },
  "exclude": [
    "node_modules"
  ]
}
```

- add file `webpack.config.js`:

```javascript
var path = require('path')
var webpack = require('webpack')

module.exports = {
  entry: {
    'main': './client/app/main'
  },
  output: {
    path: path.resolve(__dirname, './client/dist'),
    publicPath: '/client/dist/',
    filename: '[name].bundle.js'
  },
  resolve: {
    extensions: ['.js', '.ts']
  },
  devtool: 'sourcemap',
  devServer: {
    stats: 'minimal',
    port: 3030
  },
  module: {
    loaders: [
      {
        test: /\.ts/,
        loaders: ['awesome-typescript-loader']
      }
    ]
  },
  plugins: [
    // https://github.com/AngularClass/angular2-webpack-starter/issues/993#issuecomment-246883410
    new webpack.ContextReplacementPlugin(
      /angular(\\|\/)core(\\|\/)(esm(\\|\/)src|src)(\\|\/)linker/,
      __dirname
    )
  ]
}
```

- create file `index.html`

```html
<html><head><meta http-equiv="refresh" content="0; URL='/client'" /></head></html>
```

- create folder `client`
- create file `client/index.html`

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Web app</title>
    <link href="../node_modules/bootstrap/dist/bootstrap.min.css" rel="stylesheet" />
  </head>
  <body style="margin-top: 50px;">
    <app-component><div style="text-align: center;">Loading...</div></app-component>
  </body>
</html>
```

- create folder `client/app`
- create file `client/app/app.module.ts`:

```javascript
import { NgModule } from '@angular/core'
import { BrowserModule } from '@angular/platform-browser'
import { AppComponent } from './app.component'

@NgModule({
  declarations: [AppComponent],
  imports: [BrowserModule],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

- create file `client/app/main.ts`:

```javascript
import 'core-js/es6'
import 'core-js/es7/reflect'

import 'zone.js/dist/zone'

import { platformBrowserDynamic } from '@angular/platform-browser-dynamic'
import { AppModule } from './app.module'

platformBrowserDynamic().bootstrapModule(AppModule)
```

- Create file `client/app/app.component.ts`:

```javascript
import { Component } from '@angular/core'
@Component({
  selector: 'app-component',
  template: `
    <h1>Angular 2</h1>
  `
})
export class AppComponent {}
```

- Run the experiment `npm run watch`
- Navigate browser to http://localhost:3030
