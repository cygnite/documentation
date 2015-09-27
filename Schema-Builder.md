###Introduction

Cygnite provides you a simple and flexible way to build your database scheme. Currently schema builder supports widely used open source database mysql. Schema builder reduce all your pain of writing raw queries. Cygnite Schema builder is also used for Database Migration.

###Create a Table

Closure object is used to create a new database table. Schema will create a Table if doesn't exists.

                  

```php
use Cygnite\Database\Schema; 

 Schema::instance(
 $this,
 function ($table) {
/**
 * By default schema will take your model name as table name,
 * Optionally you can also overwrite table name as below.
 *   
 */
 $table->tableName = 'users';

 $table->create(
        array(
        array('column'=> 'id', 'type' => 'int', 'length' => 11,
        'increment' => true, 'key' => 'primary'),
        array('column'=> 'username', 'type' => 'string', 'length' =>100),
        array('column'=> 'password', 'type' => 'string', 'length'  =>16),
        array('column'=> 'country', 'type' => 'string', 'length'  =>20),
        array('column'=> 'city', 'type' => 'string', 'length'  =>16, 'null'=> true),
        array('column'=> 'is_admin', 'type' => 'enum', 'length'  =>array('0', '1')),
        array('column'=> 'price', 'type' => 'decimal', 'length'  =>'10,2'),
        array('column'=> 'depth', 'type' => 'float', 'length'  =>'10,2'),
        array('column'=> 'created_at', 'type' => 'datetime'),
        array('column'=> 'updated_at', 'type' => 'datetime'),
        array('column'=> 'access_time', 'type' => 'time'),
        array('column'=> 'time', 'type' => 'timestamp',
        'length'=> "DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP"
        ),
       ),
       'InnoDB',
       'latin1'
      )->run();
    } 
 ); 

```
###Has Table

```php
 if ($table->hasTable()->run()) {
     echo "Table exists";
 }
```
###Column Exists

```php
   $table->hasColumn('user_details')->run();
```
###Renaming Table

Rename your table with below command.

```php
    $table->rename('users_category');
```
###Add A Column To The Table
```php

    $table->addColumn(
                'features',
                'varchar(100) NOT NULL'
            )->run();
```
###Add Column After

```php
    $table->addColumn(
                'description',
                'varchar(100) NOT NULL'
            )->after('features')->run();
```
###Dropping Single Column

```php
   $table->dropColumn('features')->run();
```
###Drop Multiple Columns
```php

    $table->dropColumn(array(
                      'features',
                      'user_detail',
                      'status'
                  )
              )->run();
```
###Adding Primary Key

```php
     $table->addPrimary('id')->run();
```
###Add Multiple Column As Primary Key

```php
    $table->addPrimaryKey(
                array(
                    'id', 'some'
                )
            )->run();
```
###Adding Unique Key
```php

    $table->unique('sunrise')->run();
```
###Add Multiple Columns As Unique Key
```php

$table->unique(array('depth','rise'), 'users')->run();
```
###Dropping Unique Key

```php
     $table->dropUnique('users')->run();
```
###Add And Drop Index Key

```php
    $table->index('is_admin')->run();
    $table->dropIndex('is_admin')->run();
```

###Drop Table

Drop your table from the database if exists with below command.

```php
if ($table->drop('users')->run()) {
    echo "Table dropped";
}
```

This is all about the Schema Builder. Simply run and enjoy building the table schema. You will find much more features in the next releases. Stay connected.