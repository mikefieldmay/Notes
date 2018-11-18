# NgRx

When using larger applications or you want to take a more elaborate approach you can use NgRx. It is an additional package you can use with Angular in order to manage state.

## What is application state?

Our application is managed entirely through an applications state. This state is lost during a refresh (but it can be stored in local storage). We also have a backend that is a persistent state as it's still there when the application is restarted. State keeps track of what the user sees and does. Some states are saved and some are lost.
When we move to larger applications we can have many different areas querying the state of the application.It can be hard to keep track of where state gets changed an queried. We want to write an application where there aren't that many places that state can change.
In the Recipe book we make sure we only change the recipes state in the recipes service. Using a service as a central place is a good practice, as well as using subjects to update the state.

## Redux
In a redux application you have a central store that holds the application state. You still have components and services that receive the state from the store, but can also talk between each other. In order to change the state we dispatch actions. Actions are pre set. They are sent to a reducer. A reducer reduces/combines the state and then overwrites (not edits) the precious state.
This is the redux setup. Actions, Reducers and Store. We can use these in an Angular app. NgRx is a library that adapts the redux approach to fit nicely into an Angular app.

## Getting started with Reducers

To start with install Ngrx:
`npm install --save @ngrx/store`

We then create the reducer file and the actions file.

```
shopping-list.reducers.ts

import { Ingredient } from '../../shared/ingredient.model';
import * as ShoppingListActions from './shopping-list.actions'; // we import everything exported from shopping-list actions
import { Action } from '@ngrx/store';

const initialState = {
  ingredients: [
    new Ingredient('Carrot', 3),
    new Ingredient('Pumpkin', 1),
    new Ingredient('Squash', 2),
    new Ingredient('Cucumber', 10)
  ]
} // no state exists at the beginning so an initial state needs to be declared

export function shoppingListReducer(state = initialState, action: ShoppingListActions.ShoppingListActions) {
  switch (action.type) {
    case ShoppingListActions.ADD_INGREDIENT:
    return {
      ...state, // to create the new state we spread the old state to create a new object
      ingredients: [...state.ingredients, action.payload] // we then overwrite the ingredients with the spread previous ingredients and the new ingredient
    }
    default:
      return state //we need to return the new state of our application
  }
} // a reducer function will be triggered whenever an action is dispatched.
// it takes state which is the current state of the application

shopping-list.actions.ts

import {Ingredient} from '../../shared/ingredient.model';
import { Action } from "@ngrx/store";

export const ADD_INGREDIENT = 'ADD_INGREDIENT';

export class AddIngredient implements Action {
  readonly type = ADD_INGREDIENT; // When we create an action (that implements action) and it needs a readonly type
  payload: Ingredient;
}

export type ShoppingListActions = AddIngredient; // typescript has a property to export type
```

We register a store module with our reducers in the app module:
```
app.module.ts

imports: [
    AuthModule,
    BrowserModule,
    HttpModule,
    AppRoutingModule,
    SharedModule,
    ShoppingListModule,
    StoreModule.forRoot({
      shoppingList: shoppingListReducer
    }) // this is for eagerly loaded modules
  ],
```

We then need to change the way our component interacts with the store.
```
shopping-list.component.ts

export class ShoppingListComponent implements OnInit, OnDestroy {
  shoppingListState: Observable<{ingredients: Ingredient[]}>; // this.ingredients is a
  private subscription: Subscription;

  constructor(private shoppingListService: ShoppingListService, private store: Store<{shoppingList: {ingredients: Ingredient[]}}>) { }
// we inject the store in the constructor. The store this component is interested in is the ingredient store
  ngOnInit() {
    this.shoppingListState = this.store.select('shoppingList'); // store.select returns an observable
  }

shopping-list.component.html

<div class="row">
  <div class="col-xs-10">
    <app-shopping-edit></app-shopping-edit>
    <hr>
    <ul class="list-group">
      <a
        class="list-group-item"
        style="cursor:pointer"
        *ngFor="let ingredient of (shoppingListState | async).ingredients; let i = index"
        (click)="onEditItem(i)">
        {{ingredient.name}}: {{ingredient.amount}}
      </a>
    </ul>
  </div>
</div>
```

Below is an example of how you might interact with the store, and also dispatch actions in the components
```
ngOnInit() {
    this.subscription = this.store.select('shoppingList')
      .subscribe(
        data => {
          if (data.editedIngredientIndex > -1) {
            this.editedItem = data.editedIngredient;
            console.log(this.editedItem)
            this.editMode = true;
            this.slForm.setValue({
              'name': this.editedItem.name,
              'amount': this.editedItem.amount
            });
          } else {
            this.editMode = false;
          }
        }
      ); // currently Ngrx does not manage subscriptions
  }

  ngOnDestroy() {
    this.store.dispatch(new ShoppingListActions.StopEdit());
    this.subscription.unsubscribe();
  }

  onAddItem(form: NgForm){
    const value = form.value;
    const newIngredient = new Ingredient(value.name, value.amount);
    if (this.editMode) {
      this.store.dispatch(new ShoppingListActions.UpdateIngredient(newIngredient));
    } else {
      this.store.dispatch(new ShoppingListActions.AddIngredient(newIngredient)); // we use dispatch to dispatch an action
    }
    this.onClear();
  }

  onDelete() {
    this.store.dispatch(new ShoppingListActions.DeleteIngredient());
    this.onClear();
  }

  onClear() {
    this.editMode = false;
    this.slForm.reset();
  }

}
```

## Effects
Asynchronous operations can't be handled within reducers. Reducers should take a state as an input, output a new state and should work synchronously.

A side note on State. State is relative to a certain feature of your application. When referring to the overall state of the application you store everything in `AppState`. You rarely access an entire apps state. When setting up the typing on injection of store, we reference the apps state. `AppState` is the global application state.

A side effect is something that needs to change the state, but that is done asynchronously. Ngrx lets us handle these side effects using `@ngrx/effects`.
It makes handling side effects a lot easier.
Effects are a lot like reducers, but we don't change our app state at any point.

## Router State

Changes to the routing is arguably a state change.
We can track router state with the package `@ngrx/router` .
The install command is `npm install --save @ngrx/router`.

```
imports: [
    AuthModule,
    BrowserModule,
    HttpModule,
    AppRoutingModule,
    SharedModule,
    ShoppingListModule,
    StoreModule.forRoot(reducers), // this is for eagerly loaded modules
    EffectsModule.forRoot([AuthEffects]),
    StoreRouterConnectingModule // we simply add it to the imports in `AppModule`.    
  ],
```

## Store Devtools
We can use store devtools to give us a view into the apps state when an app loads.
You need to install the redux devtools as a chrome extension.
`npm install --save @ngrx/store-devtools`

## Lazy loading State into an applications
If we are lazy loading a module, we are unable to add it's state to the app's state at the time the app is bootstrapped. We are able to get around that though.  
