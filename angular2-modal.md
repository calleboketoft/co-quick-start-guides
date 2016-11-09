# Angular 2 and bootstrap modal

### Add modal

- `npm install --save ng2-bootstrap`

- Hack to make modal component work. Add the following code to the root component:

```javascript
import { ViewContainerRef } from '@angular/core'

export class AppComponent {
  // You need this small hack in order to catch application root view container ref
  public constructor(public viewContainerRef: ViewContainerRef) { }
}
```

- Modal abstraction component

```javascript
import { Component, Input, ViewChild } from '@angular/core'
import { ModalDirective } from 'ng2-bootstrap/ng2-bootstrap'

@Component({
  selector: 'basic-modal',
  styles: [`
    .modal-header {
      border-radius: 0.3rem 0.3rem 0 0;
    }
  `],
  template: `
    <div bsModal class="modal fade" tabindex="-1" [config]="modalConfig">
      <div [class]="_modalSize">
        <div class="modal-content">
          <div [class]="_headerClass">
            <button type="button" class="close" (click)="modal.hide()">
              <span>&times;</span>
            </button>
            <h4 class="modal-title">
              <ng-content select=".bsmodal-title"></ng-content>
            </h4>
          </div>
          <div class="modal-body">
            <ng-content select=".bsmodal-body"></ng-content>
          </div>
          <div class="modal-footer">
            <ng-content select=".bsmodal-footer"></ng-content>
          </div>
        </div>
      </div>
    </div>
  `
})
export class BasicModalComponent {

  @ViewChild(ModalDirective) public modal: ModalDirective;

  @Input() set headerClass (value) {
    this._headerClass += (' ' + value)
  }
  public _headerClass = 'modal-header'

  @Input() set modalSize (value) {
    switch (value) {
      case 'modal-sm':
      case 'small':
        this._modalSize += 'modal-sm'
        break

      case 'modal-lg':
      case 'large':
        this._modalSize += 'modal-lg'
        break
    }
  }
  public _modalSize = 'modal-dialog '

  public modalConfig = {
    backdrop: false
  }
}
```

- Using the modal component in another component

```javascript
import { Component, ViewChild } from '@angular/core'
import { BasicModalComponent } from './example-modal.component'

@Component({
  selector: 'modal-parent',
  template: `
    <!-- Example modal -->
    <basic-modal #exampleModal [headerClass]="'alert-warning'">
      <div class="bsmodal-title">Example modal</div>
      <div class="bsmodal-body">Some text</div>
      <div class="bsmodal-footer">
        <div style="text-align: right;">
          <button type="button" class="btn btn-secondary"
            (click)="exampleModal.modal.hide()">
            Cancel
          </button>
        </div>
      </div>
    </basic-modal>
  `
})
export class ModalParentComponent {
  @ViewChild('exampleModal') public exampleModal: BasicModalComponent;

  public showExampleModal () {
    this.exampleModal.modal.show()
  }
}
```

You also need to import the ModalModule from ng2-bootstrap into the module where you plan to use it

### Further use

Now you can use `@Input`, `<ng-content>`, `ViewChild`, etc. like usual