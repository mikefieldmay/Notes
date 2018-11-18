**Angular Routing**
With Angular it's easy to have multiple pages on a single page application.
It still is only one page, but it exchanges large parts of the application. We achieve this with Angular routes.
We register routes in the app module.
```
const appRoutes: Routes =[
  { path: '', component: HomeComponent }, // when this path is reached we should load the home component
  { path: 'users', component: UsersComponent },
  { path: 'servers', component: ServersComponent },
  { path: 'users', component: UsersComponent }
];

...
imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    RouterModule.forRoot(appRoutes) // forRoot registers the routes we've registered in the application
  ],
...

// In our app.component.html template we choose where we want the component to be rendered With
<router-outlet></router-outlet>
```

Using the usual href attribute in an a element causes the page to reload. In Angular we use `routerlink` to stop this behaviour.
```
<ul class="nav nav-tabs">
  <li role="presentation" class="active"><a routerLink='/'>Home</a></li>
  <li role="presentation"><a routerLink='/servers'>Servers</a></li>
  <li role="presentation"><a [routerLink]="['/users']">Users</a></li>
</ul>
```

We need to use the absolute path `/`. With relative path it appends to the path you are currently on, absolute path takes us to the absolute path.

*Active Path*
We can add the `routerLinkActive="className"` to add classes to active links. This directive can be called on the link itself or the container. It applies the class to the currently active link.
```
<ul class="nav nav-tabs">
  <li role="presentation"
    [routerLinkActiveOptions]="{exact: true}" // this is an extra directive to be added to the / path. Only use         routerLink active if the exact path is /.
    routerLinkActive="active">
    <a routerLink='/'>Home</a></li>
  <li role="presentation" routerLinkActive="active"><a routerLink='/servers'>Servers</a></li>
  <li role="presentation" routerLinkActive="active"><a [routerLink]="['/users']">Users</a></li>
</ul>
```

*Navigating Programatically*

We can navigate from within our components too.
```
import { Component, OnInit } from '@angular/core';
import { Router } from "@angular/router";

@Component({
  ...
})
export class HomeComponent implements OnInit {

  constructor(private router: Router) { }

  ngOnInit() {
  }

  onLoadServers(){
    // complex calculation
    this.router.navigate(['/servers']);
  }

}

```

When using the relative route with the `this.router.navigate(['/servers'])` method it doesn't cause errors. This is because it is relative to the main route, not the currently activated route. It does not have the knowledge of which route is actived by default. You can change this behaviour with `relativeTo`.
```
import { Component, OnInit } from '@angular/core';
import { ServersService } from './servers.service';
import { Router, ActivatedRoute } from "@angular/router";

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  private servers: {id: number, name: string, status: string}[] = [];

  constructor(private serversService: ServersService,
              private router: Router,
              private route: ActivatedRoute) { } // ActivatedRoute knows which route is currently activated

  ngOnInit() {
    this.servers = this.serversService.getServers();
  }

  onReloadPage() {
    this.router.navigate(['servers'], {relativeTo: this.route}); // the path servers is added relative to the current route
  }

}

```

*Passing Parameters to routes*

We can add dynamic parameters to routes using `route/:nameOfParameter` e.g the path of an individual user:
`{ path: 'users/:id', component: UserComponent }`. These parameters can be accessed by importing `ActivatedRoute and calling` the method `this.route.snapshot.params['parameterName']`. Angular does not re-render components we're already on with routerLink. Because of this snapshot can cause issues after the first time the component is loaded. To get around this we can use `this.route.params` which is an Observable.
```
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Params } from '@angular/router';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css']
})
export class UserComponent implements OnInit {
  user: {id: number, name: string};

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.user = {
      id: this.route.snapshot.params['id'],
      name: this.route.snapshot.params['name']
    };
    this.route.params // params is an Observable to be subscribed too
      .subscribe(
        (params: Params) => {
          this.user.id = params['id'];
          this.user.name = params['name'];
          }
      );
  }
}
```
If you know that your component can not be loaded from within itself using snapshot is fine, but if it can, params should be used. Angular cleans up this subscription whenever this component is destroyed. The subscription is not destroyed.

*Passing Query Parameters and Fragments*

Query parameters are separated with a ?. To pass query params and fragments we need to pass them in the link:
```
[routerLink]="['/servers', 5, 'edit']" // link to /servers/5/edit
[queryParams]="{allowEdit: '1'}" // another bindable property of the routerLink directive which passes allowEdit as a query parameter.
fragment='loading' // passes the fragment loading at the end of the url.

// The end result is .../servers/5/edit/?allowEdit=1#loading
```
We can achieve the same thing from within our component (programatically) with
`this.router.navigate(['/servers', id, 'edit'], {queryParams: {allowEdit: 1}, fragment: 'loading'});`

