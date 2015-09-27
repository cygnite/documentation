## Namespaces

Cygnite Framework has powerful namespace import. You can import your libraries, helpers via namespaces.

### Why Namespaces ?
Every Class should have namespace in top of the page after your php tag. You can easily avoid your name collision. Your namespace should be MixedCase. Example-

 "**Cygnite\Pdf**" is allowed but "**Cygnite\PDF**" not be proper.

### Namespace Aliases

You should follow below patterns while aliases - 

* i. You can import libraries or class by
```php
use Cygnite\AssetManager\Asset; // Your alias is Asset
```
* ii.
```php
 use Cygnite\Common\UrlManager\Url; // Your alias is Url 
```
* iii.
```php
use Cygnite\Common\Encrypt as Crypt; //Your alias is Crypt 
```

### Autoload with Namespaces

Cygnite Framework allows you to autoload all your classes by namespace aliases. You can autoload all your libraries, helpers, models or any kind of class in apps/configs/autoload.php using Robot register. Since Cygnite framework uses lazy loading concept so you can register your multiple namespaces.

### Available Namespaces
Current version of Cygnite Framework has below namespaces.

### Asset Manager

Asset manager used to manage all your asset js, css files etc.
```php
//Asset Manager
use Cygnite\AssetManager\Asset;
use Cygnite\AssetManager\AssetCollection;
```
### Base
Base includes Event manager and Router
```php
//Base
use Cygnite\Base\Event;
use Cygnite\Base\Router;
```
### Cache
Cygnite various caching mechanism.
```php
//Cache
use Cygnite\Cache\Storage\Apc;
use Cygnite\Cache\Storage\CMemCache as MemCache;
use Cygnite\Cache\Storage\FileCache;
```
### Common Namespaces
```php
//Common
use Cygnite\Common\CookieManager\Cookie;
use Cygnite\Common\SessionManager\Session;
use Cygnite\Common\SessionManager\Flash\FlashMessage;
use Cygnite\Common\UrlManager\Url;
use Cygnite\Common\Encrypt;
use Cygnite\Common\Input;
use Cygnite\Common\Mailer;
use Cygnite\Common\Pagination;
use Cygnite\Common\Downloader;
use Cygnite\Common\Security;
use Cygnite\Common\Zip;
```
### Database Namespaces
```php
//Database
use Cygnite\Database\ActiveRecord;
use Cygnite\Database\Connections;
use Cygnite\Database\Schema;
use Cygnite\Database\Migration;
```
### IoC Container
```php
// Dependency Injection
use Cygnite\DependencyInjection\Container;
```
### Exception Handler & Dubugger
```php
// Exception and Dumper
use Cygnite\Exception\Handler;
```
### Form Builder
```php
//Form Builder
use Cygnite\FormBuilder\Form;
```
### Foundation
```php
//Cygnite Foundation
use Cygnite\Foundation\Application;
```
### Helpers
```php
//Helpers
use Cygnite\Helpers\Config;
use Cygnite\Helpers\Inflector;
use Cygnite\Helpers\Helper;
use Cygnite\Helpers\Profiler;
```
### Widget Views
```php
// Widget View
use Cygnite\Mvc\View\Widget;
```
### Proxy - Static Resolver
```php
// Proxy - Static Resolver
use Cygnite\Proxy\StaticResolver;
use Cygnite\Proxy\Resolver;
```

### Validation
```php
//Validation
use Cygnite\Validation\Validator;
```
### Reflection
```php
//Reflection
use Cygnite\Reflection;
```
