# SpawnEngine Node API Proxy

This project is designed to provide an API proxy for web services to contact any number of Spawncoin nodes for basic information regarding the state of the Node. It utilizes a cache that helps speed up the delivery of responses to clients while minimizing the load against the specified daemon by remote callers.

The sample **service.js** includes an example of how to quickly spin up the web service. It supports clustering via PM2 and I ***highly*** recommend that you run it with multiple threads.

## Dependencies

* [NodeJS](https://nodejs.org) v8.x

## Easy Start

This will spin up a copy of the webservice on 0.0.0.0:8080. See the additional options below to customize the port or IP the web service binds to.

```bash
git clone https://github.com/spawncoin/spawncoin-api-proxy.git
cd spawncoin-api-proxy
npm install
node service.js
```

## Keep it Running

I'm a big fan of PM2 so if you don't have it installed, the setup is quite simple.

```bash
npm install -g pm2@latest
pm2 startup
pm2 install pm2-logrotate
pm2 start service.js --watch --name spawncoin-api-proxy -i max
pm2 save
```

## Initialization

This is incredibly simple to setup and use. No options are required but you can customize it as you see fit. Default values are provided below.

```javascript
const SPWNProxy = require('./')

var service = new SPWNProxy({
  cacheTimeout: 30, // How quickly do we timeout cached responses from individual nodes
  timeout: 2000, // How long to wait for underlying RPC calls to return
  bindIp: '0.0.0.0', // What IP address do we bind the web service to
  bindPort: 80 // What port do we bind the web service to
  defaultHost: 'public.spawncoin.org', // The default node to look to for RPC calls
  defaultPort: 11898 // The default port to use on the default node
})
```

## Methods


### service.start()

Starts the web service

```javascript
service.start()
```

### service.stop()

Stops the web service

```javascript
service.stop()
```

## Events

### Event - ***error***

Event is emitted when an error is encountered.

```javascript
service.on('error', (err) => {
  // do something
})
```

### Event - ***ready***

Event is emitted when the web service is listening for connections.

```javascript
service.on('ready', (ip, port) => {
  // do something
})
```

### Event - ***stop***

Event is emitted when the web service is stopped.

```javascript
service.on('stop', () => {
  // do something
})
```

### Querying Multiple Nodes

To query a node other than the one supplied in ```defaultHost``` call any of the API commands in one of the following formats:

* /endpoint
* /:node:/endpoint
* /:node:/:port:/endpoint

Examples:

* /getinfo
* /public.spawncoin.org/getinfo
* /public.spawncoin.org/11898/getinfo
* /json_rpc
* /public.spawncoin.org/json_rpc
* /public.spawncoin.org/11898/json_rpc

## License

Copyright (C) 2018-2019 Brandon Lehmann, The TurtleCoin Developers, Spawncoin Developers

Please see the included LICENSE file for more information.
