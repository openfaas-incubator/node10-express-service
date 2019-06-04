OpenFaaS Node.js 10 (LTS) and Express.js micro-service template
=============================================

This template provides Node.js 10 (LTS) and full access to [express.js](http://expressjs.com/en/api.html#req.is) for building microservices for OpenFaaS, Docker, Knative and Cloud Run.

## Status of the template

This template is experimental and I would like your feedback through GitHub issues.

## Supported platforms

* x86_64 - `node10-express-service`

## Get started

```
$ export USERNAME=alexellisuk

$ faas template pull https://github.com/openfaas-incubator/node10-express-service
$ faas new --lang node10-express-service microservice1 --prefix="${USERNAME}"
```

## Example usage

### Minimal example with one route

```js
"use strict"

module.exports = async (config) => {
    const app = config.app;
    app.disable('x-powered-by');

    app.get('/', (req, res) => {
        res.send("Hello world");
    });
}
```

### Minimal example with one route and `npm` package

```
npm install --save moment
```

```js
"use strict"

const moment = require('moment');

module.exports = async (config) => {
    const app = config.app;
    app.disable('x-powered-by');

    app.get('/', (req, res) => {
        res.send(moment());
    });
}
```

### Example usage with multiple routes, middleware and ES6

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
