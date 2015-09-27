##Pretty Exception Handler

###Introduction

By default Cygnite uses beautiful & powerful [Tracy debugger](http://nette.github.io/tracy/) for debugging and handling exceptions. By default debugger is turned off to "production" mode, however you are allowed to enable the debugger. In order to build ajax based application or in the production server make sure you are turning debugger off to "production" mode.

###Activating Debugger

By default Cygnite uses production environment for the development. You can change the environment as per development mode. If you open **/apps/configs/application.php** you can see "environment" set to "production", you can simply change it to "development" to enable debugger. It will display stack trace and pretty exceptions.

```php

return array(
    'environment'  =>  'production',//or development
);

```
All exceptions are handled by the Cygnite\Exceptions\Handler class. This is the wrapper class of Tracy debugger.

>[Note: We strongly recommend to turn off debugger in production server.]  

###Logging Exceptions

By default Cygnite has out of box configurations for you, you may also change the configuration based on your needs. You can simply turn on logging by setting "enable_logging" as true in your **apps/configs/application.php** Default log path is defined as well as email address to trigger email if any exception occurs on production mode. If you open "application.php" file you can see as below.

```php

   'enable_logging'          => false,

   'log_path'                => 'apps.temp.logs',

   'enable_error_emailing'   => false,

   'log_email'               => 'xyz@gmail.com', // email address where to send exception report

```

All your exceptions will be logged into apps/temp/logs/ directory with with date, time and information. You can also enable log error emailing as above example.

###Dumping Variables Into Debug Bar

As like firebug console you can also dump your data into debugger panel. For example some sql query or data you would like to dump into debug bar as below.

```php

 use Cygnite\Exception\Handler;

 Handler::dump('Select * From User', 'SQL'); // Dumping Sql

 Handler::dump(array('foo' => 'bar'), 'bar'); // Dumping Data into bar
```