## URL Routing

### Routing

A Cygnite routing will invoke the route that matches the current HTTP request’s URI and method. If router doesn't find any routes matching with HTTP request URI and method then Cygnite router automatically throw "404 Page Not Found" error. Cygnite has powerful routing features which allows various routing patterns.

The main features of router are -

* i. Resourceful Routing
* ii. Static Route Patterns
* iii. Dynamic Route Patterns
* iv.Optional Route Subpatterns
* v. GET, POST, PUT, DELETE, and OPTIONS request methods
* vi. Before Route Middleware
* vii. After Router Middleware (Finish Callback)


### Where should I define my custom routing ?

You can define your routing inside **apps/routerconfig.php** for direct routing or Create restful routing into **/apps/router.php**. Router supports closure callback. It respond to your router, can call controller directly from closure syntax or defined resourceful routing, which allow you to write powerful RESTful api.

### Route Patterns

Below are the routing patterns supported by Cygnite Router-

### Default Routing Patterns

Use Short and Simple Syntax for routing.
```php    
   {:num}         = ([0-9]+),
   {:digit}       = (\d+),
   {:name}        = (\w+),
   {:any}         = ([a-zA-Z0-9\.\-_%]+),
   {:all}         = (.*),
   {:namespace}   = ([a-zA-Z0-9_-]+),
   {:module}      = ([a-zA-Z0-9_-]+),   
   {:year}        = \d{4},
   {:month}       = \d{2},
   {:day}         = \d{2}(/[a-z0-9_-]+)
```
If you like to use regular expression for routing you may use below patterns.

For example-
```php
  \d+         = One or more digits (0-9)
  \w+         = One or more word characters (a-z 0-9 _)
  [a-z0-9_-]+ = One or more word characters (a-z 0-9 _) and the dash (-)
  .*          = Any character (including /), zero or more
  [^/]+       = Any character but /, one or more
```

### RESTful Resource Routing

Cygnite router make your development painless. Cygnite also allow you resource routing to controller to build powerful RESTful controllers.

Now let us register resourceful routing to "**UserController**" controller.

 ```php
  use Cygnite\Foundation\Application as App;

  $app = App::instance();
  
  //$app->router->resource('route-name', 'controller'); //syntax

  $app->router->resource('users', 'user');

  // Access router object from the application container using $app->router or 
  // array syntax $app['router']->resource('users', 'user'); 
```

This single route declaration creates multiple routes to handle various RESTful actions of "employee" resource.

