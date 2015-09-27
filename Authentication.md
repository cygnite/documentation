##Authentication Manager

###Registering Authentication Manager

Make sure Auth manager is registered into cygnite autoloader. You can find **/apps/configs/autoload.php** where you need to add the authentication manager namespace in order to autoload components. For example.

```php
   namespace Cygnite;

   Application::load()->registerDirectories(
    array(
        'apps.components.authentication.auth'
        ........
    )
  )
 ```
###Creating A Model Class For Authentication

You also required a model class in order to authenticate users. Let assume you are using "User" model to authenticate. Sample model class below.

```php
namespace Apps\Models;

use Cygnite\Database\ActiveRecord;

class User extends ActiveRecord
{
    //your database connection name
    protected $database = 'cygnite';

    // your table name here
    //protected $tableName = 'user';

    protected $primaryKey = 'id';

    public function __construct()
    {
        parent::__construct();
    }
}
```
###Setting Model For Auth Manager

By default, Cygnite includes an **apps/models/User.php** model in your apps directory. You need to provide model namespace, auth manager to identify the model.


```php
 $auth = Auth::model('\Apps\Models\User');

```
###Authenticating users
By default Cygnite shipped with AuthController for authentication out of box. AuthController helps user to authenticate and login into the application.

Verify User Credentials Without Login

Some time you may not want to login user but need to verify user credentials against database to perform some action. In such cases you can use verify method to validate.


```php
 $credentials = array(
       'email' => $email, 'password' => $password 
 );

 if ($auth->verify($credentials)) {

   //......

 }

```
###Logging In A User / Create User Session

If user credentials are verified and you would like to set current user session you can simply use login method.


```php
 $credentials = array(
       'email' => $email, 'password' => $password 
 );

 if ($auth->verify($credentials)) {

    $auth->login(); // will login current user and create session
 }

```
###Logging In A User Directly

If you like to validate and login user directly then you can also make use of login method. This method is equivalent to verify using credentials.

```php

 $credentials = array(
       'email' => $email, 'password' => $password 
 );

 if ( $auth->setCredential($credentials)->login() ) {

    //........
 }

```
###Logging In A User With Conditions

You may also want to add extra conditions to authenticate user. Some application may also need to validate if user is active currently to login.


```php
 $credentials = array(
       'email' => $email, 'password' => $password, 'status' => '1'
 );

 if ( $auth->credential($credentials)->login() ) {

    // User is active user and exists
 }

 Or

 // In this case your authentication table should have 'username' 'password' 'status' columns
 if ( $auth->credential($username, $password, $status)->login() ) {

    // User is active user and exists
 } 
```

###Getting Current User Information

You may need to get the user session information to do some action.


```php
 $userInfo = $credentials = array();

 $credentials = array(
       'email' => $email, 'password' => $password 
 );
 
 if ($auth->verify($credentials)) {

    $auth->login();
    $userInfo = $auth->userInfo(); // Get the current session informations
 }

```
###Check If A User Is Authenticated

You may use isLoggedIn method to determine if user already logged into the application.

```php
 
 if ($auth->isLoggedIn()) {

    // User already logged in and session exists
 }
```

###Log Out Current User

You may use logout method to logout from the application.

```php
 
 $auth->logout();// Logout and redirect to base url

 $auth->logout(false); // logout but won't redirect to baseurl. 
```