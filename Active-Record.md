## Active Record

* # Query Builder
* # Create
* # Read
* # Update
* # Delete
* # Using Where Clause
* # Aggregate Functions
* # Joins
* # Using Alias
* # Skip Columns
* # Collections
* # Dynamic Finders
* # Raw Expressions
* # Model Events
* # Transactions

### Introduction

Cygnite provides you beautiful ActiveRecord implementation. Each "Model" class acting as database table which is used to interact with database table for manipulation.

Before digging into ActiveRecord, be sure to configure a database connection in **apps/configs/database.php**.

### Basic CRUD

[CRUD defined by Wikipedia](http://en.wikipedia.org/wiki/Create,_read,_update_and_delete):

CRUD is the day to day task for every developers. Writing queries and running against the database may be painful at some point of time for developers, where Cygnite Framework takes your pain and provides you convenient way of running database queries using ActiveRecord. Cygnite makes your Create, Read, Write, Delete operations extremely simple and expressive.

### Create or Save Records Into Database

This is where Cygnite makes your job so simple, it follows activerecord style. Every model class act as a database table object. Save records into a table using instance of your model. Here we register a new user for our blog by simply instantiating a object of Users model and finally save it into a table.
```php
 use Apps\Models\User;

 $user = new User();
 $user->name = 'Sanjoy Dey';
 $user->email = 'sanjoy09@hotmail.com';
 $user->country = 'India';
 $user->gender = 'Male';
 $user->save();
```
The above code is equivalent of writing.

```php
#sql => INSERT INTO `users` (name, email, country, gender) VALUES('Sanjoy Dey', 'sanjoy09@hotmail.com', 'India', 'Male');
```
Isn't much easier to create a record into the database.

We recommend you to have auto increment primary field name as 'id'. Get the last inserted id by your user object "**$user->id;**";
### Setting Attributes To Model

You may also set attributes directly into the model using setAttributes method.
```php
 use Apps\Models\User;

 $user = new User();
 $user->setAttributes($input->except('btnSubmit')->post());
 $user->save();
```

### Getting Last Inserted Id

```php
  echo $user->id;
```
### Read or Select From Table

Retrieving rows from a table is very easy and simple by using Cygnite Dynamic finders and ActiveRecord.

## Query Builder: Basic Select Query

Select a particular column or all columns of the table with single line of code.
```php
 $data = $this->select('name,email,country')->findAll();

 or

 $user = new User;

 $data = $user->select('*')->findAll();
```

## Using Where Clause

Single where method does multiple tasks. For example.

### Using Single Where condition
```php
  $user->where('id', '=', '3');

  #sql => WHERE `id` = '3';
```

### Using multiple where conditions with AND

```php
 //Where with AND conditions
 $user->where('id', '>', '3')
      ->where('name', 'LIKE', '%Sanjoy');

 #sql => WHERE `id` > 4 AND `name` LIKE '%Sanjoy';


 $user->where('created_at', '>=', '2015-02-24 05:20:00')
      ->where('updated_at', '<=', date('Y-m-d H:m:s'));

 #sql => WHERE `created_at` >= '2015-02-24 05:20:00' AND `updated_at` <= date('Y-m-d H:m:s'));
```
### Using Where In
```php
  $user->where('id', 'IN', "2,3,6");

  // Or use whereIn()
 
  $user->whereIn('id', 'IN', "2,3,6");

  #sql => WHERE `id` IN ('2', '3', '6');
```

### Using orWhere
```php
  $user->orWhere('name', '!=', "shane");

  #sql => OR WHERE `name` != 'shane';
```

### Using orWhereIn
```php
  $user->orWhereIn('id', "2,3,6");

  #sql => OR WHERE `id` IN ('2', '3', '6');
```

### Using Distinct
```php
  $user->distinct('user');
```

### Using Order By
```php
$user->orderBy('id','DESC');
```
### Using Group By

```php
  $this->groupBy('name'); // single column
```
### Group By with multiple fields
```php
$this->groupBy(array('name','id')); // multiple column
```

### Limit Clause
```php
  $user->limit(3);
  #sql => LIMIT 0,3

  $user->limit(1,3);
  #sql => LIMIT 1,3
```

### Using Aggregate Functions
```php
  $user->select("name, COUNT(id) AS total, SUM(id) AS sum");
  #sql => SELECT name, COUNT(id) AS total, SUM(id) AS sum FROM `user`

  // OR use selectExpr() to use expression inside select 

  $user->selectExpr("name, COUNT(id) AS total, SUM(id) AS sum");
  #sql => SELECT name, COUNT(id) AS total, SUM(id) AS sum FROM `user`
```

## Using JOIN

Active Record allow you to query against database using multiple join conditions. Active Record uses fluent query builder to build join query for your table. Various join supported by active record are JOIN, LEFT JOIN, INNER JOIN, LEFT OUTER JOIN, RIGHT OUTER JOIN, FULL OUTER JOIN and raw JOIN queries.

 ```php
 use Apps\Models\Product;

 $product = new Product();

 $data = $product->select('product.name, category.id')
                 ->join('category', array('category.id', '=', 'product.category_id'))
                 ->where('product.id', '>', '295')
                 ->orderBy('product.id', 'DESC') 
                 ->limit(10)
                 ->findAll();  
```

### Using LEFT JOIN
```php
 $data = $product->select('product.name, category.id')
                 ->leftJoin('category', array('category.id', '=', 'product.category_id'))
                 ->findAll();  
```

### Using INNER JOIN
```php
 $data = $product->select('product.name, category.id')
                 ->innerJoin('category', array('category.id', '=', 'product.category_id'))
                 ->findAll();  
```
### Using LEFT OUTER JOIN
```php
 $data = $product->select('product.name, category.id')
                 ->leftOuterJoin('category', array('category.id', '=', 'product.category_id'))
                 ->findAll();  
```

### Using RIGHT OUTER JOIN
```php
 $data = $product->select('product.name, category.id')
                 ->rightOuterJoin('category', array('category.id', '=', 'product.category_id'))
                 ->findAll();  
```

### Using Raw JOIN Queries
```php
 //->rawJoin('raw query', array('table1.id', '=', 'table2.id'), 'alias name');

 $data = $product->select('product.name, category.id')
                 ->rawJoin('JOIN (SELECT * FROM category WHERE category.id = $id)',
                           array('p.id', '=', 'c.id'), 'c'
                 )
                 ->findAll();  

 show($data);
```

### JOIN Query Using Table Alias

```php
 $data = $product->select('p.name, c.id')
                 ->tableAlias('as p')
                 ->join('category', array('p.id', '=', 'c.id') , 'c')
                 ->where('p.id', '>', '295')
                 ->groupBy(array('p.type'))
                 ->orderBy('p.id', 'DESC') 
                 ->limit(10)
                 ->findAll();
```

### Skipping Columns

Cygnite Active Record also allow you to fetch data from database except columns. You just need to define a function into your model class as below.
```php
 public function skip()
 {
     return array('created_at', 'updated_at');
 }
```

 Collections

By default Active Record fetch data as "**Collection**" object. Cygnite Collection provide you flexibility to modify data as **Array, Json, ArrayIterator**. Also Collection makes easier to get the count, serialize, unserialize data on the fly.
```php
 $data->asArray();
 
 //or

 $data->asJson();

 //or

 $data->count();

 //or

 $data->getIterator();

 //or

 $data->serialize();

 //or

 $data->unserialize();
```

Collection object allow you to call "Model" functions directly.

### Dynamic Finders

Cygnite actively follows ruby on rails types of Active Record pattern and its dynamic finders made it more expressive. Dynamic finders are fully model based, these provides quick and easy way to access your data from table. Read More: Dynamic Finders

### Raw Expressions and Queries

Occasionally, though hopefully rarely, you may need to do specify some queries by hand or Select expressions. Query builder also allow you to write raw queries against your database connection. You can use **selectExpr()**, **rawJoin()** etc. method for raw expressions. You can also check Raw Queries for running raw queries.

### Model Based Events: Hooks

The following events are available, just define the method of the same name in the model that you want to use them:

* - beforeCreate
* - afterCreate
* - beforeUpdate
* - afterUpdate
* - beforeSelect
* - afterSelect
* - beforeDelete
* - afterDelete

```php
namespace Apps\Models;

use Cygnite\Database\ActiveRecord;

class User extends ActiveRecord
{

  protected $database = 'cygnite';

  protected $primaryKey = 'id';

  public function __construct()
  {
     parent::__construct();
  }

  public function beforeCreate()
  {
     //....
  }

  public function afterCreate()
  {
     //....
  }
}
```  

### Result-Set Types

By default Cygnite ActiveRecord will return you the results in the form of Collection object of your model class. You are also allowed to modify results as group, both, json, assoc, object, column, class etc. You can make use of **findAll()** method by passing parameter into it to modify result set as below. let

For example:
```php
 // get the result set as json collection
 $user->select('*')->findAll('json');
```

### Updating Record In A Table

We access model instance to update the records in a table. It's very simple than you think. It works similar as INSERT/CREATE works.
```php
use Apps\Models\User;

 $user = User::find(20);

 $user->first_name = 'Shane';
 $user->last_name = 'Watson';
 $user->email_address = 'shanewatson@gmail.com';
 $user->gender = 'Male';
 $user->save();
```
You will still have last id ```php echo $user->id; //20 ```

## Deleting Records

### Deleting a record from the Table

```php
 use Apps\Models\User;

 $user = new User();
 $user->trash(23);
```

### Deleting multiple records from the Table
```php
$user->trash(array(21,22,23,34), true);
```

### Accessing PDO methods Statically

Cygnite model provide you flexibility to access all [PDO](http://php.net/manual/en/book.pdo.php) method statically. So you can use the power of PDO using model class. 
For example-
```
 PDO::prepare() can be called as YourModel::prepare($sql);
 PDO::query() can be called as YourModel::query($sql);
 PDO::beginTransaction() etc.
```

### Using Transactions

Cygnite Framework database manipulation is completely model based. You can use transaction with simple syntax using your model.
```
 use Apps\Models\User;

 User::beginTransaction();

 try {
   
   $user = new User();
   $user->name = 'Clinton';
   $user->email = 'oscar@gmail.com';
   $user->save();

   User::commit();

 } catch (Exception $e) {

   User::rollback();
   User::close();
   throw $e;
 }
```
Isn't it simple? You may also have a look at [Cygnite Finders](http://www.cygniteframework.com/2013/11/dynamic-finders.html) for finding records from database.