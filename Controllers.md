## Controllers

After determining which controller to use for the route request, your Controllers are responsible for handling request and producing the appropriate output to the user. Controllers does most of the magic work for you. It also fetch or save data from model and use a view page to render HTML output. Meaning that controllers are middleware between your model and views.

### Configuring Default Controller

You can set default controller of your application via configuration inside **apps/configs/application.php** file. For example-
```php
 return array(
        'default_controller' => 'Home',
        'default_method'     => 'index',
 );
```
You can even change the default action for your controller with above configuration.

### Controller

* 1. Controllers are stored in the **apps/controllers/** directory.

* 2. All controllers must have namespaces top of controller file and controller name must be prefixed with the Controller.

* 3. Every Controllers must extends **AbstractBaseController**.

* 4. Controllers may or may not have constructor and it should call parent constructor.

* 5. Every actions of controllers should be **prefixed with Action **after action name. Example- **indexAction**

* 6. You can import classes by using **use keyword** inside namespaces.

Now let us create a simple controller which render a view and see how it is works.

### Basic Controllers

```php
 namespace Apps\Controllers;

 use Cygnite\Foundation\Application;
 use Cygnite\Mvc\Controller\AbstractBaseController;

 class HomeController extends AbstractBaseController
 {
    // Plain layout
    protected $layout = 'layout.base';

    public function __construct()
    {
        parent::__construct();
    }

    public function indexAction()
    {
        $this->render('welcome', array( 'title'=> "Welcome to Cygnite Framework"));
    }
 }
```
Create your view page into **/apps/views/home/welcome.view.php** page to render it.

We are done ! Now we can call index action via url to view output.

```html 
http://www.example.com/cygnite/index.php/home/index
```

### Controller Naming Convention And URL

```html 
http::/www.example.com/cygnite/user/index
```

| Class Name                     | URL                                          |
| -------------                  |:-----------------------------:               |
| UserCategoryController	 | userCategory OR user_category                |
| UserCategoryListingController  | userCategoryListing OR user_category_listing |


| Action Name           | URL                          |
| -------------         |:----------------------------:|
| userDetailsAction	| userDetails OR user_details  |


### Url Rewrite

Cygnite Framework shipped with **.htaccess** to remove **index.php** from url. If you miss .htaccess in your installation than create one into your root directory of your project.

```apache
 Options +FollowSymLinks
 RewriteEngine On
 php_value safe_mode off
       
 RewriteCond %{REQUEST_FILENAME} !-f
 RewriteCond %{REQUEST_FILENAME} !-d
 RewriteRule ^(.*)$ index.php/$1 [L] 
  
 Order allow,deny
 Allow from all
```

You can now call same action as follows.

```html
http://www.example.com/cygnite/home/index
```

### Importing classes into controller

All classes are autoloaded, just use namespace to autoload any class in runtime. Since cygnite uses lazy loading concept, so you can import number of namespace, no need to worry about performance, class will get autoloaded when you create object of the class. 
For example -

```php
  namespace Apps\Controllers;

  use Cygnite\FormBuilder\Form;
  use Cygnite\AssetManager\Asset;
  use Cygnite\Validation\Validator;
  use Apps\Models\Product;
```
### RESTful Resource Controllers

Gives you flexibility for painless development. Resourceful REST controllers respond to various routing by registering single resource route declaration. In Cygnite, a resourceful route is a mapping between HTTP verbs and URLs to controller actions.

A single line of entry in the routing file does all magic behind. Let us register resourceful route to "**PhotoController**" controller as below.

```php
  use Cygnite\Foundation\Application as App;

  $app = App::instance();

  $app['router']->resource('photos', 'photo');
```

Above single entry creates seven different routes in your application and all route mapping to the Photo controller:

### Actions Handled By Resourceful Controller

