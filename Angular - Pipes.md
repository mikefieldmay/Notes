**Pipes**

Pipes are a feature built into Angular that allow you to transform output within your template.
```
username = 'Mike'

<p>{{ username }}</p>

Mike

// If you decide you want to change a way you display the username, but not change the username itself then you can use pipes

<p>{{ username | uppercase }}</p>

MIKE

```

You can create pipes using the cli with the command `ng generate pipe pipeName`
There are inbuilt pipes as well, but you can also build your own pipes.
Because pipes transform the output they are used in the HTML template.

Two common pipes are date and uppercase.

*Parameterizing pipes*

We can configure many built in pipes. This is done by adding a colon to the pipe.
```
{{ server.started | date }}

Aug 9, 1920

{{ server.started | date:'fullDate' }}

Monday, August 9, 1920
```
Angular's API reference has more about inbuilt pipes: https://angular.io/api?type=pipe

*Chanining Pipes*

You can chain Pipes by passing a pipe after a pipe.
```
{{ server.started | date:'fullDate' }}

Monday, August 9, 1920

{{ server.started | date:'fullDate' | uppercase }}

MONDAY, AUGUST 9, 1920
```
The order is incredibly important. You should apply the pipes in the order you want to transform the input.

*Custom Pipes*

```
import { PipeTransform } from "@angular/core"; // We should import the PipeTransform interface from Angular Core

@Pipe({
  name: 'shorten'
}) // the pipe decorator allows us to give our pipe a name that can be referenced in the HTML

export class ShortenPipe implements PipeTransform {
  transform(value: any) { // transform takes the value which is the string being passed to the pipe
    return value.substr(0, 10) // substr is a javascript method
  }

}
```
Pipes must be added in the App.module declarations

*Parameterizing a custom pipe*

We can also parameterize our custom pipe.
```
export class ShortenPipe implements PipeTransform {
  transform(value: any, limit: number) { // by adding an extra argument in the constructor we can pass parameters to pipes
    if (value.length > limit) {
      return value.substr(0, limit) + ' ...';
    } else {
      return value;
    }
  }

  // In the HTML
  <strong>{{ server.name | shorten:10}}</strong>
```

*Advanced pipes*

We can use pipes to filter results such as search results.
```
// Filter Pipe
import { Pipe, PipeTransform } from '@angular/core';

import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'filter'
})
export class FilterPipe implements PipeTransform {

  transform(value: any, filterString: string, propName: string): any { we filter by the filterstring for the property name.
    if (value.length === 0 || filterString === '') {
      return value;
    }
    const resultArray = [];
    for (const item of value) {
      if (item[propName] === filterString) {
        resultArray.push(item);
      }
    }
    return resultArray;
  }

}

// In the HTML

<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <input type="text" [(ngModel)]="filteredStatus"> // we give filteredStatus a value here. filteredStatus is initially an empty string in the component
      <hr>
      <ul class="list-group">
        <li
          class="list-group-item"
          *ngFor="let server of servers | filter:filteredStatus:'status'" // we can pass the filter directly into ngFor
          [ngClass]="getStatusClasses(server)">
          <span
            class="badge">
            {{ server.status }}
          </span>
          <strong>{{ server.name | shorten }}</strong> |
          {{ server.instanceType | uppercase }} |
          {{ server.started | date:'fullDate' | uppercase }}
        </li>
      </ul>
    </div>
  </div>
</div>
```

*Pure and Impure Pipes*

Angular does not rerun the pipe on the data whenever the data changes. Changing the input of the pipe will update the data, but changing the data will not. This is to stop Angular running the pipe whenever the data changes which would cost a lot of performance and is the reason why no filter pipe exists. We can force the pipe to update whenever the data changes by adding a second property to the `@Pipe` decorator.
```
@Pipe({
  name: 'filter',
  pure: false
})
```

*Understanding the Async pipe*

If you are interpolating data that you receive asynchronously, Angular will display it as an object, rather than the value you're returning. We can get around this using the async pipe.
```
appStatus = new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('stable');
     }, 2000);
  });

  <h2>App Status: {{ appStatus | async}}</h2> // async recognizes this is a  promise or an observable and will display the result after the data is retrieved/promise is resolved.
```
