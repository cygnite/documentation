##Pagination

###Configuration

We believe in painless development. Cygnite pagination is completely model based though you can use it separately. Generating pagination is very simple with almost zero configuration.

###Pagination With Model

You just need to set how many records should display per page in your model. Two ways we can set page number.

* Set page number using model. or

* Set page number using all() method.

* Let us consider model is ShoppingProduct.

```php

namespace Apps\Models;

use Cygnite\Common\UrlManager\Url;
use Cygnite\Database\ActiveRecord;

class Product extends ActiveRecord
{
   //your database connection name
   protected $database = 'cygnite';

   protected $primaryKey = 'id';

   public $perPage = 5; // Set Pagination per page

   public function __construct()
   {
       parent::__construct();
   }

   // Set Current Page Number using URL segment
   protected function pageNumber()
   {
       return Url::segment(3);
   }
}

```

* If you don't wish to set current page number using model, you can also use finder method to set current page number to pagination.

```php
$products = Product::all(
       array(
           'orderBy' => 'id desc',
           'paginate' => array(
               'pageNumber' => Url::segment(3)
            )
       )
 );
```
That's all you are done.

###Create Pagination Links

Your pagination links will generate with createLinks() and pass into your view page.


```php
  $links = Product::createLinks();  

  $this->render('index', array('links' => $links));

```
###Displaying links into the view page

Render links where you would like to display pagination as below.

 
```php

echo $links;
```