Read More: [RESTful Resource Controllers](http://www.cygniteframework.com/2013/08/controllers.html)

### Group Routing

Cygnite router can route group of resource on the same uri, in that case you may use group routing to response to route request.

```php
 $app->router->group('/movies', function($route) {

    //echo "movies ";

    $route->get('/', function() {
        echo "Movies Overview";
    });

    $route->get('/{:num}/{:name}', function($route, $id ,$name) {
        echo "Movie $name and $id";
    });

    $route->get('string/{:digit}', function($route, $id) {
        echo "Movie $id";
    });

 });
```

### Overriding And Custom Routing Patterns

Router flexible enough to override default named routing and also if would like to add some custom routing you can do using where clause.

```php
 $app->router->group('/movies', function($route) {

    $route->where('{:id}', '(\d+)')->get('where/{:id}', function() {
        echo "Movie with custom where clause ";
    });

    $route->where('{:string}', 'name')->get('/where/{:string}', function()
    {
        echo "Movie custom pattern";
    });
 });
```

## Dynamic Routing

### GET Routes

```php
// Dynamic route: /hello/any_name/32
$app->router->get('/hello/{:name}/{:digit}', function($router, $name, $id) 
{   
   echo 'Hello  ' . htmlentities($name). ". Your id is $id ";
});
```
### POST Routes
```php
$app->router->post('/product/', function () 
{
   //Create Product
});
```
### PUT Routes

```php
$app->router->put('/product/{:digit}', function ($router, $id) 
{
   //Update product identified by $id
});
```

### DELETE Routes

```php
$app->router->delete('/product/{:digit}', function ($router, $id) 
{
   //Delete Product identified by $id
});
```
### OPTIONS Routes

```php
$app->router->options('/product/{:digit}', function ($router, $id) 
{
   //Return response headers
});
```

### Registering A Route For Responding Any HTTP Verb

```php
// Will respond to any GET|POST|PUT|PATCH|DELETE
$app->router->any('/foo', function ($router) 
{
   //Hello!! Cygnite

});
```
### Registering A Route to Respond Multiple Verbs

```php
// Will respond to GET|POST
$app->router->match('GET|POST', '/product/', function ($router) 
{
   //Respond to any HTTP verb as GET|POST
});
```
> Note: The leading / at the very beginning of a route pattern is not mandatory, but is recommended.

When multiple sub-patterns are defined, the route handle parameters, are passed into the route handling function in the order they are defined.

```php
$app->router->get('/users/{:digit}/photos/{:digit}', function($router, $userId, $photoId) 
{
    echo 'User #' . $userId . ', photo #' . $photoId);
});
```

### Optional Route Subpatterns

Route sub-patterns can be made optional by making the sub-patterns optional by adding a ? after them. Think of blog URLs in the form of **/blog(/year)(/month)(/day)(/slug)**:

```php
 $app->router->get('/blog(/{:year}(/{:month}(/{:day}?)?)?)?',
     function($router, $year = null, $month = null, $day = null, $slug = null) 
     {
           if (!$year) { echo 'Blog overview'; return; }
           if (!$month) { echo 'Blog year overview'; return; }
           if (!$day) { echo 'Blog month overview'; return; }
           if (!$slug) { echo 'Blog day overview'; return; }

           echo 'Blog Post ' . htmlentities($slug) . ' detail';
     }
 );
```
The code snippet above responds to the URLs **/blog**, **/blog/year**, **/blog/year/month**, **/blog/year/month/day**, and **/blog/year/month/day/slug**.

> Note: With optional parameters it is important that the leading / of the sub-patterns is put inside the sub-pattern itself. Don't forget to set default values for the optional parameters.

The code snipped above unfortunately also responds to URLs like /blog/foo and states that the overview needs to be shown - which is incorrect. Optional sub-patterns can be made successive by extending the parenthesized sub-pattern, so that they contain the other optional sub-patterns: The pattern should resemble /blog(/year(/month(/day(/slug)))) instead of the previous /blog(/year)(/month)(/day)(/slug):

>Note: It is highly recommended to always define successive optional parameters.

To make things complete use quantifiers to require the correct amount of numbers in the URL:

```php


 // Override default patterns using custom patterns

 $app->router->patterns['{:year}'] = '[12][0-9]{3}';
 $app->router->patterns['{:month}'] = '0[1-9]|1[012]';
 $app->router->patterns['{:day}'] = '0[1-9]|[12][0-9]|3[01]'; 
  
 $app->router->get('/archive/{:year}/{:month}/{:day}', 
    function($router, $year = null, $month = null, $day = null, $slug = null) 
    {
       // ...           
    }
 );
```
### Calling Controller from the Closure callback

```php    
$app->router->get('/archive/', function($router) 
{
   Router::call('Home.welcome', array($name, $id));             
});
```
### Calling Module Controller From Router Callback

```php     
$app->router->get('/category/', function($router) 
{
   //Acme::controller.action
   Router::call("Acme::user.index", array($id));            
});
```

### Before Route Middlewares

Router supports Before Route Middlewares, which are executed before the route handling is processed.

Like route handling functions, you hook a handling function to a combination of one or more HTTP request methods and a specific route pattern.

```php
use Cygnite\Helpers\Url;
use Cygnite\Libraries\Session;

$app->router->before('GET|POST', '/admin/{:all}', function() 
{
    if ( !Session::has('user') ) {
        Url::redirectTo('/auth/login');
    }
});
```
Unlike route handling functions, more than one before route middleware is executed when more than one route match is found.

### After Router Middleware

In some case you may also want to execute filter after all routes processed. Run one (1) middleware function, name the After Router Middleware (in other projects sometimes referred to as after app middlewares) after the routing was processed.

```php
 $app->router->after(function()
 {
    // Will execute after all routes processed
 });//After Routing Callback 
```
### Set Custom 404 Page

You can also set your own custom 404 page if no routes matched and exit from here.

```php
 $app->router->set404(function()
 {
    // set 404 message and exit here
 });
```

### Run Callback

The run is route independent.

```php
$app->router->run();
```

> Note: If the route handling function has exit then callback won't be run.