We can access these values with the ActivatedRoute object.
```
console.log(this.route.snapshot.queryParams); // we can use snapshot to get the queryParams on the first page enter
    console.log(this.route.snapshot.fragment);
    this.route.queryParams.subscribe(); // we can subscribe to the queryParams observable to pick up on any changes
    this.route.fragment.subscribe();
```

*N.B* When we parse a parameter from our URL it will always be a string because of how URLs work. This can cause some problems with our methods. We can make sure the id we're retrieving is a number by adding a + to the beginning e.g
`const id = +this.route.snapshot.params['id'];`.

*Nested (Child) Routing*
Child routes are nested under their parent route. When nesting child routes we can omit the parent path:

```
const appRoutes: Routes =[
  { path: '', component: HomeComponent }, // when this path is reached we should load the home component
  { path: 'servers', component: ServersComponent, children: [
    { path: ':id', component: ServerComponent },
    { path: ':id/edit', component: EditServerComponent }
  ] },
  { path: 'users', component: UsersComponent },
  { path: 'users/:id/:name', component: UserComponent }
];
```
`<router-outlet>` at the app.component.html is reserved for all our top level route components. Child routes need a new `<router-outlet>` in their parent component.
This allows us to render child components in the parent component.

*Configuring the handling of query parameters*

We can persist query params by adding the queryParamsHandling method with preserve as the value.
`this.router.navigate(['edit'], {relativeTo: this.route, queryParamsHandling: 'preserve'});`
We can merge any new queryParams with merge as the value.
`this.router.navigate(['edit'], {relativeTo: this.route, queryParamsHandling: 'merge'});`
`preserve` will overwrite any new query params, so if you want to persist and add, use merge.

*Redirecting and Wildcard Routes*

We can add 404 error handling and redirects.
`{ path: '**', redirectTo: '/not-found'}` The `**` is a wildcard that handles any routes. `redirectTo` redirects to a different established path. The wildcard route will catch all routes so it needs to declared at the end of the routes.

'In our example, we didn't encounter any issues when we tried to redirect the user. But that's not always the case when adding redirections.

By default, Angular matches paths by prefix. That means, that the following route will match both /recipes  and just /

{ path: '', redirectTo: '/somewhere-else' }

Actually, Angular will give you an error here, because that's a common gotcha: This route will now ALWAYS redirect you! Why?

Since the default matching strategy is "prefix" , Angular checks if the path you entered in the URL does start with the path specified in the route. Of course every path starts with ''  (Important: That's no whitespace, it's simply "nothing").

To fix this behavior, you need to change the matching strategy to "full" :

{ path: '', redirectTo: '/somewhere-else', pathMatch: 'full' }

Now, you only get redirected, if the full path is ''  (so only if you got NO other content in your path in this example).'

*Outsourcing app-routes*

Typically if you have more than 3 routes you create your own app-routing module.
```
const appRoutes: Routes =[
  { path: '', component: HomeComponent },
  { path: 'servers', component: ServersComponent, children: [
    { path: ':id', component: ServerComponent },
    { path: ':id/edit', component: EditServerComponent }
  ] },
  { path: 'users', component: UsersComponent, children: [
    { path: ':id/:name', component: UserComponent }
  ] },
  { path: 'not-found', component: PageNotFoundComponent},
  { path: '**', redirectTo: '/not-found'}

];
@NgModule({
  imports: [ // we do not use declarations as these are declared in app.module
    RouterModule.forRoot(appRoutes)
  ],
  exports: [RouterModule] // we need to export the RouterModule
})
export class AppRoutingModule {}

This is added with an import to the app.module file

...
imports: [
    BrowserModule,
    FormsModule,
    HttpModule,
    AppRoutingModule
  ],
  ..
```

*Guards*

We can add guards to routes. A guard is logic that is activated before a route is activated, or when a route is being left.
We can protect routes using canActivate.
Below is an example of an authorisation service, an authorisation guard, and how it implemented in the routes.

```
// Auth Service
export class AuthService {
  loggedIn = false;

  isAuthenticated(): Promise<any> {
    const promise = new Promise(
      (resolve, reject) => {
        setTimeout(() => {
          resolve(this.loggedIn);
        }, 800);
      }
    );
    return promise; // returns a promise that resolves in 800ms to simulate a database call.
  }

  login() {
    this.loggedIn = true;
  }

  logout() {
    this.loggedIn = false;
  }
}

// Auth Guard

import {Router, ActivatedRouteSnapshot,  RouterStateSnapshot,   CanActivate} from '@angular/router';
import { Observable } from 'rxjs/Observable';
import { Injectable } from '@angular/core';
import { AuthService } from 'app/auth.service';


@Injectable()
export class AuthGuard implements CanActivate {

  constructor(private authService: AuthService, private router: Router) {}

  canActivate(route: ActivatedRouteSnapshot,
              state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
                return this.authService.isAuthenticated()
                  .then(
                    (authenticated: boolean) => {
                      if (authenticated) {
                        return true; // if the authentication passes  then proceed.
                      } else {
                        this.router.navigate(['/']); // if the authentication fails then re-route to home
                       }
                    }
                  );
  }

  // can activate can either return: An Observable<boolean> that wraps a boolean value.
  // A Promise Promise<boolean> or just a boolean. It can return things asynchronously
  // or synchronously. It depends if it's running something on the client of from the server.
}

//In the routing module:

{ path: 'servers', canActivate: [AuthGuard], component: ServersComponent, children: [ // these conditions are passed to all the child components
   { path: ':id', component: ServerComponent },
   { path: ':id/edit', component: EditServerComponent }
 ] },

```

