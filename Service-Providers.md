##Introduction

Modern PHP application are full of objects. Service is an object of system, which can be defined. A Service is object pool which perform some sort of global task. Cygnite container allow you to standardize and centralize the way objects are constructed into your application. Container makes your life easier, helps you for super fast development, creating standard architecture for reusable and decoupled codebase. Service providers are the central place to configure your application, used throughout your application whenever you need to do some specific task Service Provider provides it. When Cygnite bootstrapping all Service Providers are registered and bind into Container.

Using Service Providers allow you to customize objects of own or any third party packages. Providers are dependent on the Cygnite IoC container to resolve objects. It makes writing good code so easy.

###Registering Service Provider in the Container

If you open /apps/configs/services.php shipped with Cygnite you will find the closure callback where you need to register your Service Provider into array. All service provider classes will be auto loaded for your application.

In this chapter you will be comfortable creating your own ServiceProviderand register them into Cygnite application.



  ```php
use Cygnite\Foundation\Application as App;

  // Register all service provider
  App::service(function ($app)
  {
       //Add multiple Service Provider into the array      
       $app->registerServiceProvider(
          array(
              "Apps\\Services\\Payment\\ApiServiceProvider",
          )
       );

  });   

```

You can add multiple service provider into the array.

###Creating Service Provider

All service providers extend the Cygnite\DependencyInjection\ServiceProvider class. Service provider require register() method on your provider. Register method is only to bind service into the container, so that service will be accessible globally in your application.

Now, let us have a look at sample service provider.


```php
  namespace Apps\Services\Payment;

  use Cygnite\DependencyInjection\ServiceProvider;
  use Cygnite\Foundation\Application;
  use Apps\Services\PayPal;

  class PaypalServiceProvider extends ServiceProvider
  {
      protected $container;
 
      public function register(Application $app)
      {
            $app['payment.paypal'] = $app->share (function($c) {
                
                return new PayPal($c['paypal.config'], $c['database.default.connection']);
            });
      }
  }

```
In this above example you can see we have registered "PayPal" service into container.

###Getting Service From Container In Controller

Once your service registered using Service Provider you can access service in your controller as such:

```php

 function indexAction() 
 {
     $app = $this->getContainer(); // Or Application::instance();

     show($app['payment.paypal']($app)->get());
 }

```
###How to Define Controllers as Services

Cygnite also make use of it's ioc container to resolve service controller. A controller also can act as Service which lead you to any services passed to the controller can be modified via the container configuration. Another advantage of using Controller as service is you can easily decouple code, inject any dependency via constructor, method, or property. You can do some specific task into your service controller and retrieve it from your container in another controller. You can define configuration in the /apps/configs/services.php file as below.


```php
   $app->setServiceController('hello.controller', '\Apps\Controllers\HelloController');

   Or

  // Manually configure Controller instance
  $app['hello.controller'] = function () use($app)
  {
      return new \Apps\Controllers\HelloController(
                new \Cygnite\Mvc\Controller\ServiceController, $app
      );
  }; 
```

That's all about Cygnite service provider