# Angular 2 and bootstrap modal

### Add modal

- `npm install --save ng2-bootstrap`

- Hack to make modal component work. Add the following code to the root component:

```javascript
import {ViewContainerRef} from '@angular/core'

export class AppComponent {
  public viewContainerRef
  public constructor(viewContainerRef: ViewContainerRef) {
    // You need this small hack in order to catch application root view container ref
    this.viewContainerRef = viewContainerRef
  }
}
```

- Modal abstraction component

```javascript
import {Component, ViewChild} from '@angular/core'
import {
  MODAL_DIRECTIVES,
  BS_VIEW_PROVIDERS,
  ModalDirective
} from 'ng2-bootstrap/ng2-bootstrap'

@Component({
  selector: 'example-modal',
  directives: [MODAL_DIRECTIVES],
  viewProviders: [BS_VIEW_PROVIDERS],
  template: `
    <div bsModal class="modal fade" tabindex="-1">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" (click)="thisModal.hide()">
              <span>&times;</span>
            </button>
            <h4 class="modal-title">Example Modal</h4>
          </div>
          <div class="modal-body">
            ...
          </div>
        </div>
      </div>
    </div>
    `
})
export class ExampleModalComponent {
  @ViewChild(ModalDirective) public thisModal: ModalDirective
}
```

- Using the modal component in another component

```javascript
import {Component, ViewChild} from '@angular/core'
import {ExampleModalComponent} from './example-modal.component'

@Component({
  directives: [ExampleModalComponent],
  selector: 'modal-parent',
  template: `
    <button class="btn btn-primary" (click)="showExampleModal()">
      Open example modal
    </button>
    <example-modal></example-modal>
  `
})
export class ModalParentComponent {
  @ViewChild(ExampleModalComponent) public exampleModal: ExampleModalComponent

  public showExampleModal () {
    this.exampleModal.thisModal.show()
  }
}
```

### Further use

Now you can use `@Input`, `<ng-content>`, `ViewChild`, etc. like usual