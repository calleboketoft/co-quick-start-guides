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