We can protect all the child routes of a route using canActivateChild instead of can activate.

```
...
{ path: 'servers',
    // canActivate: [AuthGuard],
    canActivateChild: [AuthGuard], // this stops any access to the child routes without authorisation, but allows us to access the parent route
    component: ServersComponent,
    children: [
    { path: ':id', component: ServerComponent },
    { path: ':id/edit', component: EditServerComponent }
  ] },
  ...
  ```

*Controlling routing with CanDeactivate*
We can use CanDeactivate to keep the user from accidentally clicking away from the page.
We need to create a can-deactivate-guard service service

```
import {ActivatedRouteSnapshot, RouterStateSnapshot,  CanDeactivate} from '@angular/router';
import { Observable } from "rxjs/Observable";

export interface CanComponentDeactivate { // for this canDeactivate to work the component needs to have a method called canDeactivate that adheres to this blue print
  canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;
}

export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
  // this is the method called by the router

  canDeactivate(component: CanComponentDeactivate, // this component needs to have the CanDeactivate method
      currentRoute: ActivatedRouteSnapshot, // this is to know the current route
      currentState: RouterStateSnapshot, // it'll know the current state
      nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
    return component.canDeactivate(); // we need to return the canDeactivate method on the component we're currently on
                                      // the router can rely on the fact that the component can use the above method
  }
  // next state is optional and knows where to go next
}

// We implement this as so:

canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
    if (!this.allowEdit) { return true; } // if you're not allowed to edit you can leave
    if (this.serverName !== this.server.name) { //
      return confirm('Do you want to discard the changes?');
    } else {
      console.log("I'm being called in the else")

      return true;
    }

  }

```

*Passing data through routes*

We can pass static data through routes. In the below example we pass an error message to an error component.
```
// In the route file:

{ path: 'not-found', component: ErrorPageComponent, data: {message: 'Page not found!'}}, data can have as many properties as you like

// To access the data in the component

ngOnInit() {
    this.errorMessage = this.activeRoute.snapshot.data['message']; // usual use case
    this.activeRoute.data.subscribe(
      (data: Data) => {
        this.errorMessage = data['message'];
      }
    ); // if the data may change
  }
```
We can also pass dynamic data with the resolve guard.
The resolve guard can fetch data from the server. It allows us to run the code before the page is rendered. The resolver will always render the component.

```
// This resolver loads the server data before the server is initialised

import { ServersService } from '../servers.service';
import { ActivatedRouteSnapshot, RouterStateSnapshot,  Resolve } from '@angular/router';
import { Observable } from 'rxjs/Observable';
import { Injectable } from '@angular/core';

interface Server {
  id: number;
  name: string;
 status: string;
} // with the interface code we are essentially creating a model within this file of what the server will look like

@Injectable()
export class ServerResolver implements Resolve<Server> {

  constructor(private serversService: ServersService) {}

  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<Server> | Promise<Server> | Server {
    return this.serversService.getServer(+route.params['id']);
  }
}

// It is passed into the routes like so:

{ path: ':id', component: ServerComponent, resolve: {server: ServerResolver} },
// the outputted data from this resolver will be stored in the server property

// We can access this data in the components OnInit method like so:

this.route.data
  .subscribe(
    (data: Data) => {
      this.server = data['server']; // this is the property we defined in the resolve of the routes
    }
  );

```

*Location strategies*

If you have a route `yourDomain/servers` this may not work on the web as the route is always parsed first to the server.
The server hosting the app has to be configured as such that in case of an error it has to return the html page containing your app. If you have `/servers` it will look for that route on your server. By returning the index we will route to the proper path.
Older browsers are unable to parse paths in the client. In order to get around this we can enable using  `#` in our routes.
In the `RouterModule.forRoutes(appRoutes, {useHash: true})`. This is disabled by default.
It informs the webserver, only care about the part in front of the hash, not afterwards (an example of fragmenting). The part after the hashtag can be parsed by the app.
Using the prettier version should be what to aim for as the routes look better. it is known as HTML history routes.
