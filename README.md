# ChuckT node module

ChuckT-node is a node server component for triggering and/or listening to
events over the [SockJS](https://github.com/sockjs/sockjs-node) websocket
API. This client is intended to be used in conjunction with the client-side
ChuckT JavaScript library:

 * [chuckt](https://github.com/epixa/chuckt)
 
# ChuckT-Redis

Injects publish and subscribe into ChuckT  which almost replicates and simplifies the core features of socket.io into sockjs.
 

# Usage:

### Installation

The recommended way to install chuckt-node is through npm:

`npm install chuckt_redis`

### Initialization

ChuckT instances require a socket connection:

```javascript
var sockjs = require('sockjs');
var chuckt = require('chuck_redis');

// To set a global reference to all clients
var connections={};

// A global reference to the redis publisher
var publisher = redis.createClient();

var sock = sockjs.createServer();
sock.on('connection', function(conn) {
  var socket = new ChuckT(conn,connections,publisher);
  // ... do chuckt stuff like add listeners or emit events
```



By default the clients don't subscribe to namespaces, as not all clients require to be join any namespaces. To initialise pubsub for a client 

```javascript
socket.initPub();
```

chuck_redis has 4 basic methods

### Emit an event to the frontend/browser

```javascript
socket.emitToBrowser('some-event', 'bar');
```

### Emit an event to all the clients browsers. 

```javascript
socket.emitToAllBrowsers('some-event', 'bar');
```

### Emit an event to all the clients in a channel

```javascript
socket.emitToChannel('some-event', 'bar');
```

### Emit an event to all the clients in a channel except the broadcaster 

```javascript
socket.emitToPeers('some-event', 'bar');
```


### Change the namespace of the socket

```javascript
socket.switchChannel('nameOfTheChannel');
```


Functions such as ```emitToBrowser``` and ```emitToAllBrowsers``` works even if a user if not part of any namespaces.

When any emit function is called you can send any number or arbitrary arguments.


### Listen to events fired from the frontend

Any number of arguments and even a callback can be passed with events. The
callback is essentially just a proxy to a callback that is defined (and
executed) on the frontend:

```javascript
chuckt.on('another-event', function(arg1, arg2, callback) {
  // ... do stuff with arguments
  callback();
});
```

Some events may only pass a callback:

```javascript
chuckt.on('another-event-2', function(callback) {
  // ... do stuff without arguments
  callback();
});
```

You may want to pass arguments back to the frontend's callback:

```javascript
chuckt.on('another-event-3', function(callback) {
  // ... do stuff without arguments
  callback('dear frontend', 'you may find these arguments compelling');
});
```

And sometimes, your event may not come with any arguments nor a callback:

```javascript
chuckt.on('another-event-4', function() {
  // ... do stuff without ever acknowledging receipt
});
```


A big thanks to [epixa/chuckt-node](https://github.com/epixa/chuckt-node) for making event driven possible in sockjs.

