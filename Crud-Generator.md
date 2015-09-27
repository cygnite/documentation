## Scaffolding - Code Generator

### Introduction

Generating crud may be painful using other framework, though there may be sample but still you need to write bunch of code to perform crud operations. Cygnite CLI makes it amusing. Cygnite CLI built on proven **Symfony2 console** component. Single command will generate necessary code for crud application with form validation, pagination etc. You may alter the code to make it fit to your application.

### Setting Up Database Connection

Before running command make sure you have configured database connection into the **apps/configs/database.php**. You may have a look at Database Configuration if you are not before.

### Changing To Application Directory

Open a terminal/command line window, and execute below command to change directory, then run generate command to generate your sample crud application.

```php
cd /var/www/cygnite/console/bin
```

### Running Generate Command
```php
php cygnite generate:crud your_controller_name your_table_name your_database_name
```

If you want to run command against your default database connection then you can simply remove database_name from the above command as below.

```php
  php cygnite generate:crud your_controller_name your_table_name
```

You can remove database name from the command as above, by doing that cygnite will identify default database connection from your configuration file **apps/configs/database.php** to generate crud. Above single line of command make you available sample crud operation against your table, pagination, validation, plain php layout with views etc.

### Generating Crud for Twig Template

Cygnite Framework also allow you to generate twig template and views, which require you to add a additional parameter in the command.
```php
 php cygnite generate:crud your_controller_name your_table_name your_database_name --template
```

### Sample Application

Let us create a sample application using generate command. We will generate CRUD for -

* i. Controller - user
* ii. Model (Table) - user
* iii. Database - social_network

```php
 php cygnite generate:crud user user social_network
```

Controller, Model, Views, Form, Fields Validation, Pagination etc. all these cool features requires you to enter a simple generate command.

>[Note: Remember your controller name and table name should be underscore prefix.]

### Running The Application From Browser

Let's us enjoy our application simply browsing URL.

```php
   http://localhost/cygnite/user
```

![Cygnite Framework: CRUD generator](http://4.bp.blogspot.com/-XDLhjdL9rF8/VQ6Vop58GNI/AAAAAAAAAwg/hGiohszwsLA/s1600/cygnite-framework-crud-generator.png)