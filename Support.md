##Support

###Displaying Formated Output

If you want to debug and view array values as formated output, use show function. Simply pass array into it.


```php
  show($resultArray);

```
If you want to exit to stop execution after formated output then you can simply pass second parameter as exit.
```php
    show($resultArray,'exit');
```
###Get Days Difference

```php
   use Cygnite\Helpers\Helper;
   echo Helper::daysDiff($date);
```

By default it will take the current date and subtract with given date.

###Split Your String As Array

```php
  use Cygnite\Helpers\Helper;
  Helper::stringSplit("Hello World", " "); // By default it will split by dot(.)

```