# RxJS patterns

- importing operators

```javascript
import 'rxjs/add/operator/map'
```

- importing observable
```javascript
import {Observable} from 'rxjs/Rx'
```

## Request based on response from another request

First get a list with detail free items. Then use the item ids from that list to make a subsequent call for those items to get all details.

```javascript
  function getItemsWithDetails () {
    // First get detail free item list
    return this.getDetailFreeItemList()
      // Prepare for flattening observables from each item request
      .flatMap(res => {
        // go through items in list, use id to make each item detail request
        let itemObsArr = res.json()
          .map(item => this.getDetailedItem(item.id))
        // all observables from item requests are in the itemObsArr.
        // Use zip to wait for all to respond and gather the responses in an array
        return Observable.zip(...itemObsArr)
      })
      // all item requests have responded, get the JSON from them
      .map(resArr => resArr.map(resItem => resItem.json()))
  }

  // function examples
  let getDetailFreeItemList = () => http.request(new Request(...))
  let getDetailedItem (itemId) => http.request(new Request(...))

  // calling main function
  getItemsWithDetails().subscribe(res => console.log(res))
  // => [{id: 1, name: 'item1'}, {id: 2, name: 'item2'}]

```

## Await multiple requests to complete

```javascript
Observable.zip(this.store.select('myReducer'), this.store.select('myOtherReducer'))
  .filter(res => <any>(res[0] && res[1])) // skip empty
  .take(1) // only need 1 response
  .subscribe(resolve) // both responses have arrived, resolve promise
```