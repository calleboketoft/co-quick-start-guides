# Angular 2 dynamic form

How to use FormBuilder and creating custom validators

`main.ts`:

```javascript
import {bootstrap} from '@angular/platform-browser-dynamic'

import {disableDeprecatedForms, provideForms} from '@angular/forms';

import {AppComponent} from './app.component'
bootstrap(AppComponent, [
  provideForms(),
  disableDeprecatedForms()
])
  .catch(err => console.error(err))
```

`app.ts`

```javascript
import {Component} from '@angular/core'
import {FormBuilder, REACTIVE_FORM_DIRECTIVES, Validators} from '@angular/forms'

@Component({
  selector: 'app',
  directives: [REACTIVE_FORM_DIRECTIVES],
  template: `
    <form [formGroup]="myForm" novalidate class="form-inline"
      (ngSubmit)="submitForm(myForm.value, myForm.valid)">
      <input type="text" class="form-control" formControlName="myName">
      <small [hidden]="fc.myName.valid || (fc.myName.pristine && !submitted)">
        Name is required (minimum 5 characters).
      </small>
      <button type="submit" class="btn btn-primary">
        Submit
      </button>
    </form>
  `
})
export class AppComponent {
  public myForm
  public fc

  constructor (private formBuilder: FormBuilder) {}

  ngOnInit () {
    this.myForm = this.formBuilder.group({
      myName: ['', [
        Validators.required,
        Validators.minLength(5)
      ]]
    })
    // shorthand for templates
    this.fc = this.myForm.controls
  }

  public submitForm (...args) {
    console.log(args)
  }
}
```

## Custom sync validator

- Create `email.validator.ts`

```javascript
import {FormControl} from '@angular/forms'

export function emailValidator(c: FormControl) {
  let EMAIL_REGEXP = /^[a-z0-9!#$%&'*+\/=?^_`{|}~.-]+@[a-z0-9]([a-z0-9-]*[a-z0-9])?(\.[a-z0-9]([a-z0-9-]*[a-z0-9])?)*$/i;

  return EMAIL_REGEXP.test(c.value) ? null : {
    validateEmail: {
      valid: false
    }
  }
}
```

- Update `app.ts`

```html
<input type="email" class="form-control" formControlName="myEmail">
<small [hidden]="fc.myEmail.valid || (fc.myEmail.pristine && !submitted)">
  Email needs to be valid
</small>
```

```javascript
import {emailValidator} from './email.validator'
...
this.myForm = this.formBuilder.group({
  ...
  myEmail: ['', [emailValidator]]
  ...
})
...
```

## Custom async validator

- Update `app.ts`

```html
<input type="text" class="form-control" formControlName="myEmail">
<small [hidden]="fc.asyncValue.valid || (fc.asyncValue.pristine && !submitted)">
  Length max one
</small>
```

```javascript
import {asyncValidator} from './async.validator'
...
this.myForm = this.formBuilder.group({
  ...
  asyncField: ['', [], [asyncValidator]]
  ...
})
...
```

- Create `async.validator.ts`

```javascript
import {FormControl} from '@angular/forms'
import {Observable} from 'rxjs/Rx'

export function asyncValidator (control: FormControl): Observable<any> {
  if (control.value.length <= 1) {
    return Observable.from([null])
  } else {
    return Observable.from([{ maxOne: true }])
  }
}
```
