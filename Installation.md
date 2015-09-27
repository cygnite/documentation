### Installation and Configuration  

No complicated **xml/yaml** configuration files. Simply install latest software via composer,configure your database connection in **apps/configs/database.php** and you are ready to start your awesome application.

### Installing Composer

Cygnite Framework is powered by [Composer](https://getcomposer.org/)for managing it's dependencies. So before using Cygnite please make sure you have composer installed into your system.

The best and recommended way to install Cygnite Framework is download [composer.phar](https://getcomposer.org/) to your root directory where you want to install or move composer move it to **usr/local/bin** to composer use globally in your system ([Linux Machine](https://getcomposer.org/doc/00-intro.md#installation-linux-unix-osx)). Windows users please [download composer and install composer setup](https://getcomposer.org/doc/00-intro.md#installation-windows).

### Installing Cygnite Framework

You may install Cygnite Framework either simply downloading skeleton application from [github repository](https://github.com/cygnite/cygnite-application) and issuing below command (Terminal / Command Prompt) from root directory.

```php 
cd /var/www/cygnite/
composer install
```
or install using composer create project. Composer installation is more convenient and recommended.

###  Using Composer Create-Project

Open your terminal (Command prompt for windows) and change to root directory, where you want to install Cygnite.

```php
composer create-project cygnite/cygnite-application cygnite --prefer-dist
       or
composer.phar create-project cygnite/cygnite-application cygnite --prefer-dist
```

The above command will create a skeleton project into the root directory. Congrats! you are ready to build your awesome application.

### Via Manual Download

If you are experiencing trouble with composer installation you can also simply download the latest framework from below github repository.

[Download Cygnite PHP Framework](https://github.com/sanjoydesk/cygniteframework/)

### Server Requirements

The Cygnite Framework has a very few system requirements:

* # PHP >= 5.3.8
* # MCrypt PHP extension
* # PDO extension must be enabled
* # PHP CLI must be enabled for Cygnite CLI commands





