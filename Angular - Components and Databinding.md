**Components and Databinding**

*Property and event binding*
We can pass data between components with property and event binding.
It involves emitting data from an event that we can listen for.

We can use property and event binding on:
- HTML Elements: Native Properties and events
- Directives: Custom Properties and events
- Components: Custom properties and events

By default all properties of components are only accessible within the component.

*Passing data down*
If you want properties of a child parent to be accessible from the parent you need to use the `@Input()` decorator.

```
<div class="container">
 <app-cockpit></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [element]="serverElement"></app-server-element>
        // because we have used the input decorator, we can access the element property of this component.
      </div>
    </div>
</div>


import { Component, OnInit, Input } from '@angular/core';

@Component({
  selector: 'app-server-element',
  templateUrl: './server-element.component.html',
  styleUrls: ['./server-element.component.css']
})
export class ServerElementComponent implements OnInit {
  @Input() element: {type: string, name: string, content: string};
  // allows this property to be accessed by the parent component

  constructor() { }

  ngOnInit() {
  }

}

```

Any component implementing this component through it's selector will be able to bind to the element.
We can also add aliases if we don't want to use the same property name inside and outside.
`@Input('srvElement')` would make `element` targetable by `srvElement` from outside the component.

*Passing data up*

`EventEmitter` is an object in Angular that allows you to emit your own events.
`serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();`
You pass in the type of data you want to emit between `< >` and then initialise the Event Emitter afterwards.
We need to add `@Output()` to make an event listenable from the outside.
```

//Example of component that passes data out:
import { Component, OnInit, EventEmitter, Output } from '@angular/core';

@Component({
  ...
})
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output() blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>()
  newServerName = '';
  newServerContent = '';

  constructor() { }

  ngOnInit() {
  }

  onAddServer() {
    this.serverCreated.emit({
              serverName: this.newServerName,
              serverContent: this.newServerContent});
  }
  // when these methods are called, data is emitted.

  onAddBlueprint() {
    this.blueprintCreated.emit({
              serverName: this.newServerName,
              serverContent: this.newServerContent});
  }

}

// The html of the 'parent' element
<div class="container">
 <app-cockpit
  (serverCreated)="onServerAdded($event)"
  (blueprintCreated)="onBlueprintAdded($event)"
  //listening for the serverCreated emission to then immediately call the onServerAdded event
  ></app-cockpit>
  <hr>
  <div class="row">
    <div class="col-xs-12">
      <app-server-element
        *ngFor="let serverElement of serverElements"
        [srvElement]="serverElement"></app-server-element>
      </div>
    </div>
</div>

// Parent component

import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  serverElements = [{type: 'server', name: 'Testserver', content: 'Test'}];

  onServerAdded(serverData: {serverName: string, serverContent: string}) {
    this.serverElements.push({
      type: 'server',
      name: serverData.serverName,
      content: serverData.serverContent
    });
  }

  onBlueprintAdded(serverData: {serverName: string, serverContent: string}) {
    this.serverElements.push({
      type: 'blueprint',
      name: serverData.serverName,
      content: serverData.serverContent
    });
  }

}
```

We can also add aliases to custom events.

These above features are incredibly important in angular. It allows us to use many components and have them talk to each other. There are some use cases where the distance between two components is too great. In that case we would use services.
Databinding is specifically used for input and output.

*View encapsulation*
View encapsulation is enforced by Angular which is not a default behaviour. It means that CSS is only applied to the component it belongs too and parent components don't pass styles down to children.
It gives a custom attribute to all elements in a component. It does this for each component with different unique attribute names. With that it can make sure that these styles are only applied to elements of that component. It emulates the shadow DOM but shadow DOM is not supported on all browsers.

It is possible to change view encapsulation if you import `ViewEncapsulation` and add it as a property of the component.
- `ViewEncapsulation.emulated` is the default and as such does not need to be set.
- `ViewEncapsulation.None` removes view encapsulation from the component. Css styles applied in this component are applied globally.
- `ViewEncapsulation.Native` this uses the shadow DOM. It will give you the same result as emulated, but is not supported on all browsers.

