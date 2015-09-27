## Models

### What is Models ?

Every model class act as simple orm to map your database tables as objects to receives the information and update it's states. Your table name must be lowercase followed by underscore prefix and model name must be **StudlyCaps**. Your models must be inside **apps/models** directory.

### How to import models ?

Cygnite Framework has redesigned and added lots of flexibility. Now you don't have to register model alias into your autoload. All your models will get autoloaded via Cygnite internally, you need to declare namespace in your controller, and you are ready. Call your model and do some stuffs. Isn't it easy and simple.

### How to access models functions ?

It is very simple as you do create object to access your class. If you need to do some extra stuffs then create a method (example: getDetails() ) into model and access.

```php  
use Apps/Models/Record;

$record = new Record();
$record->getDetails();
```

### Want to access model functions into Views ?

Though its not good practice to call your model functions in your view page, Cygnite Framework gives your flexibility access your model from view page (if you are using pain php as view page).

For example-
```php
use Apps\Models\ShoppingProducts;

$model = new ShoppingProducts();
info = $model->getDetails();
show($info);
```
These are basics of models. There are more stuffs you can do with your simple model class. Find more information about database query builder, Dynamic Finders, Pagination, Model Events, Crud operation on database user manual.