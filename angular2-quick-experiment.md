# Angular 2 Quick Experiment

## Skeleton

- mkdir `myproj`
- cd `myproj`
- `npm init -y`
- `yarn add --exact @angular/{core,compiler,common,platform-browser,platform-browser-dynamic} rxjs@5.0.0-rc.4 zone.js@0.7.2 reflect-metadata @types/core-js typescript webpack@2.1.0-beta.27 webpack-dev-server@2.1.0-beta.12 awesome-typescript-loader`

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
    "sourceMap": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "removeComments": false,
    "noImplicitAny": false
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
    <script src="../node_modules/zone.js/dist/zone.js"></script>
    <script src="../node_modules/reflect-metadata/Reflect.js"></script>
    <script src="./dist/main.bundle.js"></script>
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
