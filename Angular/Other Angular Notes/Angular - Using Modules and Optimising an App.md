## Using Modules and Optimising an app

# The Idea behind app Modules

In general all the elements that make up an angular app have to registered in modules. Angular wants you to be explicit about what's being used. It is good because you are clear about what you need and what you don't need.
We can use multiple modules in an application.

The app module from the recipe app has a large `app.module` file.
Imports are all typescript related.

- Declarations: We define which components, directives or pipes this module uses
- Imports: Which other modules does this module use (usually other built in Ang modules). When we import a module, we import everything that that module exports. Modules bundle certain functionalities that we can import.
- Providers: This lets us declare what we want to provide for the whole application.
- Bootstrap: Points us at our root component.

# Feature Modules

A typical module you may add to an app would be a feature module. There may be components and directives that are only relevant to a feature of the app. We can separate these out into a feature module instead.
All modules getting loaded at app launch get merged into one injector.
```
import {DropdownDirective} from '../shared/dropdown.directive';
import {RecipeEditComponent} from './recipe-edit/recipe-edit.component';
import {RecipeListComponent} from './recipe-list/recipe-list.component';
import {RecipeDetailComponent} from './recipe-detail/recipe-detail.component';
import {RecipeStartComponent} from './recipe-start/recipe-start.component';
import {RecipesComponent} from './recipes.component';
import { NgModule } from '@angular/core';
import { ReactiveFormsModule } from '@angular/forms';
import { CommonModule } from '@angular/common';

@NgModule({
  declarations: [
    RecipesComponent,
    RecipeDetailComponent,
    RecipeStartComponent,
    RecipeListComponent,
    RecipeEditComponent,
    DropdownDirective
  ],
  imports: [
    CommonModule, // we add the common module to give us access to the common directives like ngClass, ngFor etc.
    ReactiveFormsModule ]
})
export class RecipesModule {}
```

In our main module we don't use CommonModule we use the BrowserModule. BrowserModule contains most of the features of common module, as well as features that are only needed when the app starts.

# Registering routes in a feature module.

The router outlet is only registered in the app module. If we create a feature module we need to move the routes to that module.
When creating the new routing module we need to set it up slightly differently.

```
@NgModule({
  imports: [
    RouterModule.forChild(recipeRoutes) // we use for child rather than forRoot as we are.
  ],
  exports: [ RouterModule ]
})
```

You must only use `forRoot` for the app module. Any other module is a child module so should use `forChild`.

# Directives in modules

You can only add a directive to one module. This creates problems when you're trying to use directives in multiple features of your app. We can get around this by creating a shared module that houses the directives and shares them between your app features.

```
import {DropdownDirective} from './dropdown.directive';
import { NgModule } from "@angular/core";
import { CommonModule } from "@angular/common";

@NgModule({
  declarations: [
    DropdownDirective
  ],
  exports: [
    CommonModule,
    DropdownDirective
  ]
})
export class SharedModule {}

```
We export the common module, but this is overwritten by the browser module in the app.module.

# Loading components via Selectors vs Routers

For routing it's not important that you declare the routes for a module in specifically in that module. It's only important that it's declared before the route to that module is referenced in the code. For a selector it needs to be included in the module that you plan to use it in. In routing, it doesn't matter so much but the component you try to load needs to be declared somewhere.

# Lazy Loading

It is good practice to restructure your app as it gets bigger, but it doesn't increase performance.
Lazy loading increases performance. A lot of the code of a website might never be used. We can lazily load our feature module and our child routers. This means it is only loaded when the routes are accessed.
To load a module lazily it needs to be a feature module. If it is loaded in the app.module, it is loaded `eagerly`. We need to remove it from the `app.module`. This means it isn't bundled when the page is loaded. We add lazy loading in the app routing file
```
...
const AppRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: '/recipes', loadChildren: './recipes/recipe.module#RecipesModule' }, // We use load children with the route to the module and then a # with the class name. This allows recipe only to be loaded when it's accessed
  { path: 'shopping-list', component: ShoppingListComponent}
];
...

In the routing for recipes:

const recipeRoutes: Routes = [
  { path: '', component: RecipesComponent, children: [ we delete the 'recipes' route as this module is loaded from the routes
    { path: '', component: RecipeStartComponent},
    { path: 'new', component: RecipeEditComponent, canActivate: [AuthGuard]},
    { path: ':id', component: RecipeDetailComponent},
    { path: ':id/edit', component: RecipeEditComponent, canActivate: [AuthGuard]}
  ] }
];

```

What if you want to use route protection (canActivate  to be precise) on lazily loaded routes?

You can add canActivate to the lazy loaded routes but that of course means, that you might load code which in the end can't get accessed anyways. It would be better to check that BEFORE loading the code.

You can enforce this behaviour by adding the canLoad  guard to the route which points to the lazily loaded module:

{ path: 'recipes', loadChildren: './recipes/recipes.module#RecipesModule', canLoad: [AuthGuard] }

In this example, the AuthGuard  should implement the CanLoad interface.

# Modules and service injection

If you provide a service to app module and feature module, only one root injector is created and the whole app only uses one services. This also applies to the lazy loaded module. If we provide a service to the lazy loaded module it creates a new instance of the service for all the children in that module. Whether you load a module lazily or eagerly can affect the amount of services created. If you want to enforce module scope you need to declare the service in the main component.
If we have a shared module with a service provided that is shared between an eager module and a lazy module, the lazy module will create a new instance of that service when it is loaded.

You should never add a service to a shared module.

# Understanding the core module

Things that only belong to the core or root of the module should be put in a core module. We should put in everything we load when we initially visit a website to the core module.
```
...
@NgModule({
  declarations: [
    HeaderComponent,
    HomeComponent
  ],
  imports: [
    SharedModule,
    AppRoutingModule
  ],
  exports: [
    AppRoutingModule,
    HeaderComponent
  ],
  providers: [
    ShoppingListService,
    RecipeService,
    DataStorageService,
    AuthService
  ] // All the app wide services can be added to the provider array in the core module
})
export class CoreModule {}
```

We should pull out Guards or providers that are only used in certain features of the app.

# Ahead of time compilation

Angular offers 2 options when compiling the code. Angular needs to compile your templates. It parses your html code into javascript. It does this because accessing javascript is faster than accessing the DOM. It can do this compilation in two different places. Just in time is the default. We develop the code, then we ship it with `ng serve` and then we view it in the browser. Once it's downloaded Angular bootstraps the application. This is the default approach.
We can change this by using ahead of time compilation. We allow Angular to compile the templates. It allows Angular to understand our templates at an earlier point of time. We ship it to production and then we ship it to the browser.

AoT advantages
- Application can start faster because compilation doesn't happen in the browser
- Our templates are checked during development
- Much smaller file size as unused features can be stripped out and compiler itself isn't shipped.

In order to build our project we should use the command: `ng build --prod --aot`

AoT speeds up the start up time of the application.

# Preloading lazy loaded routes.

Lazy loading and AoT are the basic optimisations that should be used in most apps. It would be good if we used lazy loaded code, but the code itself is pre-loaded incase the user wants to visit these features. Angular makes this easy.

```
// In the app-routing module

@NgModule({
  imports: [
    RouterModule.forRoot(AppRoutes, {preloadingStrategy: PreloadAllModules}) // add an extra argument to forRoot
  ],
  exports: [RouterModule]
})
```

## Callbacks

```
In the HTML:

<div
style="width: 100px; height: 100px"
[@divState]="state"
(@divState.start)="animationStarted($event)"
(@divState.done)="animationFinished($event)"></div>

This will call the above methods declared in the component. 
```
