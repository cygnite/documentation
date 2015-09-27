###Overview

Create your database version control using Cygnite Migration commands. The Migration commands gives you convenient way to alter your database schema over time and stay you up-to-date. In most of the database driven application we often need to keep track of database schema changes, similar thing as we do with our source code (with git, svn etc.). For example once your application is in production server, you may want to add a new column or add a index to the table. This is where Cygnite Migration tool comes to action, now you can keep track of your alteration with Cygnite Migration tool.

Cygnite migration tool is inspired by [Ruby on Rails migration](http://edgeguides.rubyonrails.org/active_record_migrations.html). Migrations are coupled with Cygnite Schema Builder to manage your database schema.

###Creating a Migration

Create your first migration with migrate:init command.

```php
  G:\wamp\www\cygnite\console\bin>   php cygnite migrate:init users
```
The above command will generate class with timestamp prefix into apps/database/migrations/xxxx_users.php but nothing into it. As follows

```php
 use Cygnite\Database\Schema;
 use Cygnite\Database\Migration;

 class Users extends Migration
 {
  /**
   * Run the migrations up.
   * @return void
   */
   public function up()
   {
          //Your schema to migrate

   }

  /**
   * Revert back your migrations.
   * @return void
   */
   public function down()
   {
          //Roll back your changes done by up method.        
   }
}
```
You can make use of up method to run migration up and down to rollback your migration. Use [Schema Builder](http://www.cygniteframework.com/2013/11/schema-builder.html) to alter your schema.

>[Note: You need not to set table name if you create a migration class same as your table name. Also Schema Builder allows you to set the table name $table->tableName = 'registration';]  
###Migration Up

Let us see an example how to create a table and alter it with migration

```php

  protected $database = 'products';

  /**
   * Run the migrations up.
   * @return void
   */
   public function up()
   {
       Schema::instance(
        $this,
        function ($table) {
            $table->tableName = 'category';
            $table->create(
                array(
                    array('column'=> 'id', 'type' => 'int', 'length' => 11,
                        'increment' => true, 'key' => 'primary'),
                    array('column'=> 'type', 'type' => 'string', 'length' =>100),
                    array('column'=> 'description', 'type' => 'string', 'length'  =>16),
                    array('column'=> 'price', 'type' => 'decimal', 'length'  =>'10,2'),
                    array('column'=> 'rise', 'type' => 'time'),
                    array('column'=> 'time', 'type' => 'timestamp',
                        'length'=> "DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP"
                    ),
                    array('column'=> 'created_at', 'type' => 'datetime'),
                    array('column'=> 'updated_at', 'type' => 'datetime'),
                ),
                'InnoDB',
                'latin1'
            )->run();
        }
      );        
   }

  /**
   * Revert back your migrations.
   * @return void
   */
   public function down()
   {
   //Roll back your changes done by up method.
       Schema::instance(
          $this,
          function ($table) {
              $table->tableName = 'category';
              $table->drop()->run();
          }
       );
   }
```
###Run Migrations : UP

Once done with appropriate changes on your latest migration file, you need to run migration to reflect changes into database.

```php
   G:\wamp\www\cygnite\console\bin>   php cygnite migrate
```
Above command will up your migration into database and also create a new version in your database migrations table.

###Run Migrations : DOWN

You can rollback your latest migration with below command.

```php
   G:\wamp\www\cygnite\console\bin> php cygnite migrate down
```
###Seed Data with Migration

Sometime people use Migration to add data to the database. However, Cygnite has seeding feature used to seed a database with initial data.

```php
   /**
   * Run the migrations up.
   * @return void
   */
   public function up()
   {
      $data = array(
                'name' => 'Apple Iphone',
                'description' => 'Electronics'
      );

      $this->insert('category', $data);
   }

   public function down()
   {
      $this->delete('category', '1');
                or
      $this->delete('category', array('1','2')); //Delete multiple row by id.

   }
```