# Angular 2 Validators

### Async Validator

```javascript
import { FormControl } from '@angular/forms'
import { Observable } from 'rxjs/Rx'

export function asyncValidator (control: FormControl): Observable<any> {
  if (control.value.length <= 1) {
    return Observable.from([null])
  } else {
    return Observable.from([{ maxOne: false }])
  }
}
```

### Synchronous Validator

```javascript
import { FormControl } from '@angular/forms'

export function hexColorValidator(c: FormControl) {
  let HEX_COLOR_REGEXP = /^#[0-9A-F]{6}$/i

  return HEX_COLOR_REGEXP.test(c.value) ? null : {
    validateHexColor: {
      valid: false
    }
  }
}
```