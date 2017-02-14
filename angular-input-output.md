# Angular 2 @Input and @Output

The @Input and @Output are always registered on the child in relation to the parent. So if a child expects data from the parent, it has an `@Input`. If the child is going to send data to its parent it’s an `@Output`. The parent handles that stuff from the template of the child `<(eventFromChild) [dataToChild]>`.

## Event and data from child to parent

- Parent component

The parent has an event listener with a handler function specified on the child component's template `<child-cmp (boop)="boopHandler()">`.

```javascript
import {Component} from 'angular2/core'
import {ChildCmp} from './child-cmp'
@Component({
  selector: 'parent-cmp',
  template: `
    <strong>Parent</strong><br>
    <child-cmp (boop)="boopHandler()"></child-cmp>
  `,
  directives: [ChildCmp]
})
export class ParentCmp {
  boopHandler () {
    alert('booped!')
  }
}
```

- Child component

The child component has an instantiated EventEmitter decorated as an @Output `@Output('boop') iAmBoopEmitter = new EventEmitter()` that can emit events by calling `iAmBoopEmitter.emit($event)`.

```javascript
import {Component, Output, EventEmitter} from 'angular2/core'
@Component({
  selector: 'child-cmp',
  template: `
    <div>Child</div>
    <button (click)="iAmBoopEmitter.emit($event)">Click me</button>
  `
})
export class ChildCmp {
  @Output('boop') iAmBoopEmitter = new EventEmitter()
}
```
---

## Data to child from parent

- Parent component

The parent sends in data to child component with the bracket notation `[dataForChild]="theData"`.

```javascript
import {Component} from 'angular2/core'
import {ChildCmp} from './child-cmp'
@Component({
  selector: 'parent-cmp',
  template: `
    <strong>Parent</strong><br>
    <child-cmp [dataFromParent]="thingamabob"></child-cmp>
  `,
  directives: [ChildCmp]
})
export class ParentCmp {
  thingamabob = 'pizza'
}
```

- Child component

The child receives the data with a decorated parameter `@Input dataFromParent`.

```javascript
import {Component, Input} from 'angular2/core'
@Component({
  selector: 'child-cmp',
  template: `<div>Child</div>`
})
export class ChildCmp {
  @Input() dataFromParent
}
```