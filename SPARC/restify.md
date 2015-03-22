# Restify

> Restify exists to let you build "strict" API services that are maintanable and observable. Restify comes with automatic DTrace support for all your handlers, if you're running on a platform that supports DTrace.

For more thorough documentation please refer to [the restify documentation](http://mcavage.me/node-restify)

## Installation

```
 $ npm install restify
```

## Usage example

    ```node
    var restify = require('restify');
    
    function respond(req, res, next){
        res.send('hello ' + req.params.name);
        next();
    }
    
    var server = restify.createServer();
    server.get('/hello/:name', respond);
    server.head('/hellp/:name', respond);
    
    server.listen(8080, function() {
        console.log('%s listening at %s', server.name, server.url);
    })
    ```

    in the shell I can let node run the file and use `curl` to test this incredibly useful API

    ```bash
    $ curl -is http://localhost:8080/hello/elio -H 'accept: text/plain'

    HTTP/1.1 200 OK
    Content-Type: text/plain
    Content-Length: 9
    Date: Sat, 21 Mar 2015 13:49:26 GMT
    Connection: keep-alive

    hello elio

    $ curl -is http://localhost:8080/hello/elio

    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 9
    Date: Sat, 21 Mar 2015 13:49:26 GMT
    Connection: keep-alive

    "hello elio"

    $ curl -is http://localhost:8080/hello/elio -X HEAD -H 'connection: close'

    HTTP/1.1 200 OK
    Content-Type: application/json
    Content-Length: 12
    Date: Mon, 31 Dec 2012 01:42:07 GMT
    Connection: close
    ```

## The `use` function

    Function `use` can be used (pun not intended) for specifying a handle to manage a given route

    ```node
    server.use(function pippo(req, res, next){
        ...
        next()
    })
    ```
    
    `use` is also the way to load the default handler of restify. Some of them follow:

    * Accept Parser `server.use(restify.acceptParser(server.acceptable));`
    * Query Parser `server.use(restify.queryParser());` which parses HTTP Query (`/param?id=whatever&name=whateverElse`)
    * Body Parser `server.use(restify.BodyParser());`

## Get parameters

    Typical GET Api with parameters
    ```
    server.get('/route/:aParam, function(req, res, next){
        param = req.params.aParam;
    })
    ```

## Post parameters

    ```
    server.post('/path', function(req, res, next) {
        param = req.body.aParam; 
        ...
    })

## Hypermedia: chaining URLs internally.

    If a parameterized route was defined with a string (not a regex), you can render it from other places in the server. This is useful to have HTTP responses that link to other resources, without having to hardcode URLs throughout the codebase. Both path and query strings parameters get URL encoded appropriately.

    ```node
   server.get({name: 'city', path: '/cities/:slug'}, /* ... */);

    // in another route
    res.send({
      country: 'Australia',
      // render a URL by specifying the route name and parameters
      capital: server.router.render('city', {slug: 'canberra'}, {details: true})
    }); 
    ```


