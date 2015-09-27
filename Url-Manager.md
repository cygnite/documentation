##Url Manager

###Getting Url Segment

Get your url segments as below.

```php

 http://www.cygniteframework.com/projects/index.php/users/list/23
 http://www.cygniteframework.com/projects/users/list/23

 use Cygnite\Common\UrlManager\Url;

 echo Url::segment(2);

```
Cygnite Framework always counts from your front controller "index.php". It counts if index.php is available in url else from your root path. The above url segment will print "list" as result.

###Getting Base Path Of Your Application

Though there is an option to set base url of your application in apps/configs/application.php, but you don't require to set. Cygnite is intelligent enough to guess your application base url and set it up for you while booting. You can get application base path as below.



  echo Url::getBase();


It returns http://www.cygniteframework.com/projects/ as result.

###Redirecting To Another Page

You can redirect one page to another using below code.


```php
   Url::redirectTo('your_controller_name.your_action_name'); 
    
   or 

   Url::redirectTo('your_controller_name/your_action_name');

```
###Refresh/Reload the Page

You may want to refresh or reload current page. In such cases you can just pass second parameter as 'refresh' to same redirectTo() method to reload your page.


```php
 Url::redirectTo('users/names', 'refresh');
```

###Getting Referer Url

In some case you may want to check which page user visit from. You can find the referer url as below.


```php
   Url::referredFrom();
```

###Encoding Url

Use encode() function to encode your url.

```php
 use Cygnite\Common\UrlManager\Url;
 Url::encode('your-url-to-encode');
```
###Decoding Url

Use decode() function to decode your encoded url.

```php
 use Cygnite\Common\UrlManager\Url;
 Url::decode('your-url-to-decode');
```