| HTTP Verb    | Path            | Controller@Action   | Used For                      |
| -------------|:---------------:|:-------------------:|:-----------------------------:|
|  GET	       | /photos	 | photo@getIndex      | display a list of all photos  | 
|  GET	       | /photos/new	 | photo@getNew	       | return an HTML form for creating a new photo. | 
|  POST	       | /photos/	 | photo@postCreate    | create a new photo.           | 
|  GET	       | photos/{id}	 | photo@getShow       | display a specific photo identified by id.  | 
|  GET	       | photos/{id}/edit| photo@getEdit       | return an HTML form for editing a photo.  | 
|  PUT|PATCH   | photos/{id}	 | photo@putUpdate     | update a specific photo identified by id.  | 
|  DELETE      | photos/{id}	 | photo@delete	       | delete a specific photo.  | 


Resource controller actions are prefixed with the REST method name.

### Generating Resource Controllers

Are you lazy to create controller? Don't worry! Cygnite CLI generate basic or resource controllers on ease. Sounds interesting. Let us see how to generate controller.

```php
   php cygnite controller:create photo // will generate basic controller

   php cygnite controller:create photo --resource // will generate resource controller    
```
> Read more: [cygnite console](http://www.cygniteframework.com/2014/03/console-overview.html).

### Controller Action Events / Hooks

It is quite common in most of the web application development need some filters or hooks to be executed on the controller actions before or after the action executed. It allow you to write some logic to execute right before / action to be executed. In Cygnite this is achieved using Dispatcher. This Filters / Hooks will get activated only when you specify it into your controller.

```php
 namespace Apps\Controllers;

 use Cygnite\Mvc\Controller\AbstractBaseController as BaseController;

 class HomeController extends BaseController
 {
    // Plain layout
    protected $layout = 'layout.base';

    public function __construct()
    {
        parent::__construct();
    }

    public function beforeIndexAction()
    {
       // Write your logic. This method will execute before indexAction()
    }

    public function indexAction()
    {
        $this->render('welcome', array(
                                  'title'=> "Welcome to Cygnite Framework"
        ));
    }

    public function afterIndexAction()
    {
           // Write your logic. This method will execute after indexAction()
    }
}
```
### Dependency Injection & Controllers

You can also inject dependencies dynamically into controller almost with zero configuration. Cygnite has powerful technique to auto resolve all your constructor dependencies. For more details please have a look at Cygnite IoC Container.

### Constructor Injection

```php
 namespace Apps\Controllers;

 use Cygnite\Foundation\Application;
 use Cygnite\AbstractBaseController;
 use Apps\Extensions\Payment\PayPal\Api;

 class HomeController extends AbstractBaseController
 {

    // Api instance 
    protected $paypalApi;

    /**
     * Create a new controller instance.
     *
     * @param  Api  $paymentApi
     * @return void
     */
    public function __construct(Api $api)
    {
       $this->paymentApi = $api;
    }

    public function indexAction()
    {
       $this->paypalApi->makePayment();
    }
 }
```
>Read More: [Cygnite IoC Container](http://www.cygniteframework.com/2013/11/ioc-container.html)

### Using HMVC Widgetization: HMVC Request

[HMVC modules](http://en.wikipedia.org/wiki/Hierarchical_model%E2%80%93view%E2%80%93controller) are great way to separate main controller logic into multiple pieces. The common usages is when use theming or when you want to segregate the whole app into block of sections, where every block of sections does different set of tasks. Likewise you can produce widget output and also can create highly modular application, reusable modules.

You can call modular controller action as below.

```php
 // Call apps/modules/admin/controllers/UserController.php indexAction($id)
$response = $this->call('modules.admin.controllers.user@index', array('id' => $id));
```
### Using Widget Controller

Every HMVC controller also perform tasks as parent controller does. HMVC module views call widget and loading widget can be done as follows.

```php
 namespace Apps\Modules\User\Controllers;

 use Cygnite\Mvc\View\Widget;
 use Cygnite\Foundation\Application; 
 use Apps\Modules\Admin\Models\User;
 use Cygnite\Mvc\Controller\AbstractBaseController;

 class ProfileController extends AbstractBaseController
 {

    public function __construct()
    {
        parent::__construct();

    }

   public function indexAction($id)
   {
        $userProfile = User::findByProfileId($id);

        //apps/modules/user/Views/profile/user.view.php
        return Widget::make('modules::profile::user', array(
                                                      'msg' => 'Hello Widget', 
                                                      'profile_info' => $userProfile
        ));
   }
 }
```
That's all about the controllers.
