# Angular 2 Directive

```javascript
import { Directive, ElementRef, Renderer } from '@angular/core'

@Directive({ selector: '[highlight]' })
/** Highlight the attached element in gold */
export class CallesDirective {
  constructor(renderer: Renderer, el: ElementRef) {
    renderer.setElementStyle(el.nativeElement, 'backgroundColor', 'gold')
    console.log(
      `* AppRoot highlight called for ${el.nativeElement.tagName}`)
  }
}
```