## NGRX Store Spying

- Create the `meta.reducer.ts` that will simply pass through anything

```javascript
export const metaReducer = (state = [], action) => action
```

- Create the `ngrx-store-spy.service.ts` that will listen to the `meta.reducer` and trigger on the beginning of the `action.type`

```javascript
import { Injectable } from '@angular/core'
import { Store } from '@ngrx/store'
import 'rxjs/add/operator/filter'

@Injectable()
export class NgrxStoreSpyService {
  constructor (
    private store: Store<any>,
    private pluginAppService: PluginAppService
  ) {
    this.store.select('metaReducer')
      .filter(action => action && action['type'])
      .subscribe((action: { type: string, payload: any }) => {
        let successStatuses = ['CREATED', 'LOADED', 'UPDATED', 'REMOVED', 'SUCCESS']

        if (successStatuses.some(type => action.type.startsWith(type))) {
          this.handleSuccess(action);
        } else if (action.type.startsWith('ERROR')) {
          this.handleError(action)
        }
      })
  }

  public handleSuccess (action) {
    // Do something in here
  }

  public handleError (action) {
    // Do something in here
  }
}

```

### Error handler

The job of the error handler is to be used in effects to have a consistent way of handling errors that happen on the .catch of the http observable.

- Create the `effect-error.service.ts`:

```javascript
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs/Observable';

export const ERROR_HTTP_GET = 'ERROR_HTTP_GET'
export const ERROR_HTTP_POST = 'ERROR_HTTP_POST'
export const ERROR_HTTP_PUT = 'ERROR_HTTP_PUT'
export const ERROR_HTTP_DELETE = 'ERROR_HTTP_DELETE'

export interface ErrorHttp {
  action: any
  error: any
  muteAlert?: boolean
}

@Injectable()
export class ErrorService {

  public errorHttpGet (options: ErrorHttp) {
    return this.errorHttp(ERROR_HTTP_GET, options);
  }

  public errorHttpPost (options: ErrorHttp) {
    return this.errorHttp(ERROR_HTTP_POST, options);
  }

  public errorHttpPut (options: ErrorHttp) {
    return this.errorHttp(ERROR_HTTP_PUT, options);
  }

  public errorHttpRemove (options: ErrorHttp) {
    return this.errorHttp(ERROR_HTTP_DELETE, options);
  }

  private errorHttp (errorActionType, {action, error, muteAlert = false}) {
    return Observable.of({
      type: errorActionType,
      payload: {
        action: action.type,
        error,
        toast: !muteAlert
      }
    })
  }
}

```

- Use the error service like this:

```javascript
// failing effect:
.catch(error => this.errorService.errorHttpGet({error, action}))
```

### integrating ngrx-store-spy.service with the SSG plugin app architecture

```javascript
public handleSuccess (action) {
  // Currently, only the action.type is needed for the toast
  this.pluginAppService.sendMessageToSource({
    ngrxAction: {
      type: action.type,
      payload: {
        fromPluginApp: true
      }
    }
  });
}
public handleError (action) {
  this.pluginAppService.sendMessageToSource({
    ngrxAction: {
      type: action.type,
      payload: {
        toast: action.payload.toast,
        action: action.payload.action,
        // Since we can't send a function, prepare the error text here
        error: {
          textString: action.payload.error.text(),
          status: action.payload.error.status
        }
      }
    }
  });
}
```