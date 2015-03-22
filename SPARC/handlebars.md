# Handlebar

[Handlebars](http://handlebarsjs.com) is a template engine largerly compatible with [mustache](https://mustache.github.io/)

## Template compilation

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

## Expressions

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

## Builtin Helpers

The complete list can be seen at [handlebar website](http://handlebarsjs.com/builtin_helpers.html)

