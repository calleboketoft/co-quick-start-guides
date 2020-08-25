# Angular 2 input debounce

## Using ngFormControl

```javascript
import {Component} from 'angular2/core'
import {Control} from 'angular2/common'
import 'rxjs/add/operator/debounceTime'

@Component({
  selector: 'using-form-control-cmp',
  template: `
    <input type='text' [ngFormControl]='myFormInput'>
  `
})
export class UsingFormControlCmp {
  myFormInput = new Control();
  constructor () {
    this.myFormInput.valueChanges
      .debounceTime(500)
      .subscribe((val) => {
        console.log(val)
      })
  }
}
```

## Using EventEmitter directly

```javascript
import {Component, EventEmitter} from 'angular2/core'
import 'rxjs/add/operator/debounceTime'

@Component({
  selector: 'app',
  template: `
    <input type='text' (keyup)='myInput.emit($event.target.value)' />
  `
})
export class AppCmp {
  myInput = new EventEmitter();

  constructor () {
    this.myInput
      .debounceTime(500)
      .subscribe(value => {
        console.log(value)
      })
  }
}
```
