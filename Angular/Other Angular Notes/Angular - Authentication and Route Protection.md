**Authentication**

In a traditional web application, the client sends the authorization information and server stores the session. It sets a session cookie and the identify is checked via the cookie.
Authentication works slightly different in a single page application. The server can check authentication, but it won't store a session. Instead the server sends back a json web token. It will also be hashed with an algorithm that only the server knows. The server does not remember the client.
Javascript web token info: https://jwt.io/introduction/

*Fire Base*

Google provides a convenient backend known as FireBase. You can set up a database by visiting:
https://firebase.google.com/.
There are 2 main ways in which you can interact with your firebase database. You can send http requests directly to the firebase URL with the usual requests e.g (get, put, delete etc), or you can use the firebase SDK in your project.
To install the SDK you should run `npm install firebase --save` in the command line.
*N.B I had issues with the last installation and had to run `npm install promise-polyfill --save-exact`*. Doing this gives us access to many useful firebase methods.
In the firebase console under authentication we can choose the type of authentication we want (usually with email and password). We can also see the `Web Setup` which gives us the info we need to add to our projects app.component:
```
// In App.component
import { Component, OnInit } from '@angular/core';
import * as firebase from 'firebase';
import { config } from "app/config";

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  loadedFeature = 'recipe';

  ngOnInit() {
    firebase.initializeApp({ // this command initializes firebase for the project.
      apiKey: config.apiKey,
      authDomain: config.authDomain
    });
  }

  onNavigate(feature: string) {
    this.loadedFeature = feature;
  }


}
```

We should then create our own AuthService that handle all the firebase database calls:
```
import {Response} from '@angular/http';
import * as firebase from 'firebase';

export class AuthService {
  signUpUser(email: string, password: string) {
    console.log('This is working');

    firebase.auth().createUserWithEmailAndPassword(email, password) // method name is pretty self explanatory
      .catch(
        error => console.log(error)
      );
  }

  signInUser(email: string, password: string) {
    firebase.auth().signInWithEmailAndPassword(email, password)
      .then(
        (response: Response) => console.log(response)
      )
      .catch(
        (error) => console.log(error)
      );
  }
}

```
Ordinarily we would have to save the web token, however when we use the firebase sdk it automatically stores the token on the browser.

We can retrieve this token in our application.

```

export class AuthService {
  token = '';

  signInUser(email: string, password: string) {
    firebase.auth().signInWithEmailAndPassword(email, password)
      .then(
        (response: Response) => {
          return firebase.auth().currentUser.getToken() // in our initial sign in we can call the getToken method. This returns a promise which we can then use to set the token value.
            .then(
              (token: string) => this.token = token
            );
        }
      )
      .catch(
        (error) => console.log(error)
      );
  }

  getToken() {
    firebase.auth().currentUser.getToken() // get Token performs another call to the database this may have not resolved by the time we return this new token, but we could ask the user to try again through an error message if it isn't working.
      .then(
          (token: string) => this.token = token
        );
    return this.token;
  }
}

// We append this token to the database calls.

storeRecipes() {
   const token = this.authService.getToken();
   return this.http.put(config.DB_URL + '?auth=' + token, this.recipeService.getRecipes());
 }

```

*Route Protection*

We can use `*ngIf` to display certain buttons based on whether we're logged in or not.
We can also add routeProtection using a route protector guard.
```
import {RouterStateSnapshot, ActivatedRouteSnapshot,  CanActivate} from '@angular/router';
import { AuthService } from "app/auth/auth.service";
import { Injectable } from "@angular/core";


@Injectable()
export class AuthGuard implements CanActivate {

  constructor(private authService: AuthService) {
  }
  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot) {
    return this.authService.isAuthenticated();
  }
}

// In the routing module

const AppRoutes: Routes = [
  { path: '', redirectTo: '/recipes', pathMatch: 'full' },
  { path: 'recipes', component: RecipesComponent, children: [
    { path: '', component: RecipeStartComponent},
    { path: 'new', component: RecipeEditComponent, canActivate: [AuthGuard]},
    { path: ':id', component: RecipeDetailComponent},
    { path: ':id/edit', component: RecipeEditComponent, canActivate: [AuthGuard]}
  ] },
  { path: 'shopping-list', component: ShoppingListComponent},
  { path: 'signup', component: SignupComponent},
  { path: 'signin', component: SigninComponent}
];
```
