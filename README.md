OpenFaaS Node.js 10 (LTS) and Express.js micro-service template
=============================================

This template provides additional context and control over the HTTP response from your function.

## Status of the template

This template is pre-release and is likely to change - please provide feedback via https://github.com/openfaas/faas

The template makes use of the OpenFaaS incubator project [of-watchdog](https://github.com/openfaas-incubator/of-watchdog).

## Supported platforms

* x86_64 - `node10-express`

## Trying the template

```
$ export USERNAME=alexellisuk

$ faas template pull https://github.com/openfaas-incubator/node10-express-service
$ faas new --lang node10-express-service microservice1 --prefix="${USERNAME}"
```

## Example usage


```js
"use strict"

module.exports = async (config) => {
    const routing = new Routing(config.app);
    routing.configure();
    routing.bind(routing.handle);
}

class Routing {
    constructor(app) {
        this.app = app;
    }

    configure() {
        const bodyParser = require('body-parser')
        this.app.use(bodyParser.json());
        this.app.use(bodyParser.raw());
        this.app.use(bodyParser.text({ type : "text/*" }));
        this.app.disable('x-powered-by');        
    }

    bind(route) {
        this.app.post('/*', route);
        this.app.get('/*', route);
        this.app.patch('/*', route);
        this.app.put('/*', route);
        this.app.delete('/*', route);
    }

    handle(req, res) {
        res.send(JSON.stringify(req.body));
    }
}
```

*handler.js*
