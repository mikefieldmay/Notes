**Directives**
There are 2 types of directives: Attribute and Structural.
Attribute directives sit on elements just like attributes. They only ever change the element they sit on.
They look like a normal HTML attribute.
Structural directives are similar, but they also change the structure of the DOM around this element.
They look like a normal attribute, but they have a leading `*`.

We can't have more than one structural directive on a element.

```
// Example of `*ngFor` and `*ngIf` Structural Directives
// and [ngStyle] and [ngClass] attribute directives

<div class="container">
  <div class="row">
    <div class="col-xs-12">
      <button
        class="btn btn-primary"
        (click)="onlyOdd = !onlyOdd">Only show odd numbers</button>
      <br><br>
      <ul class="list-group">
        <div *ngIf="onlyOdd"> // If the value onlyOdd is true display this div
          <li
            class="list-group-item"
            [ngClass]="{odd: odd % 2 !== 0}" Dynamically adds a class to an element
            [ngStyle]="{backgroundColor: odd % 2 !== 0 ? 'yellow' : 'transparent'}" // Dynamically adds a style to an element
            *ngFor="let odd of oddNumbers"> // Iterates through the array oddNumbers
            {{ odd }}
          </li>
        </div>
         <div *ngIf="!onlyOdd"> // If the value onlyOdd is false display this div
          <li
            class="list-group-item"
            [ngStyle]="{backgroundColor: even % 2 !== 0 ? 'yellow' : 'transparent'}" // Dynamically adds a style to an element
            *ngFor="let even of evenNumbers"> // Iterates through the array oddNumbers
            {{ even }}
          </li>
        </div>
      </ul>
      <ng-template [ngIf]="onlyOdd">
        <p>Only odd</p>
      </ng-template>
    </div>
  </div>
</div>

```

*Creating your own directives*
Example of a basic highlight directive:

```
import { Directive, ElementRef, OnInit } from "@angular/core";


@Directive({ //Directive decorator must be imported from @angular/core
    selector: '[appBasicHighlight]' //Every directive needs a unique selector. The [] tell Angular that this is a selector.
})
export class BasicHighlightDirective implements OnInit{

    constructor(private elementRef: ElementRef){ // We need to pass in the element that this directive is placed on so we have access to it with ElementRef
    }

    ngOnInit(){
        this.elementRef.nativeElement.style.backgroundColor = 'green';
        // this changes the background colour to green, but is bad practice as it accesses the DOM directly.
    }
}

// HTML:

<p appBasicHighlight>Style me with basic highlight directive</p> // we don't need [] around the selector

```

*N.B* new directives must be imported into app-module and added to the app declarations.

You can generate directives using the CLI with the command `ng generate directives directive-name`.
It is good practice to store directives in the same folder.

You should use `Renderer2` to change the attributes of elements.
Example of a more complex directive:

```
import { Directive, OnInit, Renderer2, ElementRef } from '@angular/core';

@Directive({
  selector: '[appBetterHighlight]'
})
export class BetterHighlightDirective implements OnInit {

  constructor(private elementRef: ElementRef, private renderer: Renderer2) { }

  ngOnInit(){
    this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'blue')
    // renderer's set style method takes 4 args (3 are necessary).
    // first arg is the element, second is the style, third is the property and the optional 4th is any tags (e.g important)
  }

}

```

This is a better method as Angular occasionally does stuff where it doesn't render the DOM, and if you access the DOM directly it can cause errors. These cases are very unlikely (service workers), but using Renderer is generally considered better practice. https://angular.io/api/core/Renderer2

*Using Host Listener*

The `@HostListener` decorator can be added to directives so they become responsive to events. It can react to all events the DOM has available.
```
@HostListener('mouseenter') mouseover(eventData: Event) { //reacts tot he mouseenter event and calls the mouseover method
    this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'blue')
  }

@HostListener('mouseleave') mouseleave(eventData: Event) {
  this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'transparent')
}

```

*Host Binding*

We can use HostBinding to bind to host properties:
`@HostBinding('style.backgroundColor') background-color: string;`
We are saying, on the element that this directive sits please access the style property and the sub property background color and change it to whatever the property backgroundColor is set to.
```
import { Directive, Renderer2, ElementRef, HostListener, HostBinding  } from '@angular/core';

@Directive({
  selector: '[appBetterHighlight]'
})
export class BetterHighlightDirective {

  constructor(private elementRef: ElementRef, private renderer: Renderer2) { }
  @HostBinding('style.backgroundColor') backgroundColor: string;

  @HostListener('mouseenter') mouseover(eventData: Event) {
    this.backgroundColor = 'blue';
  }

  @HostListener('mouseleave') mouseleave(eventData: Event) {
    this.backgroundColor = 'transparent'
  }

}

```

