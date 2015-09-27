##Introduction

Cygnite has powerful dependency injection container to resolve your class dependencies at runtime. [Dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) is a software design pattern that implements inversion of control IoC). Cygnite IoC Container gives you more flexibility to write powerful, decoupled and large application.

###Usage of Container

Dependency Injection
Cygnite Container can resolve dependencies different ways.

1. Autowiring (Automatic Resolution). Constructor Injection. 
2. Interface Injection. Binding an Interface with it's implementation class.
3. Property Injection.  

###Autowiring

Cygnite container has powerful mechanism to resolve all dependencies of the class at runtime without any configuration. Container uses [PHP Reflection](http://www.php.net/manual/en/book.reflection.php) to understand what parameters a constructor needs. Automatic resolution works only with the constructor injection and must be type-hint. 
For Example-

```php 
namespace Apps\Extensions;

use Apps\Extensions\Bar;
use Cygnite\Foundation\Application as App;

class Foo 
{
    private $bar;

    public function __construct(Bar $bar) 
    {
        $this->bar = $bar;
    }
}

$foo = App::instance(function ($container) 
{
    return $container->make('\Apps\Extensions\Foo');
});

OR 

$app = App::instance();
$foo = $app->resolve('apps.extensions.foo');
```


You can see without doing any kind of configuration Container automatically resolves all dependencies of Foo class and injecting Bar class into it.


```php
namespace Apps\Extensions;

class FooBar
{

}

$foo = $app->resolve('apps.extensions.foo-bar');
```


###Type-Hinting Controller

 
 ```php
use Cygnite\Foundation\Application;
 use Cygnite\AbstractBaseController;
 use Apps\Extensions\Api;

 class HomeController extends AbstractBaseController
 {

    private $api;

    public function __construct(Api $api, $type = "service")
    {
       $this->api = $api;
    }

    public function indexAction()
    {
       $this->api->makePayment();

    }
 }
```
Autowiring works only with the class, however if type of the parameter is Interface then you need to configure to inform Container which Interface implementation should be injected.

###Interface Injection

Register an Interface with it's implementation class. This can be done using apps/configs/definitions/DefinitionManager class as below.



 ```php
  public function registerAlias()
   {
         return array(
              'FooInterface' => '\\Apps\\Extensions\\General'
         );
   }
```


and your controller may look like following.


 
 ```php
use Cygnite\Foundation\Application;
 use Cygnite\AbstractBaseController;
 use Apps\Extensions\FooInterface;

 class HomeController extends AbstractBaseController
 {

    private $foo;

    public function __construct(FooInterface $foo)
    {
       $this->foo = $foo;
    }
 }
```
Since you registered FooInterface to it's implementation class. Cygnite container automatically inject it runtime.

###Property Injection

Some time over uses of constructor injection may be messy, in that case you may use property injection. You need to configure which instance should be injection into what property of your controller. This can be achieve using DefinitionManager.php. 
For Example.


   
   ```php
 return array(
                 'HomeController' => array(
                    'service' => 'apps.extensions.service',
                    'api' => 'apps.extensions.api'
                 ),
                 'ProductController' => array(
                    'foo' => 'apps.extensions.foo',
                 ),
            );
```

Above we have set service object into service property of HomeController, now Cygnite Container is aware about which instance to inject into service property.


 
 ```php
use Cygnite\Foundation\Application;
 use Cygnite\AbstractBaseController;
 use Apps\Extensions\Foo;

 class HomeController extends AbstractBaseController
 {

    private $foo;

    // Instance of class \Apps\Extensions\Service inject here
    private $service;

    public function __construct(Foo $foo)
    {
       $this->foo = $foo;
    }

    public function indexAction()
    {
       // access the service object directly to do some task
       $this->service->makePayment();
    }
 }
```

###Defining a Parameters or Values

Set a parameter or value into Container using instance as array.



  ```php
 use Cygnite\DependencyInjection\Container;

   $container = new Container();

   // define some parameters as array 
   $container['foo'] = 'Bar';
   $container['foo_bar'] = 'FooBar';

   OR
  //Define parameter as property
  $container->bar = 'bar';
  $container->baz = new Baz;

```  

###Accessing Session Instance Into Controller

```php

   $container = $this->get('cygnite.common.session-manager.session');
```


###Creating Service

Service is an object of system, which can be defined by anonymous functions. If you are fond of Pimple syntax then you will also love Cygnite container.



 ```php
  use Apps\Extensions\General;
   use Apps\Extensions\Api;  

   $container['foo'] = 'Foobar';
   $container['bar'] = 'baz';

   $container->general = function ($c) {
       return new General($c['foo']);// Allowing you to refer other service or values
   };

   $container->api = function ($c) {
       return new Api($c['bar'], $c->general);// Allowing you to refer other service or values
   };
```


If you look at the above example we have stored parameters into the container, as well as stored general service (General object) into container. Now when returning Api service we are able to inject General and other parameters into the Api. You may also notice the Closure has access of container instance inside it.

###Extending Service

In the above example we have created service, now you may also want to extend it. To do that simply use extend method. 
Have a look at the below example.


  
  ```php
$container['apiKey'] = 'xxxxxxx';

  //Setter Injection and property injection
  $container->api = $container->extend('api', function ($api, $c)
  {
      $api->key = $c['apiKey'];// set key to public property
      $api->setBar($c->bar);

      return $api;
  });
 
  $container->api; //Hold your Api service
```

###Sharing an Object

Binding as shared is almost similar to Singleton pattern, where the first time object is created and return same object for all subsequent call.


```php
 $container->apiGateway = $container->share(function ($c)
 {
       return new ApiGateway($c->api);
 });

 $container->apiGateway ; // Holds singleton object

```
###Store factory of objects into Container

You also can use the power of [SplObjectStorage](http://www.php.net/manual/en/class.splobjectstorage.php). Simply get the object of container and attach factory of objects into it.

```php

 $stdClass = new StdClass;
 $general = new General;
 $api = new Api;

 $container->attach($general);
 show($container->contains($api));

 $container->detach($general);

 $general[$api] = "hello";

 
 $container->addAll($general);
 echo $container[$general]."\n";

```