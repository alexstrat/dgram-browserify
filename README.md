Kind of udp support for the browser : replace the [dgram](http://nodejs.org/api/dgram.html) module. Behind the scene, a [socket.io](http://socket.io)/udp proxy makes this possible.

*dgram-browserify* is a wrapper around [simudp](https://github.com/alexstrat/simudp) for automatic [browserify](https://github.com/substack/node-browserify) support.

**Be careful**, for the [moment](https://github.com/substack/node-browserify/pull/143), the main *browserify* version provides a broken implementation of *Buffer*. That's why [this](https://github.com/toots/node-browserify) version should be used..

### Installation

```bash
$ npm install dgram-browserify
```

### Use

**Server-side**, the proxy server should be launched :

```js
var server = require('http').createServer();

require('dgram-browserify').listen(server);

server.listen(8080);
```

*Note :* [see](https://github.com/alexstrat/simudp/blob/master/lib/server.js#L29-43) *listen* options

**Browser-side** with *browserify* :

```js
var dgram = require('dgram');

//be sure Buffer is present
var Buffer = require('buffer').Buffer;

var socket = dgram.createSocket('udp4');

var hello = new Buffer('hello');
socket.send(hello, 0, hello.length, 3000, 'anywhere.com');

socket.on('message', function(buf, rinfo) {
  //...
});

//you've understood, it's dgram for the browser...
```