* Install and Configuration
* Creating Controller
* Creating Model
* Creating View
* Routing
* Display Data in View

### Install and Configuration

Before starting you need to be sure that you have installed Cygnite Framework into the machine. If you haven't already, you can find installation steps in Cygnite Framework [Installation Procedure](http://www.cygniteframework.com/2015/02/installation.html).

### Registering Directories For Auto loading Classes

You can register number of directories to auto-load classes for your application in the **apps/configs/autoload.php**. If you open the file you can see by default, Cygnite register few directories. You can simply add directory namespace with dot separator as like below example.

```php
return array(
 Application::instance()->registerDirectories(
        array(
            'apps.controllers',
            'apps.models',
            'apps.modules',
            'apps.configs.definitions',
            'apps.components.form',
            'apps.extensions',
            'apps.services',
            //...........
        )
    )
);
```

### Creating Controller

To get started, there are sample controller available into the **apps/controllers** directory. You can also create controller. 

```php
namespace Apps\Controllers;

use Cygnite\Foundation\Application;
use Cygnite\Mvc\Controller\AbstractBaseController as BaseController;

class UsersController extends BaseController
{
   public function __construct()
   {
       parent::__construct();
   }

   public function indexAction()
   {
      echo 'Hello World!!';
   }
}
```

### Creating Model

Cygnite Skeleton application also shipped with sample model into your **apps/models/** directory. You can create your model in same directory to interact with your database. Your model class name should be **StudlyCaps**, every model class act as simple orm to map your database tables as objects, Cygnite Model follows ActiveRecord style highly inspired by ruby on rails. Your table name must be lowercase followed by underscore prefix.

Example- If your table name is **guest_book **than your model name should be **GuestBook**.

```php    
namespace Apps\Models;

use Cygnite\Database\Schema;
use Cygnite\Foundation\Application;
use Cygnite\Common\UrlManager\Url;
use Cygnite\Database\ActiveRecord;

class GuestBook extends ActiveRecord
{
   /**
    * Your database name goes here
    * @param database 
    */
    protected $database = 'cygnite';

   /**
    * @param $tableName
    * Optional. If you don't provide table name here class will act as database table.
    */
    //protected $tableName = 'guest_book'; 

   /**
    * Primary key of your table
    * We recommend to have id as primary key
    */
    protected $primaryKey = 'id';

    public function __construct()
    {
        parent::__construct();
    }
}
```

### Creating View

Next, let's create simple view to display our welcome page which is reside in apps/views directory. Cygnite gives you flexibility to use either plain php layout or twig template engine. You are allowed to use widget to render partial views. Example-

```html 
<html>
    <body>
        <h1>Welcome to Cygnite Framework QuickStart </h1>
            Congrats !! You have created your first view page.
    </body>
</html>
```

You can also create template layout and render the sections into it. Cygnite Framework has built in support for most elegant and powerful twig template engine. For more details check [Views](http://www.cygniteframework.com/2013/08/views.html).

### Routing

Cygnite Framework has powerful routing features. You can use routerconfig.php inside your apps directory. Also you can write powerful rest api with simple closure object with routes.php. Cygnite Framework also gives you flexibility to call controller with parameters from your router directly.

Let us create our first routing.

Example -

1. For direct routing use **routerconfig.php**.

```php
    return array(
       '/profile/{:name} /' => 'user.profile',
    );
```

2. router.php has closure support to build powerful REST api. You can define all routes here.

```php
use Cygnite\Foundation\Application as App;
 
    $app = App::instance();

    // Before Router Middleware
    $app['router']->before('GET', '/{:all}', function() {
          echo "Welcome! to Cygnite PHP Framework";
    });

    $app['router']->run(function () 
    {
       // after routing filter
    });
```

For more details documentation please have a look at [Routing guide](http://www.cygniteframework.com/2013/08/routing.html).


### Displaying Data Into View

Let us see how to display data into view. If you are using plain php view page you should pass second parameter as to render function and if in the case of twig template engine you should be passing data into view using with().

```php
 // using php layout and view
  $this->render('welcome', array('name'=> 'Cygnite PHP Framework'));

// If you use twig template engine
  $this->render("welcome")->with(array(
                                    'author'=>$this->author,
                                    'email'=>'sanjoy09@hotmail.com',
                                    'messege' => 'Welcome to Cygnite Framework',
                                   ));
```

You need to assign values to view as by simply passing values to with function. You can get the data into the view (if plain php file used as view) or in layout as follows.

```php   
  echo $this->author;
  echo $this->email;
```

### Generating Crud

Cygnite CLI now makes your job much more simpler. You can create simple crud operation via terminal. For more details have a look at [Cygnite CLI - Crud Generator](http://www.cygniteframework.com/2013/11/crud-generator-by-cygnite-cli.html)