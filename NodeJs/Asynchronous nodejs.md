### Asynchronous node

Below is an example of synchronous code:
```
console.log('Starting');

console.log('Stopping');
```

regardless of how long each line of code takes to run, the code runs in order. That is a synchronous running model. Asynchronous is when code doesn't run in the order it is depicted in the file.
```
console.log('Starting');
setTimeout(() => console.log('2 sec timer'), 2000)
console.log('Stopping')
```
In an async non-blocking model the starting and stopping log and after 2 secs the 2 sec timer log runs.
```
console.log('Starting');
setTimeout(() => console.log('2 sec timer'), 2000)
setTimeout(() => console.log('0 sec timer'), 0)
console.log('Stopping')
```
The output of this is:
Starting
Stopping
0 sec timer
2 sec timer

Surprisingly the 0 sec setTimeout isn't immediately run. This is because of how NodeJs works.

## Call stack, callback queue and event loop

The call stack is a simple data structure provided by the v8 javascript engine. The job of the call stack is to track the execution of the program and it does that by keeping track of all the functions that are running. The data structure for the call stack can be added to or removed from. Node wraps your code in an anonymous function and it is known as the main function. The function is added to the call stack and the code begins to execute. When a function is called within main, it is dropped to the top of the call stack. When it has executed it is removed from the call stack. At the end of the code, the main function will also be removed from the callstack.

When you run an async function like setTimeout, the function does run and leave the call stack, and by doing so, registers an event in the node api. While we're waiting for the node api to happen, we can exxecute other things from the call stack. Javascript is a single threaded language and the code we run is single threaded, but behind the scenes, c++ itself runs in multiple threads. When the events complete, it is pushed into the callback queue. The callback queue keeps a list of every item that is waiting to execute. Before it can be execute it needs to be added to the callstack. The event loop will look to the call stack and the callback queue. If the call stack is empty it will then add the function to the call stack from the callback queue. It will need to wait for main to finish running before it can start executing stuff from the callback queue. 

## Making http calls
Http requests allow us to access data from the outside world. Http requests are at the core of building applications. 

```
const request = require('request'); // request is a wrapper around the nodejs http call

const url = 'https://api.darksky.net/forecast/87240058ac11fb36941c1cdfe13d88a6/37.8267,-122.4233'
request({url}, (err, res) => { // request takes an object and a callback function as an argument.
    const data = JSON.parse(res.body)
    console.log('response', data.currently)
})
```