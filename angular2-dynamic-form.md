## Angular 2 dynamic form

- Basic form with built-in validators:

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

- Custom validators

```javascript
```