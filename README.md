
[![npm version](https://badge.fury.io/js/ng2-signalr.svg)](https://badge.fury.io/js/ng2-signalr)
![live demo](https://img.shields.io/badge/demo-live-orange.svg)

# ng2-signalr
An angular typescript library that allows you to connect to Asp.Net SignalR

## Features:
 1. 100% typescript
 2. use rxjs to observe server events 
 3. write unit tests easy using the provided SignalrConnectionMockManager & ActivatedRouteMock

## [ng2-signalr live demo](http://ng2-signalr-webui.azurewebsites.net)
![ng2-signalr](https://cloud.githubusercontent.com/assets/2285199/22845870/f8cdaff4-efe4-11e6-905d-a471a998125a.gif)
source: [ng2 signalr demo](https://github.com/HNeukermans/ng2-signalr.demo.webui.systemjs)

## Installation
```
npm install ng2-signalr --save
```

##Setup
inside app.module.ts
```
import { SignalRModule } from 'ng2-signalr';
import { SignalRConfiguration } from 'ng2-signalr';

const config = new SignalRConfiguration();
config.hubName = 'Ng2SignalRHub';
config.qs = { user: 'donald' };
config.url = 'http://ng2-signalr-webui.azurewebsites.net/';

@NgModule({
  imports: [ 
    SignalRModule.configure(config)
  ]
})
```


## How to listen for server side events
```
// 1.create a listener object
let onMessageSent$ = new BroadcastEventListener<ChatMessage>('ON_MESSAGE_SENT');
 
// 2.register the listener
this.connection.listen(onMessageSent$);
 
// 3.subscribe for incoming messages
onMessageSent$.subscribe((chatMessage: ChatMessage) => {
       this.chatMessages.push(chatMessage);
});
``` 

## How to invoke a server method
```
// invoke a server side method
this.connection.invoke('GetNgBeSpeakers').then((data: string[]) => {
     this.speakers = data;
});

// invoke a server side method, with parameters
this.connection.invoke('ServerMethodName', new Parameters()).then((data: string[]) => {
     this.members = data;
});
``` 
 
## How to listen for connection status
```
this.connection.status.subscribe((status: ConnectionStatus) => {
     this.statuses.push(status);
});
```

## Also supported 
```
// start/stop the connection
this.connection.start();
this.connection.stop();
 
// listen for connection errors
this.connection.errors.subscribe((error: any) => {
     this.errors.push(error);
});
```

## Best practises 
```
// if you want your component code to be testable, it is best to use a route resolver and make the connection there
import { Resolve } from '@angular/router';
import { SignalR, SignalRConnection } from 'ng2-signalr';
import { Injectable } from '@angular/core';

@Injectable()
export class ConnectionResolver implements Resolve<SignalRConnection> {

    constructor(private _signalR: SignalR)  {

    resolve() {
        console.log('ConnectionResolver. Resolving...');
        return this._signalR.connect();
    }
}

// then inside your component
 constructor(
    private route: ActivatedRoute) {

  }

  ngOnInit() {
    this.connection = this.route.snapshot.data['connection'];
 }


```

### Detailed webpack install
```
npm install jquery signalr expose-loader --save

//inside vendor.ts
import 'expose-loader?jQuery!jquery';
import '../node_modules/signalr/jquery.signalR.js';
```

### Detailed systemjs install
```
TODO
```
>>>>>>> af93c8777fb64c74f74a875e5da60a168f410e06

## Issue Reporting

If you have found a bug or if you have a feature request, please report them at this repository issues section. 

## Contributing

Pull requests are welcome!

## Building

Use `tsc` to compile and build. A `/dist` folder is generated.


##TODO: Code coverage

Use `npm test` cmd to compile and run all tests. 

## Unit testing

Use `npm test` cmd to compile and run all tests. Test runner is configured with autowatching and 'progress' as test reporter. 

  
