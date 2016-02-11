# EventSource [![Build Status](https://secure.travis-ci.org/aslakhellesoy/eventsource-node.png)](http://travis-ci.org/aslakhellesoy/eventsource-node) [![Dependencies](https://david-dm.org/aslakhellesoy/eventsource-node.png)](https://david-dm.org/aslakhellesoy/eventsource-node) [![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/aslakhellesoy/eventsource-node/trend.png)](https://bitdeli.com/free "Bitdeli Badge")


[![NPM](https://nodei.co/npm/eventsource.png?stars&downloads)](https://nodei.co/npm/eventsource/)
[![NPM](https://nodei.co/npm-dl/eventsource.png)](https://nodei.co/npm/eventsource/)

This library implements the [EventSource](http://www.w3.org/TR/eventsource/) client for Node.js. The API aims to be W3C compatible.

## Install

    npm install eventsource

## Example

    npm install
    node ./examples/sse-server.js
    node ./examples/sse-client.js  (Node.js client)
    open http://localhost:8080     (Browser client - both native and polyfill)
    curl http://localhost:8080/sse (Enjoy the simplicity of SSE)

## Extensions to the W3C API

### Setting HTTP request headers

You can define custom HTTP headers for the initial HTTP request. This can be useful for e.g. sending cookies
or to specify an initial `Last-Event-ID` value.

HTTP headers are defined by assigning a `headers` attribute to the optional `eventSourceInitDict` argument:

```javascript
var eventSourceInitDict = {headers: {'Cookie': 'test=test'}};
var es = new EventSource(url, eventSourceInitDict);
```

### Allow unauthorized HTTPS requests

By default, https requests that cannot be authorized will cause connection to fail and an exception
to be emitted. You can override this behaviour:

```javascript
var eventSourceInitDict = {rejectUnauthorized: false};
var es = new EventSource(url, eventSourceInitDict);
```

Note that for Node.js < v0.10.x this option has no effect - unauthorized HTTPS requests are *always* allowed.

### HTTP status code on error events

Unauthorized and redirect error status codes (for example 401, 403, 301, 307) are available in the `status` property in the error event.

```javascript
es.onerror = function (err) {
  if (err) {
    if (err.status === 401 || err.status === 403) {
      console.log('not authorized');
    }
  }
};
```
