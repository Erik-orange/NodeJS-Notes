# NodeJs-ExpressJS-Notes

___

## Node.js

Much of the Node.js core API is built around an idiomatic asynchronous event-driven architecture in which certain kinds of objects (called "emitters") emit named events that cause Function objects ("listeners") to be called.

A simple Node server looks like this:
```js
const http = require('http');

const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.end('Hello World\n');
});

server.listen(port, hostname, () => {
  console.log(`Server running at port: ${port}/`);
});
```
This will print a "Hello World" message on the screen when a user visits `http://localhost:3000`.

#### Events

* All objects that emit events are instances of the `EventEmitter` class. 

  These objects expose an `myEmitter.on()` function that allows one or more functions to be attached to named events emitted by the object.

  When the `EventEmitte`r object emits an event, all of the functions attached to that specific event are called synchronously.

  Any values returned by the called listeners are ignored and will be discarded.

* The `myEmitter.on()` method is used to register listeners, while the `myEmitter.emit()` method is used to trigger the event.

* If an `EventEmitter` does _not_ have at least one listener registered for the `error` event, and an `error` event is emitted, the error is thrown, a stack trace is printed, and the Node.js process exits.

```js
const myEmitter = new MyEmitter();

myEmitter.on('error', (err) => {
  console.error('whoops! there was an error');
});

myEmitter.emit('error', new Error('whoops!'));    // Prints: whoops! there was an error
```

`emitter.emit(eventName, [ ...args])`
Synchronously calls each of the listeners registered for the event named eventName, in the order they were registered, passing the supplied arguments to each.

`emitter.on(eventName, listenerFunc)`
Adds the `listenerFunc` to the end of the listeners array for the event named `eventName`.

#### Streams

A `stream` is an abstract interface for working with streaming data in Node.js.

Streams can be readable, writable, or both. All streams are instances of `EventEmitter`.

**Example** Node.js application that's using streams and implements an HTTP server:
```js
const http = require('http');

const server = http.createServer((req, res) => {
  // `req` is an http.IncomingMessage, which is a Readable Stream
  // `res` is an http.ServerResponse, which is a Writable Stream

  let body = '';

  // Readable streams emit 'data' events once a listener is added
  req.on('data', (chunk) => {
    body += chunk;
  });

  // The 'end' event indicates that the entire body has been received
  req.on('end', () => {
    const data = JSON.parse(body);
    res.write(typeof data);       // Write back something interesting to the user
    res.end();
   }
  });
});

server.listen(1337);
```

`myWritable.end(chunk, encoding, callbackFunc)` all arguments are optional
Signals that no more data will be written to the Writable Stream named `myWritable`.

`writable.write(chunk, encoding, callbackFunc)` all arguments are optional
Writes some data to the stream, and calls the supplied callback once the data has been fully handled.










