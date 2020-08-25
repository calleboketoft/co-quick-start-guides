# Angular 2 parent child

http://blog.mgechev.com/2016/01/23/angular2-viewchildren-contentchildren-difference-viewproviders/

- The children element which are located inside of its template of a component are called `view children`.
- Elements which are used between the opening and closing tags of the host element of a given component are called `content children`

## ViewChild

- `view-child-baby.component.ts`:

```javascript
import {Component} from '@angular/core'

@Component({
  selector: 'view-child-baby',
  template: `view child baby`
})
export class ViewChildBabyComponent {
  public fingers = 10
}
```

- `view-child-parent.component.ts`:

```javascript
import {Component, ViewChild} from '@angular/core'
import {ViewChildBabyComponent} from './view-child-baby.component'

@Component({
  directives: [ViewChildBabyComponent],
  selector: 'view-child-parent',
  template: `view child parent, <view-child-baby></view-child-baby>`
})
export class ViewChildParentComponent {
  @ViewChild(ViewChildBabyComponent) baby: ViewChildBabyComponent

  ngAfterViewInit () {
    console.log(this.baby.fingers)
  }
}
```
## ContentChild

- `inception.component.ts`:

```javascript
import {Component} from '@angular/core'

@Component({
  selector: 'inception',
  template: `Inception`
})
export class InceptionComponent {
  public mySecret = 'I have 9 toes'
}
```

- `content-child-baby.component.ts`:

```javascript
import {Component, ContentChild} from '@angular/core'
import {InceptionComponent} from './inception.component'

@Component({
  selector: 'content-child-baby',
  template: `<ng-content></ng-content>`
})
export class ContentChildBabyComponent {
  @ContentChild(InceptionComponent) inceptionComponent: InceptionComponent

  ngAfterViewInit () {
    console.log(this.inceptionComponent.mySecret)
  }
}
```

- `content-child-parent.component.ts`:

```javascript
import {Component} from '@angular/core'
import {ContentChildBabyComponent} from './content-child-baby.component'
import {InceptionComponent} from './inception.component'

@Component({
  selector: 'content-child-parent',
  directives: [ContentChildBabyComponent, InceptionComponent],
  template: `
    <content-child-baby>
      <inception></inception>
    </content-child-baby>
  `
})
export class ContentChildParentComponent {}
```