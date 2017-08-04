**Angular**

Angular is a JavaScript Framework that allows you to create reactive Single-Page-Applications (SPA's). This means that everything that changes is rendered in the browser. This makes the site much quicker when making page changes and calls to databases.
Angular is a JS framework changing the DOM at runtime.

Why use the CLI?
- There are some extra steps such which the cli helps with.


*CLI Commands*
```
ng start name-of-project //will create the project
ng serve // will start up the project server and compiler the files
ng serve --port YOURPORT // will change the port number
```
All the code is written in the App folder. The assets folder stores images/data and any environmental variables are stored in environments.
When creating a new file it's a good idea to install bootstrap aswell.
`npm install --save bootstrap`
We then need to import bootstrap into the angular-cli.json file. The cli uses webpack bundles the files and adds them to the index.html file. Add `../node_modules/bootstrap/dist/css/bootstrap.min.css`

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

`thing.component.ts` is the naming convention.
A component is just a typescript class that is exported with the `export` keyword. The majority of items come from `@angular/core`, but there are other folders to import from.

*App module*
To use a component we need to add it to the app module. Angular uses components to build webpages and modules to bundle individual pieces into packages. A module is a bundle of functionalities of our app which gives Angular the information about the features of which the app uses.
```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
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
