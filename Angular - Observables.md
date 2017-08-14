**Using Observables with Angular**

An observable is a data source (e.g User Input events, Http requests, Triggered in Code etc.). It emits data and can be watched by an observer.
The observer has 3 ways of handling the data: Handling Data, Handling Error and Handle Completion. An Observable doesn't have to complete. HTTP requests have a clear end.
We use Observables to handle asynchronous events.
Once set up, an observable might emit a normal data package, an error, or it may complete.

```
// A component subscribed to the params Observable

import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Params } from '@angular/router';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css']
})
export class UserComponent implements OnInit {
  id: number;

  constructor(private route: ActivatedRoute) { }

  ngOnInit() {
    this.route.params
      .subscribe(
        (params: Params) => {
          this.id = +params['id'];
        }
      );
  } // It has subscribed to the params observable. It will not re-render every time the params change, but it will be able to react to the changes

}

```

*Creating observables*

You can create Observables by importing Observable and rxjx/Rx into your file. Observable has many in built methods that help us to create observables.
```
import { Observable } from 'rxjs/Observable';
import 'rxjs/Rx';

ngOnInit() {
   const myNumbers = Observable.interval(1000); // increases a count at an interval of 1 second
   myNumbers.subscribe(
     (number: number) => {
       console.log(number);
     }
   );
 }
```

Complex Observable:
```
const myObservable = Observable.create((observer: Observer<string>) => {
     // .create takes a function as an argument. It takes an Observer type. We are telling the observer when it will receive which data.
     setTimeout(() => {
       observer.next('first package'); // next pushes the next data package
     }, 2000);
     setTimeout(() => {
       observer.next('second package'); //
     }, 4000);
     setTimeout(() => {
       observer.error('this does not work'); // error passes an error message
       observer.complete(); // after complete is called, no more data can be emitted
     }, 5000);
   });

   myObservable.subscribe(
     (data: string) => { console.log(data); },
     (error: string) => { console.log(error); },
     () => { console.log('completed'); }
   );
 }
```

*Unsubscribing*

In the below example, even when we navigate away from the component and the component is destroyed, we are still subscribed and as such the count continues to increase. This is known as a memory leak. You should always unsubscribe when you leave an area.
```
ngOnInit() {
   const myNumbers = Observable.interval(1000); // increases a count at an interval of 1 second
   myNumbers.subscribe(
     (number: number) => {
       console.log(number);
     }
   );
 }
```

The way to stop this from happening is with unsubscribe. In order to do this, we need to store our subscription as a property of the component.
```
numbersObsSubscription: Subscription; // it is of type subscription.


constructor() { }

ngOnInit() {
  const myNumbers = Observable.interval(1000);
  this.numbersObsSubscription = myNumbers.subscribe(
    (number: number) => {
      console.log(number);
    }
  );
```

We also need to implement the ngOnDestroy lifecycle hook and call the unsubscribe method on our subscription.
```
ngOnDestroy(){
    this.numbersObsSubscription.unsubscribe();
  }
```

The observables created by Angular clean themselves up automatically.

http://reactivex.io/rxjs/ has more information on Observables.

*Using Subjects to pass and listen to data*

A Subject is like an Observable but it allows you to push it to emit new data during your code. It is an Observable, and an Observer at the same time. It is good practice to use a subject instead of an event emitter.
```
// Subject created in the service

import { Injectable } from '@angular/core';
import { Subject } from "rxjs/Subject";

@Injectable()
export class UsersService {
  userActivated = new Subject();

  constructor() { }

}

// Subject can be treated as an observable when we want to send data:

onActivate() {
  console.log('onActivate called');

  this.usersService.userActivated.next(this.id); // we are pushing a new data package containing this.id to the observable.
}

// Subject can be treated as an Observer when when we want it to receive data.

ngOnInit() {
   this.usersService.userActivated
     .subscribe(
       (id: number) => {
         if (id === 1) {
           this.user1Activated = true;
         } else if (id === 2) {
           this.user2Activated = true;
         }
       }
     );
 }
```

Subjects should be unsubscribed from.

*Observable Operators*

With rxjs we can use operators to transform the data we receive to something else from within the observable. We can chain Operators to any observable. One example is .map.

```
const myNumbers = Observable.interval(1000)
      .map( // .map is an example of an operator. It allow's us to transform our data. rxjs turns this into a new observable.
        (data: number) => {
          return data * 2;
        }
      );

```

 
