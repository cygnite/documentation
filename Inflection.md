###Introduction

The Inflector transforms words from singular to plural, class names to table names, modularized class names to ones without, and underscore to camelCase etc. We mostly access Inflector functions statically as we required functions to be accessible globally.

###Singularize String


```php
 echo Inflector::singularize('movies'); // will print movie

 echo Inflector::singularize('products'); // will print product
```
###Pluralize String

```php

 echo Inflector::pluralize('movie'); // will print movies

 echo Inflector::pluralize('product'); // will print products
```

###Camelize String

Underscore to camelCase String.


```php
  echo Inflector::camelize('user_info'); // will print userInfo

```

###Getting Class Name From Namespace

```php

 echo Inflector::getClassName('\Apps\Models\User'); // will print "User"

```
###DeCamelize String


 ```php
 echo Inflector::camelize('userInfo'); // will print user_info

```
###Converting String To Table Name


```php
 echo Inflector::tabilize('UserInfo'); // Class Name to Table Name. Will print "user_info"

```
###Classify The String

Will convert underscore or hyphen to class name as below.


```php
 echo Inflector::classify('user_info'); // Will print "UserInfo"
 echo Inflector::classify('user-info'); // Will print "UserInfo"
```