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
