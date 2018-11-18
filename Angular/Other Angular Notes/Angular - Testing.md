**Angular Testing**
Why unit test:
- Does the component works as intended?
- Does the pipe work as intended?
- Does the service work as intended?
- Does the injection work as intended?
- Does the input work as intended?


Tests are good because:
- Guard against breaking changes.
- Analyze code behaviour (Expected and Unexpected)
- Reveal Design mistakes

https://angular.io/guide/testing

Unit tests should be used when testing a pipe or service. Angular refers to these as shallow tests.
Most other tests should be done using the Angular testing utilities using the inbuilt testing class.

*Testing services*

https://blog.thoughtram.io/angular/2016/11/28/testing-services-with-http-in-angular-2.html

An example of a usual service that calls to a database is below.

```
import { Injectable, Inject } from '@angular/core';

import 'rxjs/add/operator/map';

@Injectable()
export class VideoService {

  constructor(private http: Http) {}

  getVideos() {
    return this.http.get('ApiURL')
                    .map(res => res.json().data);
  }
}
```

We need to configure a testing module so that we don't actually make http calls to the server. We can do this using MockBackend instead. NgModule configures injectors and a testing module allows us to do exactly that.

When testing services or components that don't have any dependencies we can instantiate them manually as below:

```
it('should do something', () => {
  let service = new VimeoService();

  expect(service.foo).toEqual('bar');
});
```

To configure a testing module we need to use angular's `TestBed`. `TestBed` is Angular's primary API to configure and initialize environments for unit testing and gives us methods for creating components and services in unit tests. We can create a module that overrides the actual dependencies with testing dependencies using `TestBed.configureTestingModule()`.

```
import { TestBed } from '@angular/core/testing';

describe('VideoService', () => {

  beforeEach(() => {

    TestBed.configureTestingModule({
      imports: [HttpModule],
      providers: [
         VideoService,
         { provide: XHRBackend, useClass: MockBackend }
       ]
    });
  });
});
```

This configures an injector for our tests that knows how to create our `VideoService` as well as the `Http` service. We want to swap out Http's ConnectionBackend to perform requests witha  different one.

This is what Angular's Http service looks like
```
@Injectable()
export class Http {

  constructor(
    protected _backend: ConnectionBackend,
    protected _defaultOptions: RequestOptions
  ) {}

  ...
}
```

By adding an `HttpModule` to our testing module, providers for `Http`, `ConnectionBackend` and `RequestOptions` are already configured. We can override these using the `providers` property.

We can create a new provider which instantiates the class with a different backend. `MockBackend` stops real Http requests being made and also provides API's to subscribe to opened connections and send mock responses.
We can tell our injector to inject an instance of `MockBackend` whenever someone asks for an `XHRBackend` which is what the `Http` method does behind the scenes.

Angular's testing module comes with the helper function `inject()`, which injects service dependencies. This is good because it means we don't have to access the injector ourselves/ `inject()` takes a list of provider tokens (that we created when configuring the testbed module), and a function with the test code, and returns a function in which the test code is executed. We can pass it straight into our spec and remove the anonymous function we've introduced in the first place.
```
import { TestBed, inject } from '@angular/core/testing';
...
it('should return an Observable<Array<Video>>',
  inject([/* provider tokens */], (/* dependencies */) => {
  // test goes here
}));
```

We want to inject our MockBackend into the providers too so we can simulate a response from the server.
```
it('should return an Observable<Array<Video>>',
  inject([VideoService, XHRBackend], (videoService, mockBackend) => {
    mockBackend.connections.subscribe((connection) => {
      // This is called every time someone subscribes to
      // an http call.
      //
      // Here we want to fake the http response.
    });

    ...
}));
```

We can subscribe to all opened connections with the `MockBackend.connections`. We then want to make the connection send a response back.
```
...
const mockResponse = {
  data: [
    { id: 0, name: 'Video 0' },
    { id: 1, name: 'Video 1' },
    { id: 2, name: 'Video 2' },
    { id: 3, name: 'Video 3' },
  ]
};

mockBackend.connections.subscribe((connection) => {
  connection.mockRespond(new Response(new ResponseOptions({ // a type response with defined ResponseOptions
    body: JSON.stringify(mockResponse)
  })));
});
...
```
