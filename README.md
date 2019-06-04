OpenFaaS Node.js 10 (LTS) and Express.js micro-service template
=============================================

This template provides Node.js 10 (LTS) and full access to [express.js](http://expressjs.com/en/api.html#req.is) for building microservices for [OpenFaaS](https://www.openfaas.com), Docker, Knative and Cloud Run.

With this template you can create a new microservice and deploy it to a platform like [OpenFaaS](https://www.openfaas.com) for:

* scale-to-zero
* horizontal scale-out
* metrics & logs
* automated health-checks
* sane Kubernetes defaults like running as a non-root user

## Status of the template

This template is experimental and I would like your feedback through GitHub issues or [OpenFaaS Slack](https://docs.openfaas.com/community).

## Supported platforms

* x86_64 - `node10-express-service`

## Get started

You can create or scaffold a new microservice using the [OpenFaaS CLI](https://github.com/openfaas/faas-cli).

```
# USERNAME is your Docker Hub account or private Docker registry
$ export USERNAME=alexellisuk

$ faas template pull https://github.com/openfaas-incubator/node10-express-service
$ faas new --lang node10-express-service microservice1 --prefix="${USERNAME}"
```

Once you've written your code you can run `faas-cli build` to create a local Docker image, then `faas-cli push` to transfer it to your registry.

You can now deploy it to OpenFaaS, Knative, Google Cloud Run or even use `docker run`.

See also: [Deploy OpenFaaS](https://docs.openfaas.com/deployment/)

## Example usage

### Minimal example with one route

```js
"use strict"

module.exports = async (config) => {
    const app = config.app;

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
