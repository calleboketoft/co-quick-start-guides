# Presentational and smart components

### Install dependencies

`npx @angular/cli@v6.0.0-beta.6 new pres --minimal --prefix=pres --inline-style=true --inline-template=true`

### Set up base application

`cd pres`

Replace content of `app.component.ts` with the base application:
```javascript
import { Component } from '@angular/core'

@Component({
  selector: 'pres-root',
  template: `
    <h3>B-day app</h3>

    <!-- Name text input -->
    <input type="text" #textInput
      placeholder="First name" (keyup)="textChanges(textInput.value)">
    <br>

    <!-- Month dropdown -->
    <label>Month of birth
      <select #monthDropdown (change)="updateSelectedMonth(monthDropdown.value)">
        <option value="" selected disabled hidden>Select month</option>
        <option *ngFor="let month of months" [value]="month">
          {{month}}
        </option>
      </select>
    </label>

    <!-- Result display -->
    <p>{{ firstName }} has {{ monthsUntilBday }} months until birthday</p>
  `
})
export class AppComponent {
  months = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12]

  selectedMonth = 1

  firstName = ''

  monthsUntilBday = null

  updateSelectedMonth(selectedMonth) {
    this.selectedMonth = parseInt(selectedMonth, 10)
    this.calculateMonthsUntilBday()
  }

  textChanges(newText) {
    this.firstName = newText
  }

  calculateMonthsUntilBday() {
    const date = new Date()
    const currentMonth = date.getMonth() + 1
    let monthsUntilBday
    if (currentMonth > this.selectedMonth) {
      monthsUntilBday = 12 - currentMonth + this.selectedMonth
    } else {
      monthsUntilBday = this.selectedMonth - currentMonth
    }
    this.monthsUntilBday = monthsUntilBday
  }
}

```

Now run the app with `npm start`

## Part 1: Purify function

The function `calculateMonthsUntilbday` has some logics that would be worth testing. Let's make it a "pure function":

> - Given the same input, will always return the same output.
> - Produces no side effects.
> - Pure functions must not mutate external state.

[Master the JavaScript Interview: What is a Pure Function?](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-pure-function-d1c076bec976)

Identify variables that cause the function NOT to produce the same output based on the same input:

- `new Date()` produces different dates depending on when it's called

Identify side effects, code that alters external state:

- `this.selectedMonth` is a property on the class
- `this.monthsUntilBirthday` is also a property on the class

Rewriting the function to make it pure, it could look like this:

```javascript
// NOTE that the function has a new, more generic, name as well
calculateMonthsUntilSelected(currentMonth, selectedMonth) {
  if (currentMonth > selectedMonth) {
    return 12 - currentMonth + selectedMonth
  }
  return selectedMonth - currentMonth
}
```

Now add that function to the class `AppComponent` and replace the previous function `calculateMonthsUntilBday` with the following code:

```javascript
calculateMonthsUntilBday () {
  const date = new Date()
  const currentMonth = date.getMonth() + 1
  this.monthsUntilBday = this.calculateMonthsUntilSelected(
    currentMonth,
    this.selectedMonth
  )
}
```

The new `calculateMonthsUntilBday` is not pure but at least we broke out the complicated calculations into another function.

## Part 2: Create service for the pure function

Pure functions are usually ready to be moved out from the component directly into a service.

Use Angular CLI to generate a new service called "bday-calculator":

`npx ng generate service bday-calculator --module=app.module.ts`

Cut out the function `calculateMonthsUntilSelected` from `app.component.ts` and paste it in the new service `bday-calculator.service.ts` (right underneath the constructor).

Now we need to import that service into `app.component.ts` so that we can use the function again.

```javascript
import { BdayCalculatorService } from './bday-calculator.service.ts'
```

And now instantiate it in the "Angular 5 way" in `app.component.ts` by adding a constructor function like following:
```javascript
constructor(private bdayCalculatorService: BdayCalculatorService) {}
```

Now we can use the service in the function `calculateMonthsUntilBday`:
```javascript
calculateMonthsUntilBday() {
  const date = new Date()
  const currentMonth = date.getMonth() + 1
  this.monthsUntilBday = this.bdayCalculatorService.calculateMonthsUntilSelected(
    currentMonth,
    this.selectedMonth
  )
}
```

## Part 3: Create presentational component

Presentational components are much like pure functions. They should only have inputs and outputs and not mutate external state.

The dropdown for selecting month has quite a bit of UI logics and would be nice to break out of the `app.component.ts` file. Create a new component for the month selector:

`npx ng generate component month-selector --module=app.module.ts`

Now copy over the HTML for the select from `app.component.ts` template to the template of `month-selector/month-selector.component.ts`:

```html
<!-- Month dropdown -->
<label>Month of birth
  <select #monthDropdown (change)="updateSelectedMonth(monthDropdown.value)">
    <option value="" selected disabled hidden>Select month</option>
    <option *ngFor="let month of months" [value]="month">
      {{month}}
    </option>
  </select>
</label>
```

Move over the `months` property from `app.component.ts` to `month-selector.component.ts`

Import `Output` and `EventEmitter` to `month-selector.component.ts`:
```javascript
import { Output, EventEmitter } from '@angular/core'
```

Add the `Output` decorator and function for emitting value first in the class of `MonthSelectorComponent`:
```javascript
@Output() monthChanged = new EventEmitter()
```

Use the event emitter in the template for the dropdown to emit the selected month directly:
```html
...
<select #monthDropdown (change)="monthChanged.emit(monthDropdown.value)">
...
```

Add the following element to the template in `app.component.ts`:
```html
<pres-month-selector (monthChanged)="updateSelectedMonth($event)">
</pres-month-selector>
```
