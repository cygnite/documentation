##Session Manager

###Configuration

The session configuration is stored in **apps/configs/session.php** file. By default Cygnite uses PHP native session to store data, you can also make use of database based session simply by configuration.

###Usage: Including Namespace

```php
 use Cygnite\Common\SessionManager\Session;

```
###Storing Into Session

Storing a value into session is just simple. Example given below.


```php
   use Cygnite\Common\SessionManager\Session;

   Session::set('name', 'Peter'); // Will store the name into session

```
When you call method to store session manager will create factory object of the driver configured into apps/configs/session.php and call corresponding method to perform operation.

###Retrieving A Value From Session


```php
    use Cygnite\Common\SessionManager\Session;

    echo Session::get('name'); // Will Display Peter

```
###Checking Existence Of Value In Session

Check if key exists into session as below.

```php

    use Cygnite\Common\SessionManager\Session;

    if (Session::has('name')) {

        echo Session::get('name');
    }

```

 ###Removing A Value From Session

Remove a particular value from session


```php
    use Cygnite\Common\SessionManager\Session;

    Session::delete('name');

```
###Destroy All Session Values


```php
    use Cygnite\Common\SessionManager\Session;

    Session::delete();

```
###Setting Flash Message

Sometime you may need to set some flash message before redirecting to the page, and after landing to the page you would like to display it into your browser, in such cases you can simple store values into flash message. Flash message is useful for processing a form, redirect and have a special message shown on the next request. Below example shows how you can use flash message.

```php
    use Apps\Models\Product;

    $product = new Product();

    $product->name = 'Cygnite';
    $product->description = 'Awesome Product';

    if ($product->save()) {

       $this->setFlash('success', 'Product saved successfully!')
            ->redirectTo('product/index/'.Url::segment(4));

    } else {

       $this->setFlash('error', 'Error occured while saving product!')
            ->redirectTo('product/index/'.Url::segment(4));

    }

```
Above example you can see after saving product into database, controller set flash message and then redirect.

###Rendering Flash Message

In the view page for the next action below code used to check notice message exists and then display to user.

 
```php
  if ($this->hasFlash('success')) {

     echo $this->getFlash('success');

  } else if ($this->hasError()) {

     echo $this->getFlash('error');

  }
```