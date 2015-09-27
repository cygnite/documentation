###Improving Performance With Cache  
Cygnite provides api wrapper for various caching system. Cache allow you to faster access of frequently used or prepopulated data. You may need to use caching system when large amount of code piece always return you same data. By default, Cygnite configured to use file cache driver. You can configure file cache in **/apps/configs/application.php**. File caching system store data into file as json format into **/apps/temp/cache/** folder. However if you are building large application then it is recommend to use memory based caching system, such as Memcached or APC.

Cygnite provides you popular APC, Memcached, File caching api to cache data.

* File Cache
* APC Cache
* Memcache

###Configurations

You can configure **apps/configs/application.php** to use built-in file cache system.

+ Enable cache in your config file by true / false.
+ Provide cache name to generate encrypted file name in your application.php . By default, cache name is "file.cache".
+ You are also allowed to change cached file extension by configuration in the same application.php file.
+ Default location of your cache files is /apps/temp/cache/ folder. You can optionally change storage path in the same configuration file.  
  
###Using File Cache: Storing Data In The File Cache

You can store data in file cache by using factory object as below. For example-


```php
 use Cygnite\Cache\Factory\Cache;

 $file = Cache::make('file', function ($file)
 {
     $file->store('name', 'Cygnite PHP Framework');

     return $file;
 });

```
###Storing Data In The File Cache With Expire Time

You can also set expire time for file cache, meaning that cache will be destroyed after the expire time.

```php

 use Cygnite\Cache\Factory\Cache;

 $file = Cache::make('file', function ($file)
 {
     $file->store('name', 'Cygnite PHP Framework', $expireTime);

     return $file;
 });

```
###Storing Data In Different File: Naming Cache

By default store() method will save all your cached information into single file. But if you wish to store data into different file based on the name you provide, then you may follow below example.

```php

 use Cygnite\Cache\Factory\Cache;

 $file = Cache::make('file', function ($file)
 {
     $file->->as('some-name')->store('name', 'Cygnite PHP Framework');

     return $file;
 });

 // you can get information from cache using same name alias

 echo $file->as('some-name')->get('name');

```
###Checking For Existence In Cache

```php

 use Cygnite\Cache\Factory\Cache;

 $file = Cache::make('file', function ($file)
 {
     $file->->as('some-name')->store('name', 'Cygnite PHP Framework'); // with alias

     $file->store('key', 'Store My Content Into Cache'); // Without aliasing 

     return $file;
 });


 if ($file->has('key')) {
   
    // .....
 }

 Or
 
 // Checking existence of cache 
 if ($file->as('some-name')->has('name')) {

    // ......

 }

```
###Retrieving Data From The File Cache

Your data are stored as json format into file cache. You can simply retrieve data from the cache by the key which used to store.

```php

 echo $file->get('name');
```

###Destroying Data From The File Cache

You can destroy all cache from the file using destroyAll(); method and also using destroyExpiredCache() method to destroy only specific cached data which are expired.

```php

 $file->destroyExpiredCache();
 $file->destroyAll();

```
###Using APC Cache
You should have APC extension loaded into your machine in order to use APC caching system, else cache will throw exception.

###Storing An Item Into APC Cache

You can use APC caching technique to boost your application performance. Storing data into APC is similar like file cache.


```php
  use Cygnite\Cache\Factory\Cache;

  $apc = Cache::make('apc', function ($apc)
  {
       $apc->store('name', 'Welcome To Cygnite PHP Framework');

       return $apc;
  });

```
###Setting Lifetime For APC Cache

Default lifetime of APC cache is 600s. You can optionally set lifetime (expire time) of APC cache as below.


```php
  $apc->setLifeTime('Time in sec');
```

###Retrieving An Item From APC Memory

```php

 $apc->get('name');

```
###Destroy Data From The APC Memory

Use the same key to destroy data from APC memory.

```php

 $apc->destroy('name');

```
###Using Memcache Memory

Before start using remember you must have Memcache configured into your system. Once you configure 'Memcache' class will be available to use.

 Creating A New Server

You need to create a new server in order to save or retrieve data from Memcache. By default, Cygnite connect with localhost and port number 11211.


```php
  use Cygnite\Cache\Factory\Cache;

  $memcache = Cache::make('memcache', function ($memcache)
  {
       $memcache->create('host-name', 'port-number');

       return $memcache;
  });

```
###Storing An Item Into Memcache Memory

```php

  use Cygnite\Cache\Factory\Cache;

  $memcache = Cache::make('memcache', function ($memcache)
  {
       // will connect with default host and port
       $memcache->create()
                ->store('key', 'value');

       return $memcache;
  });

```
###Retrieving Item From Memcache Memory

```php

 $memcache->get('key');

```
###Destroying Item From Memcache


```php
 $memcache->destroy('key');

```
###Getting Instance Of Cache Drivers

If you don't wish to use Closure syntax, you can also get the instance of driver using make() method call.


```php
 use Cygnite\Cache\Factory\Cache;

 $file = Cache::make('file'); // Will return file cache instance
 $apc = Cache::make('apc'); // Will return APC cache instance
 $memcache = Cache::make('memcache'); // Will return Memcache instance

```
That's all about the cygnite caching system. You can also optionally make use of any composer packages for other caching mechanism.