*Local References in templates*
If you don't need to use two way binding, another option is local references. A local reference is a reference to the html element, not just the value inside. It can only be referenced outside the element, but can be passed into html.
```
     <input
         type="text"
         class="form-control"
         #serverNameInput>    // This html element has the local reference serverNameInput  
     <label>Server Content</label>
     <input type="text" class="form-control" [(ngModel)]="newServerContent">
     <br>
     <button
       class="btn btn-primary"
       (click)="onAddServer(serverNameInput)">Add Server</button> //this HTML element is then passed as an argument into the component
     <button
       class="btn btn-primary"
       (click)="onAddBlueprint(serverNameInput)">Add Server Blueprint</button>
   </div>
 </div>
```

*Accessing the template DOM with @ViewChild*

View child can be imported through angular core into a component. It creates an Element Reference (`ElementRef` which is an Angular thing) to a html element rather than giving us the Element itself. The actual element can be accessed by calling the .nativeElement method on the ElementRef.

```
import { Component, OnInit, EventEmitter, Output, ViewChild, ElementRef } from '@angular/core';

@Component({
  selector: 'app-cockpit',
  templateUrl: './cockpit.component.html',
  styleUrls: ['./cockpit.component.css']
})
export class CockpitComponent implements OnInit {
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();
  @Output('bpCreated') blueprintCreated = new EventEmitter<{serverName: string, serverContent: string}>()
  // newServerName = '';
  // newServerContent = '';
  @ViewChild('serverContentInput') serverContentInput: ElementRef; // Creates a variable based on the local ref serverContentInput of type ElementRef

  constructor() { }

  ngOnInit() {
  }

  onAddServer(nameInput: HTMLInputElement) {
    this.serverCreated.emit({
              serverName: nameInput.value,
              serverContent: this.serverContentInput.nativeElement.value}); //uses the .nativeElement method access the html element underneath.
  }

  onAddBlueprint(nameInput: HTMLInputElement) {
    this.blueprintCreated.emit({
              serverName: nameInput.value,
              serverContent: this.serverContentInput.nativeElement.value});
  }

}
```
It is advised not to change the DOM in the typescript file.

*Projecting Content into components with ngContent*
Everything you place between the opening and closing tag of your component selector is lost by default. This can be overridden though with `<ng-content></ng-content>`tags placed inside the child element. This is good for creating things like widgets and tabs. We can access the `<ng-content>` from the adult view with the template reference by using `@ContentChild` rather than with `@ViewChild`.
```
// Parent:
<div class="row">
   <div class="col-xs-12">
     <app-server-element
       *ngFor="let serverElement of serverElements"
       [srvElement]="serverElement">
          <p>
         <strong *ngIf="serverElement.type === 'server'" style="color: red">{{ serverElement.content }}</strong>
         <em *ngIf="serverElement.type === 'blueprint'">{{ element.content }}</em>
       </p>
     </app-server-element>
   </div>
 </div>

 // Child:

 <div
 class="panel panel-default">
 <div class="panel-heading">{{ element.name }}</div>
 <div class="panel-body">
   <ng-content></ng-content>
 </div>
</div>

```

*Component Lifecycle*
ngOnInit is a lifecycle hook and there are a few lifecycle hooks. If a new component is created it goes through a few phases and we are able to access these phases with a few methods:
- ngOnChanges: Called after a bound property changes (whenever properties decorated with `@Input` receive new values)
- ngOnInit: Called once the component is initialised (it is not visible on the DOM yet but the object has been created)
- ngDoCheck: Called during every change detection run (whenever a property value changes and the DOM needs to be re rendered).
This can happen on any events as change detection occurs.
- ngAfterContentInit: Called after content (ng-content) has been projected into view
- ngAfterCintentChecked: Called every time the projected content has changed
- ngAfterViewInit: Called after the component's view (and child views) has been initialised (after the view is rendered)
- ngAfterViewChecked: Called everytime the views (and child views) have been initialised
- ngOnDestroy: Called once the component is about to be destroyed

Templates cannot be accessed before the view has been initialised. It works from AfterViewInit onwards.
