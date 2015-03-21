# What is SPARC

SPARC is the first *vehicle specific smart-device* for delivering connected-car services B2B.

SPARC is the only smart-device in the IoT ecosystem specifically conceived to eliminate the barriers to entry into the connected-car market B2B because it:

* universally integrates with any vehicle, car or truck without any installation needed;
* costs less than any other device on the market;
* is operated directly without any smartphone needed and in a safe way for the drivers;
* can be programmed directly or through server side APIs;
* offers connectivity out-of-the-box;
* can interoperate with other devices through `Bluetooth`, `NFC` or `USB`
* can be shared amongst multiple drivers without any issue of privacy or ownership
* includes a unique suite of E2E features immediately useful to drivers and fleet owners

Apart from the device's embedded technology, the SPARC comes with a unique set of features specific to drivers. These features are provided through a backend powered by `Node.js`. 

## Restify

> Restify exists to let you build "strict" API services that are maintanable and observable. Restify comes with automatic DTrace support for all your handlers, if you're running on a platform that supports DTrace.

For more thorough documentation please refer to [the restify documentation](http://mcavage.me/node-restify)

1. Installation

```
 $ npm install restify
```

2. Usage example

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

3. The `use` function

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

4. Get parameters

    Typical GET Api with parameters
    ```
    server.get('/route/:aParam, function(req, res, next){
        param = req.params.aParam;
    })
    ```

5. Post parameters

    ```
    server.post('/path', function(req, res, next) {
        param = req.body.aParam; 
        ...
    })

6. Hypermedia: chaining URLs internally.

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

## Handlebar

[Handlebars](http://handlebarsjs.com) is a template engine largerly compatible with [mustache](https://mustache.github.io/)

### Template compilation

In the browser the template will be rendered using the following `<script>` tag

    ```html
    <script id="entry-template" type="text/x-handlebars-template">
        <div class="entry">
            <h1>{{title}}</h1>
            <div class="body">
                {{body}}
            </div>
        </div>
    </script>
    ```

Templates are compiled in the code in the following way

    ```node
    //compiling
    var source   = $("#my-template").html();
    var template = Handlebars.compile(source);
    //preparing the context
    var context  = {title: "Whatever title", body: "Whatever body"};
    var html     = template(context)
    ```

which gives the following result

    ```html
    <div class="entry">
        <h1>Whatever title</h1>
        <div class="body">
            Whatever body
        </div>
    </div>
    ```

### Expressions

Property lookup in the current context

    ```html
    <!-- lookup the 'title' property in the current context -->
    <h1>{{title}}</h1>

    <!-- lookup the 'article' property and thus the 'title' property in the result object -->
    <h1>{{article.title}}</h1>

    <!-- Escaped HTML is the default. If title=m&ms, the escaped is m&amp;ms. To unescape use 3 {{{}}} -->
    <p>{{{unescaped}}}</p>
    ```

Helpers are custom expressions that can be programmed.

    ```
    {{link param1 param2}}
    ```

    ```node
    Handlebars.registerHelper('link', function(param1, param2) {
        var url = Handlebars.escapeExpression(param1),
            text = Handlebars.escapeExpression(param2);
      
        return new Handlebars.SafeString(
          "<a href='" + url + "'>" + text + "</a>"
        );
    });
    ```

### Builtin Helpers

The complete list can be seen at [handlebar website](http://handlebarsjs.com/builtin_helpers.html)

