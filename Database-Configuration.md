## Database Configuration

### Configure Your Database Connections

Cygnite makes your database configuration extremely simple. All connections are lazy loaded by model object. You can setup your database connections in **/apps/configs/database.php**.
For Example :
```php 
 namespace Cygnite\Database;

 Configuration::initialize(
    function ($config) {
        $config->default = 'db';
        $config->setConfig(
            array(
                'db' => array(
                    'driver' => 'mysql',
                    'host' => 'localhost',
                    'port' => '',
                    'database' => 'cygnite',
                    'username' => 'root',
                    'password' => '',
                    'charset' => 'utf8'
                )
                /*'db1' => array(
                    'driver' => 'mysql',
                    'host' => 'localhost',
                    'port' => '',
                    'database' => 'directory',
                    'username' => '',
                    'password' => '',
                    'charset' => 'utf8'
                )*/
            )
        );
    }
 );
```
In the above configuration the **"db"** is the default connection. Don't worry about the application performance, all connections are lazy loaded, until you create an object of model class, the connection will not be active. Enjoy working with an ease using Cygnite's ActiveRecord.