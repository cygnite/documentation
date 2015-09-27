###Getting the Input Instance

Accessing your post inputs are very easy and don't worry about security, Cygnite escape your string internally. Create Input instance to retrieve user input data. 
For Example :

 ```php


 use Cygnite\Common\Input\Input;

 $input = Input::make(function ($input) 
 {
      return $input;
 }); 
 
 Or 

 $input = Input::make(); 
```
    
###Checking Post Existence

Verify form posted or not using hasPost() method.

 ```php


  <input type="submit" name="btnSubmit" value="Save" />

  if ($input->hasPost('btnSubmit') == true)  {
    ................
  }  
```
 

 
###Getting All Post Values

###Retrieving particular field value

```php
    if ($input->hasPost('btnSubmit') == true)  {
        echo $input->post('name'); // Cygnite PHP Framework
    }
```
###Retrieving Field Array Values

 
```php

    if ($input->hasPost('btnSubmit') == true)  {
        echo $input->post('user.name'); // Sanjoy Dey 
    }
 
```
Above code equivalent of using $_POST['user']['name'];

###Getting Only Needed Input

You may don't want to get all the post values, except a field want to retrieve all post array. Then you may use except() method to escape field value from post values.

 

```php
 if ($input->hasPost('btnSubmit') == true)  {
     show($input->except('address')->post()); // Give you all post values except "address" field. 
 }     
```
###Checking Is Ajax Request

You may want to check if incoming request is AJAX request or not. You can achieve it as below.

 
 
```php
  if ($input->isAjax()) {
     
     // Ajax Request

  }
``` 
    
###Getting JSON Input

Sometimes ajax related application, we make use of json object to pass value into controller, in such case input post or get will not work. You can use json(); to get json object values.

 
 
```php
 $json = $input->json();
  
  echo $json->email;
```
    
###Cookie Manager

 Getting Cookie Instance

In the below example shown how to get cookie instance to manipulate cookies.

 
 
```php
  use Cygnite\Common\Input\CookieManager\Cookie;

  $cookie = Cookie::create(function ($cookie) 
  {
     return $cookie;                 
  });  

  Or 

  $cookie = Cookie::create(); 
 
```

###Setting Cookie

We can set cookies as below.

 
```php

 $cookie->setName('cygnite_cookie')
       ->setValue('Cygnite Framework Cookie')
       ->setExpire('+1 Days')
       ->setPath('/')
       ->setDomain('www.cygniteframework.com')
       ->setSecure(false)
       ->setHttpOnly(true);

 or

 $cookie = Cookie::create(function ($cookie) 
 {
     $cookie->setName('cygnite_cookie')
            ->setValue('Cygnite Framework Cookie')
            ->setExpire((time()+3600))
            ->setPath('/')
            ->save();

     return $cookie;
 });

```
###Verifying Cookie Existence

 

```php
 $cookie->has('cygnite_cookie');
```

###Getting Cookie Value Using The Key

We can verify if cookie is set using has() method and it will return boolean value. If returning true we can get the cookie value using get() method.

 

```php
 $cookie->get('cygnite_cookie');

```

###Destroying Cookie

Destroy unwanted cookies by name.

 

```php
  $cookie->destroy('cygnite_cookie');

 ```
 