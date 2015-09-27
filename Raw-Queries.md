##Database Raw Queries
###Database Driver Supports By Cygnite

Cygnite Framework query builder currently supports the well know MySql database by default. Since we are using PDO as database abstraction layer, you can also write PDO queries to handle other databases. We are actively working on ActiveRecord to support other databases as well.

###Write raw queries

Cygnite Framework gives you flexibility to write your own raw queries alongside the query builder. Setup your database configuration in apps/configs/database.php file. You need to provide your database connection name and primary key in model, Let ActiveRecord to know against which database you want to run queries. Let us see now how to fetch data using raw queries.

  ```php 
 // Return mulltiple users from table
 public function getUsers()
 {
     return $this->query("SELECT * FROM `guest_book`")->getAll(\PDO::FETCH_ASSOC);
 }

 public function getUser()
 {
     return $this->query("SELECT * FROM ".$this->tableName)->fetch();
 }

 public function save()
 {
     return $this->query("Your INSERT query")->execute();
 }
```
###Call Function Using Model Instance

```php
 use Apps\Models\Users;

 $users = new Users();
 $users->getUsers();
```
o need to worry Cygnite will autoload all your models in runtime, so just create an object and concentrate on doing your application stuffs. Similarly you can execute your insert, update, delete queries using query() function.

You can also take an advantage of all PDO methods by using $this->pdo inside your model function and run queries.