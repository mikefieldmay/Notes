**Angular**

Angular is a JavaScript Framework that allows you to create reactive Single-Page-Applications (SPA's). This means that everything that changes is rendered in the browser. This makes the site much quicker when making page changes and calls to databases.
Angular is a JS framework changing the DOM at runtime.

Why use the CLI?
- There are some extra steps such which the cli helps with.


*CLI Commands*
```
ng new name-of-project //will create the project
ng serve // will start up the project server and compiler the files
ng serve --port YOURPORT // will change the port number
```
All the code is written in the App folder. The assets folder stores images/data and any environmental variables are stored in environments.
When creating a new file it's a good idea to install bootstrap aswell.
`npm install --save bootstrap`
We then need to import bootstrap into the angular-cli.json file. The cli uses webpack bundles the files and adds them to the index.html file. Add `../node_modules/bootstrap/dist/css/bootstrap.min.css` into the styles array.

With an angular application, the index.html file is served. it has a tag for `<app-root>` which is a custom angular component.

Whenever `ng serve` runs the project it'll create bundles and add them to the index.html file which contains our own code. main.ts is the file that runs the app. It starts the app with the command `platformBrowserDynamic().bootstrapModule(AppModule);`.
In the app.module.ts file there is a bootstrap property which point to the AppComponent. It should list all the components that should be known to Angular at the time it compiles.

*App components*

```
// app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root', // this is the selector tag for the html file
  templateUrl: './app.component.html', // this is the template
  styleUrls: ['./app.component.css']
})
export class AppComponent { // allows the class to be used outside the file
  name = '';
}

```

Examples of components are header, main area, sidebar. Each component has it's own business logic, styling and template. It allows for great separation of concerns and re-usable code.

`thing.component.ts` in the folder `thing` is the naming convention.
A component is just a typescript class that is exported with the `export` keyword. The majority of items come from `@angular/core`, but there are other folders to import from.

You are able to change the selector for the html.
- `[app-root]` allows the selector to be an attribute. To add it in the html you would need to add it as such: `<div app-root></div>`
- `.app-root` allows you to add the tag in a class of an element e.g `<div class="app-root"></div>`

Components typically use the regular `app-root` selector.

You can use the command `ng generate component component_name` to automatically create the folder and the component. This also adds it to the moodule declarations.

*App module*
To use a component we need to add it to the app module. Angular uses components to build webpages and modules to bundle individual pieces into packages. A module is a bundle of functionalities of our app which gives Angular the information about the features of which the app uses.
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';
import { ServerComponent } from './server/server.component'; //tells Angular about your server component.

@NgModule({ //You tell the application that this is a module
  declarations: [
    AppComponent,
    ServerComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

Additional components are imported at the top of the file and then added in the declarations property of the module. Services are registered in the providers section.
Modules are added in the imports section and allow us to give extra functionality to our app. Angular has many built in ones, but if you were creating a large application you may want to separate your own components into modules.

*Component Templates*

You can use:
- templateUrl: which directs the file to an external html file.
- template: this allows you to write the html code inline in the typescript file. Use \`\` for multiple lines.

If you have more than 3 lines it is better to use a separate file. A template or templateUrl has to be present.

*Component Styles*

Similarly with styles you can use
- styleUrls which links to the style file.
- style which allows us to add the styles inline.

styleUrls is contained in an array so you are able to add multiple style sheets.

*Data Binding*

Databinding is communication between your business logic and your template.
There are a few ways this works:
- Output Data e.g String Interpolation (`{{ data }}`) and Property Binding )`[property]="data"`
- React to User events e.g Event Binding(`(event)="expression"`)
- Combination of both: Two-Way-Binding e.g (`[(ngModel)]="data"`)

*Sting Interpolation*

You may want to output dynamic fields. If they are properties of the component you can use string interpolation e.g
```
// In component file
serverId = 10;
serverStatus = 'Offline';

// In the template file
<p>Server with ID {{serverId}} is {{serverStatus}}

```
Whatever you put in, in the end it has to be returned as a string. You can't write multiline expressions in string interpolation.

*Property Binding*

There are a lot of times you can use either property binding or string interpolation.
Property binding allows us to bind to the native property of the html element directly e.g
`[disabled]="!allowNewServer"` binds the disabled property directly to the allowNewServer property of the servers component.
You can bind to html element properties, directives properties and your own components.

*Property binding vs string interpolation*

You should use string interpolation to output text, but if you want to change the property of a html element use property binding. String interpolation only works in a normal template, not in a normal template.

*Event Binding*

Event binding allows us to call a method in our component. The event binding in Angular uses parentheses e.g `<button (click)="doSomething()">Click me to do something</button>`. You can bind to any events made available by the html element you are attaching to (onClick, onMouseEnter etc).

How do you know to which Properties or Events of HTML Elements you may bind? You can basically bind to all Properties and Events - a good idea is to console.log()  the element you're interested in to see which properties and events it offers.

Important: For events, you don't bind to onclick but only to click (=> (click)).

The MDN (Mozilla Developer Network) offers nice lists of all properties and events of the element you're interested in. Googling for YOUR_ELEMENT properties  or YOUR_ELEMENT events  should yield nice results.

`$event` is a reserved variable name that you can pass from the template when using event biding. $event will be the data emitted by that event. We can capture the data input using `$event`.
```
onUpdateServerName(event: Event) {
    this.serverName = (<HTMLInputElement>event.target).value;
  }
```

*Two-Way-Binding*

Two-Way-Binding can be achieved by using a special syntax. `[(ngModel)]='serverName'` will change the name but will also pre populate an input or similar as the data binding is two-way. It is a very simple way of binding in both directions and reacting to changes in the input value.
N.B you need the forms module for Two-Way-Binding to work properly.

*Directives*
Directives are instructions in the DOM. Components are directives with a template. We typically add directives with the attribute selector.
```
<p appTurnGreen>Receives a green background</p>

@Directive {
  selector: '[appTurnGreen]'
}
export class TurnGreenDirective {
  //....
}
```

One popular in built directive is the ngIf directive `<p *ngIf="boolean"></p>`. ngIf is a structural directive as it changes the structure of the DOM.
ngIf also has an else condition. You can do this by placing a local reference on an element.
```
<p *ngIf="serverCreated; else noServer">Server was created. Server name is {{ serverName }}</p>
<ng-template #noServer>
    <p>No Server has been created</p>
</ng-template>
```

ngStyle is another directive. It is known as an attribute directive. It doesn't change the html, only the element they are placed on.
ngStyle allows us to dynamically update style.
`<p [ngStyle]="{'background-color': getColor()}">{{ 'Server' }} with id {{ serverId }} is {{ serverStatus }} </p>`

ngClass is a directive that lets you add css classes dynamically.
`[ngClass]="{online: serverStatus === 'Online'}">` the class is the key and the condition is the value.

ngFor allows us to output lists from enumerable objects (arrays, objects etc.)
`*ngFor="let server of servers"` is a simple for of loop that can be added to html elements in order to repeat them.
You can extract extra information using ngFor e.g `*ngFor="let server of servers; let i = index;"`
