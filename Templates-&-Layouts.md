## Using Twig Templates & Php Layouts

###Introduction

If you have exposure on Django template engine or smarty template engine you also will like Twig templates, which is very fast, secure and flexible by sensio lab. If you would like to use twig templating system instead of Cygnite plain php templates, you can make use of twig templating just making small configuration in your controller. Cygnite has built in support of beautiful Twig templates. Cygnite Layouts gives you more flexibility to extends its blocks.

### Using Controller Layout

By specifying layout property in your controller and making **$templateEngine** true into your controller you are ready to use twig templates as your view page.
```php
 namespace Apps\Controllers;

 use Cygnite\Mvc\Controller\AbstractBaseController;

 class HomeController extends AbstractBaseController
 {

    protected $layout = 'layout.main.base';

    protected $templateEngine = true;

    /* you can change default template extension here */
    //protected $templateExtension = '.html.twig'; 

    protected $autoReload = true;

    // Activate debugging of your twig template by changing it true or false
    protected $twigDebug = true;


    public function __construct()
    {
         parent::__construct();
    }

    public function indexAction()
    {
         //apps/views/home/index.html.twig
         $this->render('index')->with(array( 'title'=> "Welcome to Cygnite Framework"));
    }  
 }
```

In the above example you can see we activated twig template engine by simply changing **$templateEngine as true**. Cygnite has shipped with sample twig layout page inside /apps/views/layout/main/base.html.twig.

### Creating Simple View Page Using Twig Layout

Let us create a simple view page called index.html.twig and render it in twig base layout.

```html
 //apps/views/home/index.html.twig

 {% extends 'layout/main/base.html.twig' %}

 {% block title %}
    Cygnite Framework - Simple Crud Operation
 {% endblock %}

 {% block content %}
   Your Contents goes here
 {% endblock %}

 {% block footer %}
   Your footer section if you want to override parent layout
 {% endblock %}
```

## Extending Twig

Cygnite is flexible enough to allow you to extend Twig core functionality or write your own extensions or functions.

### Extending Twig By Creating Custom Extension

Extending Twig Extension is simple enough using cygnite view wrapper. You need to register an extension by using the addExtension() method on Template object. Let us see how to extend twig extension below.
```php
  use Twig\Extension;
  use Apps\Extensions\Twig\Extension;

  class Project extends Twig_Extension
  {
    public function getName()
    {
        return 'project';
    }
  }
```
And register it in your controller.

```php
 namespace Apps\Controllers;

 use Cygnite\Mvc\Controller\AbstractBaseController;

 class HomeController extends AbstractBaseController
 {

    protected $layout = 'layout.main.base';

    protected $templateEngine = true;

    /* you can change default template extension here */
    //protected $templateExtension = '.html.twig'; 

    protected $autoReload = true;

    // Activate debugging of your twig template by changing it true or false
    protected $twigDebug = true;


    public function __construct()
    {
         parent::__construct();
    }

    public function indexAction()
    {

         // Adding custom extension to twig template
         $this->getTemplate()->addExtension(new \Apps\Extensions\Twig\Extension\Project());

         //apps/views/home/index.html.twig
         $this->render('index')->with(array( 'title'=> "Welcome to Cygnite Framework"));
    }  
 }
```

For more detailed information about twig extension you can read beautiful documentation by sensio lab here.

### Extending Twig By Creating Custom Functions

Adding a function is similar to adding a new filter function to twig engine. You can write your own custom functions which can be accessible in the twig template. This can be done by calling the **addFunction()** method on the Template instance:
```php
 namespace Apps\Controllers;

 use Cygnite\Mvc\Controller\AbstractBaseController;

 class HomeController extends AbstractBaseController
 {

    protected $layout = 'layout.main.base';

    protected $templateEngine = true;

    /* you can change default template extension here */
    //protected $templateExtension = '.html.twig'; 

    protected $autoReload = true;

    // Activate debugging of your twig template by changing it true or false
    protected $twigDebug = true;


    public function __construct()
    {
         parent::__construct();
    }

    public function indexAction()
    {

         // Adding custom function to twig template
        $path = 'your-input-with-dash';
 
        $this->getTemplate()->addFunction('mySimpleFilter', function () use ($path)
        {
           // .. we will replace dash with underscore and return it

        });

        //apps/views/home/index.html.twig
        $this->render('index')->with(array( 'title'=> "Welcome to Cygnite Framework"));
    }  
 }
```

You can find more detail information of twig in Twig Template Documentation.