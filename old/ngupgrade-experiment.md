> npx @angular/cli@1.7.0-beta.3 new ng-mashup --minimal=true --style=less --prefix=mashup --inline-template

> cd ng-mashup && npm install @angular/upgrade@5.2.0 @uirouter/angularjs bootstrap --save

create file `.prettierrc`:

```json
{
  "printWidth": 80,
  "singleQuote": true,
  "useTabs": false,
  "tabWidth": 2,
  "semi": true,
  "bracketSpacing": true,
  "parser": "typescript"
}
```

create dir `src/app-ng1` and create file in there `app-ng1-module.ts`:

```javascript
import * as angular from "angular";
import uiRouter from "@uirouter/angularjs";

export let AppNg1Module = angular.module("app", [uiRouter]).config([
  "$stateProvider",
  function($stateProvider) {
    $stateProvider.state({
      name: "hello",
      url: "/hello",
      template: "<h2>Testing</h2>"
    });
  }
]);
```

update the contents of `src/app/app.module.ts`:

```javascript
import { AppNg1Module } from '../app-ng1/app-ng1-module';

import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { UpgradeModule } from '@angular/upgrade/static';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    UpgradeModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule {
  constructor (private upgradeModule: UpgradeModule) {
    this.ngDoBootstrap();
  }

  private ngDoBootstrap () {
    const htmlEl = document.getElementsByTagName('html')[0];
    this.upgradeModule.bootstrap(htmlEl, AppNg1Module.name, { strictDi: true });
  }
}
```

Add to the body of `index.html`:

```html
    <a ui-sref="hello">hello</a>
    <ui-view></ui-view>
```

Update `angular-cli.json`:

```json
      "styles": [
        "../node_modules/bootstrap/scss/bootstrap.scss"
      ],
```
