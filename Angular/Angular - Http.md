**Http requests**

We can use Angular to make Http requests. Angular uses ajax requests and makes this process simple using the Http service.
It is a good idea to create our own service to communicate with servers.
```
// ServersService
import {Http} from '@angular/http';
import { Injectable } from "@angular/core";

@Injectable()
export class ServerService {

  constructor(private http: Http) {}

  storeServers(servers: any[]) {
    return this.http.post('https://udemy-ng-9bcd1.firebaseio.com/data.json', servers); // The first argument is the url and the second is the data
    // this method creates an observable, but does not send it yet.
    // This is because if there is nothing subscribing to it, there is no point in sending the request.
  }

}

// In the component

constructor(private serverService: ServerService) {}

 onSave() {
   this.serverService.storeServers(this.servers)
     .subscribe(
       (response) => console.log(response),
       (error) => console.log(error)
     ); // Because we have subscribed the data will be sent.
 }

```
The Http service has many built in methods that mirror the usual http requests e.g post, get, put etc.

*Adjusting request Headers*
You can also add request headers to a request.
```
import {Headers, Http} from '@angular/http';
import { Injectable } from "@angular/core";

@Injectable()
export class ServerService {

  constructor(private http: Http) {}

  storeServers(servers: any[]) {
    const headers = new Headers({
      'Content-Type': 'application.json'
    });
    return this.http.post('https://udemy-ng-9bcd1.firebaseio.com/data.json', servers, {headers: headers}); we add the header as a third argument
  }

}
```

*Retrieving Data*

We can retrieve data from a server using the get method.
```
  getServers() {
    return this.http.get('https://udemy-ng-9bcd1.firebaseio.com/data.json') // this returns data from server. We need only pass it the url we want to retrieve data from.
      .map( // map allows us to transform the retrieved data and return it as another observable.
          (response: Response) => {
            const data = response.json();
            for (const server of data) {
              server.name = 'FETCHED_' + server.name
            }
            return data;
          }
        );
  }

```

*Error Handling*
We can handle any errors returned from the server with the `.catch` method.

```
getServers() {
    return this.http.get('https://udemy-ng-9bcd1.firebaseio.com/data')
      .map(
          (response: Response) => {
            const data = response.json();
            for (const server of data) {
              server.name = 'FETCHED_' + server.name;
            }
            return data;
          }
        )
        .catch(
          (error: Response) => {
           return Observable.throw('Something went wrong'); // .catch does not automatically return an observable, so we have to specify it.
          });
  }
```

This allows us to send different error messages to our components which can be handled by modal windows or displays on the page.
