
## Introduction

**Cygnite CLI** build on popular Symfony console component. It provides you helpful commands to make your development faster.

## Code Generation

### Code Generation - Crud Generation

```php
  php cygnite generate:crud controller table_name database //plain php layout, views

  php cygnite generate:crud controller table_name database --template// twig templates  

  or

  php cygnite generate:crud controller table_name // Default database connection

  php cygnite generate:crud controller table_name --template // twig templates for view page
```

## Database Migration and Seeding

[Database Migration](http://www.cygniteframework.com/2013/11/migrations.html)

```php
 php cygnite migrate:init table_name
 
 php cygnite migrate up

 php cygnite migrate down
```

### Form Generator

```php
 php cygnite generate:form table_name database
  or 
 php cygnite generate:form table_name // Default database connection
```

### Generating Basic Controller

Below command will generate basic ProductController into your controllers folder.

```php
 php cygnite controller:create product
```

### Generating Resource Controller

Below command will generate resource **ProductController** into your controllers folder.
```php
 php cygnite controller:create product --resource
```

### Generating Models

Below command will generate model class called **ProductCategory** into your models directory.

```php
 php cygnite model:create product_category directory
```

Above command "**product_category**" represent table name and "directory" represent as database.

If you want to generate model against your default database connection then we can also make above shorter.

```php
 php cygnite model:create product_category
```

Above features are driven by **Cygnite CLI** tool to make your job very easy.