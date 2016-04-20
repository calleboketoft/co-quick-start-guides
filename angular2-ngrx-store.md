# @ngrx/store

- `npm install --save @ngrx/store`

- Add to `systemjs.config.ts`:

```javascript
System.config({
  map: {
    '@ngrx/store': '../node_modules/@ngrx/store'
  },
  packages: {
    '@ngrx/store': {
      main: 'dist/index.js',
      defaultExtension: 'js'
    }
  }
})
```

- Add to `bootstrap.ts`

```javascript
import {provideStore} from '@ngrx/store';
import {dataReducer} from './dataReducer';

bootstrap(App, [
  provideStore({
    dataReducer
  }, {
    dataReducer: [] // initial value
  })
]);
```

- Creating the reducer `data-reducer.ts`:

```javascript
export const ADDED_DATA_LIST = 'ADDED_DATA_LIST'
export const CREATED_DATA_ITEM = 'CREATED_DATA_ITEM'

export const dataReducer = (state = [], {type, payload}) => {
  switch (type) {

    case ADDED_DATA_LIST:
      return payload

    case CREATED_DATA_ITEM:
      return [...state, payload]

    default:
      return state
  }
}

```

- Getting and using the reducer:

```javascript
import {Store} from '@ngrx/store'
import * as dataActions from './data-reducer'

class MyClass {
  private _dataReducer;
  constructor (private _store: Store<any>) {
    this._dataReducer = _store.select('dataReducer')
  }

  public createSomeData (data) {
    this._store.dispatch({
      type: dataActions.CREATED_DATA_ITEM,
      payload: {my: 'big data object'}
    })
  }
}
```