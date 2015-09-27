## Views

### What is Views ?

Views are just web pages where your headers,menus,sidebar,contents and footers resides. Since we follow MVC patterns, views neither called directly nor passed any parameters into it, views can be called only via controllers.

In controllers section you found how to create controller and now lets create views and render it via controller.

### Use Plain PHP or Twig Template

View are reside into your **apps/views/controller-name-as-directory/your-view-file**. Now use pain php or use twig templates to create most elegant, awesome view page. Cygnite Framework has built in support of plain php layouts and twig template engine. Cygnite Framework shipped with sample view page using plain php and also with twig template layouts. Layouts gives you more flexibility to extends its blocks.

## Simple view page

### Using Plain PHP :

If you are using plain php as View then your filename should have prefix .view (**welcome.view.php**).

### Using twig template:

Your twig template page should have prefixed with .html.twig (**index.html.twig**).

You need to activate template engine into your controller in order to use twig template engine.

```php
namespace Apps\Controllers;

use Cygnite;
use Cygnite\AbstractBaseController;

class HomeController extends AbstractBaseController
{
    //protected $layout = 'layout.main';//By default it will call base layout

    protected $templateEngine = true;

    // you can change default template extension here
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
Render view page by render function, for example-

```php
   //apps/views/home/welcome.view.php
   $this->render('welcome', array('name' => 'Cygnite Framework'));// plain php view page 

   //apps/views/home/index.html.twig
   $this->render('index')->with(array('name' => 'Cygnite Framework'));// Twig template view 
```
### Render view with Twig Layout

Cygnite Framework has built in support of beautiful, elegant twig template engine. Render your view page with layout.
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
You can find more detail Documentation of Twig Template

### Rendering view with PHP Layout

Cygnite Framework gives you flexibility to render layout template using plain php. You can change layout accordingly. Let us see how to render template using controller. For example.
```php
namespace Apps\Controllers;

use Cygnite\Mvc\Controller\AbstractBaseController;

class CategoryController extends AbstractBaseController
{
    // Plain php layout views/layout/base.view.php
    protected $layout = 'layout.base';

    public function __construct()
    {
         parent::__construct();
    }

    /**
    * Your index view page will render into the base layout
    */
    public function indexAction()
    {
         $category = array();         
         //apps/views/category/index.view.php
         $this->render('index', array(
               'category' => $category,
               'title' => 'Cygnite Sample Layout'     
         ));
    }
}
```
### Plain PHP Layout page

Let us create base layout to render it content into it.

```php
<?php
  use Cygnite\AssetManager\Asset;
  use Cygnite\AssetManager\AssetCollection;
  use Cygnite\Foundation\Application;
  use Cygnite\Common\UrlManager\Url;

   $asset =  AssetCollection::make(function($asset)
   {
          $asset->add('style', array('path' => 'assets/twitter/bootstrap/css/bootstrap-theme.min.css'))
                ->add('style', array('path' => 'assets/css/cygnite/table.css'))
                ->add('style', array('path' => 'assets/js/tablesorter/css/theme.default.css'))//Pick a theme, load the plugin & initialize plugin
                ->add('style', array('path' => 'assets/css/cygnite/style.css'))
                ->add('script', array('path' => 'assets/js/cygnite/jquery.js'))
                ->add('script', array('path' => 'assets/js/custom.js'))
                ->add('script', array('path' => 'assets/twitter/bootstrap/js/bootstrap.js'))
                ->add('script', array('path' => 'assets/js/tablesorter/js/jquery.tablesorter.min.js'));

       return $asset;
   });

?>

<!DOCTYPE html>
<html>
    <head>
        <title><?php echo $this->title; ?></title>
        <?php $asset->dump('style');// Style block ?>
    </head>
    <body>
        <div class='container'>
            <?php echo $yield;//your content block ?>
        </div>
        <?php
        //Script block. Scripts will render here
        $asset->dump('script');
        ?>
        <style type="text/css">
            tr:hover { background-color: #4DC7EB !important; }
        </style>
    </body>
</html>
```

Your view page will render into the above layout.

Well! We are done. Your view page will render into $yield; If you are still wondering or lazy to create layout/views, then you can make use of Cygnite Crud generator. Also you can find sample code shipped with skeleton application.

For more details about rendering assets into the view page have a look at AssetManager.

### Accessing data into php view or layout page

You can access your data into your view or layout page as object.
```php
 echo $record; // You can directly access controller properties into view page.
 echo $title; // implemented in v1.3.2
```
### Using Widget

You can also use of widget either directly into views or HMVC modules to make your views or application much more modular and create many block section to perform set of task.

### Rendering Widget Into HMVC Modules

```php
 use Cygnite\Mvc\View\Widget;

 // Using Widget into the modules
 $widget = Widget::make('admin:user', function ($widget)
 {
    $widget->isModule(true);
    return $widget->render();// will look into apps/modules/admin/Views/user.view.php

 }, array('msg' => 'Hello! Widget', 'userId' => $id)); 
```

 or 
```php
 $widget = Widget::make('admin:user', function ($widget)
 {
    // will look into apps/modules/admin/Views/user.view.php
    return $widget->render(true); // If you pass true Widget will understand you are 
                                  // trying to access module widget
 }, array('msg' => 'Hello! Widget', 'userId' => $id)); 
```
```php
 $widget = Widget::make('admin:user', function ($widget)
 {
    // will look into apps/views/user.view.php
    return $widget->render();

 }, array('msg' => 'Hello! Widget', 'userId' => $id)); 
 
 echo $widget;
```
 
If you are looking for much modular blocks into your view then use HMVC modules, access via parent controller.

### Rendering Widget Without Parameter
```php
 //It will look into apps/views/home/welcome.view.php
 echo Widget::make('home:welcome');
```
### Accessing Parameters Into Widgets

You may need to access some parameters into your widget as below.

```php
 $widget = Widget::make('admin:user', function ($widget)
 {
    return $widget->render(true); // If you pass true Widget will understand you are trying to access widget

 }, array('greet' => 'Hello! Widget')); 

 echo $greet; //Access your params in your widget view page 
```
That's all about views.