##Security Manager

###Security

The aim of Cygnite Framework is to make your application secure. You do not need to worry, all your POST, GET, COOKIE values are sanitize internally. Everything has been configured for you out of box.

###Sanitize Your String

Cygnite Framework sanitize, and do cross site filtering (XSS) recursively with single method **sanitize()**


```php
    use Cygnite\Common\Security;

    $security =Security::instance(
       function ($instance) {

            $instance->sanitize("Your Data to Sanitize");

            return $instance;
       }
    );
 
```
If you particularly want to remove javascript protocols from your string.


```php
   $security->removeJavaScriptProtocols();
```