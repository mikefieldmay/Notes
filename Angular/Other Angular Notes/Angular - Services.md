**Services**

Services can act as a central store of data for the entire application.
They could also be used to store reusable bits of code like a log statement.
They act as a central repository.
A service is just a normal typescript class and as such does not need a decorator.
```
export class LoggingService {

    logStatusChange(status: string) {
        console.log('A server status changed, new status: ' + status);
    }
}
```

Services are passed into components with dependency injection.
This logging service is passed into
```
import { Component, EventEmitter, Output } from '@angular/core';
import { LoggingService } from "../logging.service"; // service is imported
@Component({
  selector: 'app-new-account',
  templateUrl: './new-account.component.html',
  styleUrls: ['./new-account.component.css'],
  providers: [LoggingService] //service is added as a provider
})
export class NewAccountComponent {
  @Output() accountAdded = new EventEmitter<{name: string, status: string}>();

  constructor(private loggingService: LoggingService) {} // Angular injects a new instance of the service into the component

  onCreateAccount(accountName: string, accountStatus: string) {
    this.accountAdded.emit({
      name: accountName,
      status: accountStatus
    });
    this.loggingService.logStatusChange(accountStatus);
  }
}

```

*n.b* if you add a service as a provider in the component, it create a new instance per component. If added to the app.module, it creates one instance across all components.

*Hierarchical Injector*

When a service is provided on one component. It creates an instance of this service for this component and all it's child components. They all share the same instance of the service.
- AppModule: Same instance of this service is available application wide.
- AppComponent: Same instance is available for all Components (but not for other services).
- Any other component: Same instance of service is available for this component and all it's child components.

If we provide a service to the app module, this service can then be injected into other services.
```

@NgModule({
  declarations: [
    AppComponent,
    AccountComponent,
    NewAccountComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [AccountsService, LoggingService],
  bootstrap: [AppComponent]
})

import { Injectable } from "@angular/core";
import { LoggingService } from "./logging.service";

@Injectable() // we need metadata in order for the service to be injected properly, so for services we use @Injectable
export class AccountsService {
  constructor(private loggingService: LoggingService){}

  addAccount(name: string, status: string){
    this.accounts.push({name, status})
    this.loggingService.logStatusChange(status);
  }

  updateStatus(id: number, status: string){
      this.accounts[id].status = status;
      this.loggingService.logStatusChange(status);
  }
}
```

*Communication through components*
Services can emit events that can be subscribed to by components.
```
// In the emitting service
import { Injectable, EventEmitter } from "@angular/core";
import { LoggingService } from "./logging.service";

@Injectable()
export class AccountsService {
  statusUpdated = new EventEmitter<string>(); // event emitter is established
  constructor(private loggingService: LoggingService){}

  addAccount(name: string, status: string){
    this.accounts.push({name, status})
    this.loggingService.logStatusChange(status); // event is emitted
  }

  updateStatus(id: number, status: string){
      this.accounts[id].status = status;
      this.loggingService.logStatusChange(status);
  }
}

// In the receiving component
import { Component } from '@angular/core';
import { LoggingService } from "../logging.service";
import { AccountsService } from "../accounts.service";
@Component({
  ...
})
export class NewAccountComponent {
  constructor(private loggingService: LoggingService, private accountsService: AccountsService) {
    this.accountsService.statusUpdated.subscribe( //subscribe lets us do things with the data from emitted events.
      (status: string) => alert('New Status ' + status)
    )
  }
  ...
}
```
