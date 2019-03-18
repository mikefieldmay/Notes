### What is Nodejs

Before Nodejs, javascript was limited to being run only in the browser. You weren'y able to use js to access the file system and to interact with databases. Node is a way to run js code on the server, not just on the client. It is a javascript runtime built on Chrome's javascript engine. The job of the engine is to take javascript code and compile it into machine code that the machine can execute. The v8 engine is written in c++. Chrome and nodejs are largely written in c++.
Nodejs provides custom functionality specific to an environment. It provides the functionality to set up webservers and read files. It has custom functions and objects injected. Node doesn't know how to run javascript code. It passes the code and some c++ bindings to the v8 engine to compile to c++ and in return allows access to features like file reading and other interactions with the computer. You can imagine that certain javascript methods are mapped to c++ functions.
The command node in the terminal allows us to use a javascript REPL (Read Eval Print Loop). In the Chrome browser, there is a window object and a document for accessing the dom. Nodejs' version is global and process. It has methods and other things. 

## Why learn nodejs

Nodejs uses an event driven, non-blocking I/O model that makes it lightweight and efficient. I/O stands for input output. Node uses I/O whenever it needs to communicate with the outside world e.g communicating with the machine it's running on by reading some data from a file, or by communicating with a server/querying a database. I/O operations take time, but because nodejs is non-blocking it can do other things whilst those operations are taking place. It can continue to process other code and make other requests. Non-blocking came originally from the browser because it needed to do other things whilst I/O operations were taking place. We can continue to do other things whilst the operations are taking place. Event-driven is just the process of registering those callbacks and having them called when the I/O operation is done.

## The Module System
Nodejs has a ton of core modules available to us. SOme are immediately available (e.g console.log) whislt others need to be manually added to our js files. One such example is file system.
```
var fs = require('fs') // file system module is loaded in. Require is a function is how we load things into node

fs.writeFileSync('notes.txt', 'This file was created by nodejs')
```