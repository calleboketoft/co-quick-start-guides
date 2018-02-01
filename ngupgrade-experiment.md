>npx @angular/cli@1.7.0-beta.3 new ng-mashup --minimal=true --style=less --prefix=mashup --inline-template

>cd ng-masup && npm install @angular/upgrade@5.2.0 @uirouter/angularjs --save

create dir `src/app-ng1` and create file in there `app-ng1-module.ts`:

```javascript
import * as angular from 'angular';
import uiRouter from '@uirouter/angularjs';

export let AppNg1Module = angular.module('app', [uiRouter])
  .config(['$stateProvider', function ($stateProvider) {
    $stateProvider.state({
      name: 'hello',
      url: '/hello',
      template: '<h2>Testing</h2>'
    });
  }]);

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
    const ng1place = document.getElementById('ng1place');
    this.upgradeModule.bootstrap(ng1place, AppNg1Module.name, { strictDi: true });
  }
}

```

Add to the body of `index.html`:

```html
  <div id="ng1place">
    <a ui-sref="hello">hello</a>
    <ui-view></ui-view>
  </div>
```