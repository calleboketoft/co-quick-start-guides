# Angular 2 Router

- modify `bootstrap.ts`:

```javascript
import {bind} from 'angular2/core'
import {bootstrap} from 'angular2/platform/browser'
import {ROUTER_PROVIDERS, LocationStrategy, HashLocationStrategy} from 'angular2/router'

import {AppCmp} from './app-cmp'

bootstrap(AppCmp, [
  ROUTER_PROVIDERS,
  bind(LocationStrategy).toClass(HashLocationStrategy)
])
```

- add `app-cmp.ts`:

```javascript
import {Component} from 'angular2/core'
import {ROUTER_DIRECTIVES, RouteConfig} from 'angular2/router'
import {PageCmp} from './page-cmp'

@Component({
  selector: 'app',
  directives: [ROUTER_DIRECTIVES],
  template: `
    <a [routerLink]="['/Page']">Page</a><br>
    <router-outlet></router-outlet>
  `
})
@RouteConfig([
  {path: '/page', as: 'Page', component: PageCmp}
])
export class AppCmp {}
```

- add `page-cmp.ts`:

```javascript
import {Component} from 'angular2/core'

@Component({
  selector: 'page',
  template: `
    <h1>Page</h1>
  `
})
export class PageCmp {}
```