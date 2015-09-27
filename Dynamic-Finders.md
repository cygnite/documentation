###Dynamic Finders

Cygnite actively follows ruby on rails types of Active Record pattern and its dynamic finders makes it more expressive.

###Using Dynamic Finders

Dynamic finders are fully model based, these provides quick and easy way to access your data from table. The finder make use of [__callStatic()](http://in2.php.net/manual/en/language.oop5.overloading.php#object.callstatic) to invoke undefinded static methods via your model. Your model is the access point of dynamic finders. You may wonder by looking at the static syntax of finder methods, but all finder methods are invoked via instance of your model.

###Fetch All Data From Table

Consider your model called Category.
```php

namespace Apps\Models;

use Cygnite;
use Cygnite\Database\Schema;
use Cygnite\Database\ActiveRecord;

class Category extends ActiveRecord 
{
     //your database connection name
     protected $database = 'cygnite';

     protected $primaryKey = 'id';

     public function __construct()
     {
         parent::__construct();
     }
 }
```
###Fetch All Records From Using Model.

```php

 Category::all();
```

Equivalent of writing sql query

```php

 #sql => SELECT * FROM category;

```
You may optionally pass Order into all() method


```php
   Category::all(array(
                   'orderBy' => 'id desc')
                );
```
###Find All Records Except Columns

Don't want to fetch all columns values? It is very simple, just provide column names in an array inside exceptColumns() method. For example:

```php
 namespace Apps\Models;

 use Cygnite\Database\ActiveRecord;

 class Category extends ActiveRecord 
 {
     //your database connection name
     protected $database = 'cygnite';

     protected $primaryKey = 'id';

     public function __construct()
     {
         parent::__construct();
     }

     // Fetch all except
     public function exceptColumns()
     {
        return array('description', 'updated_at');
     }
 }

 
 $category = Category::all();


Cygnite ActiveRecord will fetch all columns as object except `description` and `updated_at`.
```
###Find First Row

Get the first record from database by your model object.
```php

    $category = Category::first();
    echo "The first category is :".$category->id;
```
###Find Last Row

Get the last record from database by your model object.

```php

    $category = Category::last();
    echo "The last category is :".$category->id;
```
###Find By Primary Key

Select a single record using primary key.

                  
```php
Category::find(20);
#sql => SELECT * FROM `category` WHERE `id` = 20;

```
###Find Records By Column

Cygnite Dynamic finders also allow you to find records by column.

```php
Category::findByName('application');

#sql 1=> SELECT * FROM `category` WHERE `first_name` = 'application';

Category::findByEmailAddress('sanjoy09@hotmail.com');    

#sql 2=> SELECT * FROM `category` WHERE `email_address` = 'sanjoy09@hotmail.com';
```
>[Note: Your table fields should be prefixed with underscore and while accessing dynamic finders it should be StudlyCaps. findBy...() followed by column name. We are adding more features into finder methods to make your job much simple.]  

###Find Records By Multiple Columns (Using AND Conditions)

```php
Category::findByUserNameAndAuthorAndEmpNumber(array('John', 'Sanjoy Dey', 'Emp10064'));

#sql 1=> SELECT * FROM `category` WHERE `user_name` = 'John' AND `author` = 'Sanjoy Dey'
AND `emp_number` = 'Emp10064';
```
You can add multiple columns with AND conditions using dynamic finders.

###Find Records By Multiple Columns (Using Or Condition)

```php

Category::findByUserNameOrIsAdminOrEmpNumber(array('Admin', 'Yes', '232');

#sql 1=> SELECT * FROM `category` WHERE `user_name` = 'Admin' Or `is_admin` = 'Yes' 
Or `emp_number` = '232';

```
###Find Records By Sql Query

```php

Category::findBySql("SELECT `category_name`, 'sub_category', 'description' FROM `category_master`");
````