Host binding can bind to any property of the element it's sitting on.

We can bind to the properties of our own directives by placing the properties on the same elements.
```
import { Directive, Renderer2, ElementRef, HostListener, HostBinding, Input, OnInit  } from '@angular/core';

@Directive({
  selector: '[appBetterHighlight]'
})
export class BetterHighlightDirective implements OnInit {

  constructor(private elementRef: ElementRef, private renderer: Renderer2) { }
  @Input() defaultColor: string = 'transparent';
  @Input() highlightColor: string = 'blue';
  @HostBinding('style.backgroundColor') backgroundColor: string;

  ngOnInit(){
    this.backgroundColor = this.defaultColor;
  }

  @HostListener('mouseenter') mouseover(eventData: Event) {
    this.backgroundColor = this.highlightColor;
  }

  @HostListener('mouseleave') mouseleave(eventData: Event) {
    this.backgroundColor = this.defaultColor;
  }

}


<p appBetterHighlight
   [defaultColor]="'yellow'"
   [highlightColor]="'blue'">
   Style me with better highlight directive
</p>
```

Furthermore, a typical use case if we only have one main property to bind to is to set the directive in [] ([ngClass]="className"). We can do this by using an alias. This is optional though and by default the directive name is not enclosed in square brackets.
```
@Directive({
  selector: '[appBetterHighlight]'
})
export class BetterHighlightDirective implements OnInit {

  constructor(private elementRef: ElementRef, private renderer: Renderer2) { }
  @Input() defaultColor: string = 'transparent';
  @Input('appBetterHighlight') highlightColor: string = 'blue'; //we pass the selector name as an alias to input
  @HostBinding('style.backgroundColor') backgroundColor: string;

  ngOnInit(){
    this.backgroundColor = this.defaultColor;
  }

  @HostListener('mouseenter') mouseover(eventData: Event) {
    this.backgroundColor = this.highlightColor;
  }

  @HostListener('mouseleave') mouseleave(eventData: Event) {
    this.backgroundColor = this.defaultColor;
  }

}

<p [appBetterHighlight]="'blue'" // We then reference our directive in the HTML as so.
   [defaultColor]="'yellow'">
   Style me with better highlight directive
</p>
```

If you are passing a string with property binding you can omit the square brackets and second quotation marks:
```
<p [appBetterHighlight]="'blue'" // We then reference our directive in the HTML as so.
   defaultColor="yellow">
   Style me with better highlight directive
</p>
```

*Structural Directives*

A `*` indicates a structural directive. Angular recognises the `*` and transforms structural directives into something else.
 ```
 <div *ngIf="true">
  <p>True content</p>
</div> // is converted to:

<ng-template [ngIf]="true">
  <div>
   <p>True content</p>
  </div>
</ng-template>
 ```
 It is essentially syntactic sugar. `<ng-template>` it is not rendered, but can render a template based on a condition.

 ```
 import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

 @Directive({
   selector: '[appUnless]'
 })
 export class UnlessDirective {
   @Input() set appUnless(condition: boolean) {  // we have to make sure the property we bind to matches the name of the selector
     if (!condition){
       this.vcRef.createEmbeddedView(this.templateRef); // the view container reference creates an embedded view using the template ref
     } else {
       this.vcRef.clear(); // if condition is true it clears the view container
     };
   }
   // set allows a method to be called whenever this condition changes. It is still a property though (for property binding)
   //

   constructor(private templateRef: TemplateRef<any>, private vcRef: ViewContainerRef) {

   }
   // we need a reference to both the template (<ng-template>) and the view container it sits in

 }

 ```

 *ngSwitch*

 If we want to display messages based on a value change. We could bind to a condition. It works as an extension of ngIf.
 ```
 <div [ngSwitch]="value">
  <p *ngSwitchCase="5">Value is 5</p>
  <p *ngSwitchCase="15">Value is 15</p>
  <p *ngSwitchCase="25">Value is 25</p>
  <p *ngSwitchDefault="">Value is Default</p>
</div>
 ```
