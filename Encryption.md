##Encryption Class

###Configuration

Set your encryption key in **apps/configs/application.php file. By default encryption key set in config, you can also change.

```php
 return array(
     'cf_encryption_key'  => 'cygnite-shaXatB.............59c4mrez',
 );

```
>[Note: You must have mcrypt extension installed in your system to use encryption and session library.]    

###How to encrypt sting ?

Cygnite Framework have built-in mechanism to make your data secure. You can able to encrypt your string as below.

```php

   use Cygnite\Common\Encrypt;

   $crypt = new Encrypt;
   $encryptedString = $crypt->encode("Encrypt my string as secure format");

```
###How To Decrypt Encoded String ?

Decrypt your encrypted string as below.

```php

    use Cygnite\Common\Encrypt;

    echo $crypt->decode($encryptedString);
```

Encryption class is also used to encrypt session details to keep your session secure, it is depended on the **salt key** provided in **apps/configs/application.php** file.