##Assets Management
Organise and manage your assets using Cygnite Asset Manager. Asset manager provide you object oriented approach to manage and dump all contents as you need. You can use either static Asset calls or AssetCollection factory, which will do most of the work for you cool way.

###Adding Assets To Collection

Cygnite provide you cool way to organise your asset contents and dump into the view page.


```php
use Cygnite\AssetManager\Asset;
use Cygnite\AssetManager\AssetCollection;

$asset =  AssetCollection::make(function($asset)
{
    $asset->add('style', array('path' => 'assets/twitter/bootstrap/css/bootstrap-theme.min.css'))
          //Pick a theme, load the plugin & initialize plugin
          ->add('style', array('path' => 'assets/js/tablesorter/css/theme.default.css'))
          ->add('style', array('path' => 'assets/css/cygnite/table.css'))
          ->add('style', array('path' => 'assets/css/cygnite/style.css'))
          ->add('script', array('path' => 'assets/js/cygnite/jquery.js'))
          ->add('script', array('path' => 'assets/js/custom.js'))
          ->add('script', array('path' => 'assets/twitter/bootstrap/js/bootstrap.js'))
          ->add('link',    array('path' => 'home/index', 'name' => 'Welcome to Cygnite Framework'))
          ->add('script', array('path' => 'assets/js/tablesorter/js/jquery.tablesorter.min.js'));

    return $asset;
});


 // Dump collection of scripts here
 $asset->dump('script');

 // Dump collection of stylesheets here
 $asset->dump('style');

 
 // Dump collection of links here
 $asset->dump('link');

```
###Loading all assets from the folder

Asset Manager also allow you to auto load all asset from the directory using 'your-path/*'.

```php
    
 $asset =  AssetCollection::make(function($asset)
 {
    $asset->add('style', array('path' => 'assets/css/*'));

    return $asset;
 });

 $asset->dump('style');// Will dump all asset(css) from the directory.

```

###Using Where To Group Asset Collections

Collections of group resources can be managed easily using where clause to tag asset resources into specific group. Some case you may not want to dump all assets into header of the page, in such cases you can group asset resources as below.


```php
 use Cygnite\AssetManager\AssetCollection;
    
 $asset =  AssetCollection::make(function($asset)
 {
    $asset->where('header')
          ->add('style', array('path' => 'assets/twitter/bootstrap/css/bootstrap-theme.min.css'))
          ->add('style', array('path' => 'assets/twitter/bootstrap/css/bootstrap.min.css', 
                'media' => '', 'title' => ''));

    $asset->where('footer')
          ->add('style', array('path' => '/assets/css/style.css'));

    return $asset;

 });

 $asset->where('header')->dump('style');// dump assets only tagged to header

 $asset->where('header')->dump('style');// dump assets only tagged to footer

<html>
    <head>
        <title>Some amazing website</title>
        <?php  $asset->where('header')->dump('style'); ?>
    </head>
    <body>

        <!-- ... -->

        <?php  $asset->where('header')->dump('style'); ?>
    </body>
<html>

```
###Adding external stylesheet

You may also would like to use external style sheets into your application. Those case you may simply set true isExternal(true);


```php
 use Cygnite\AssetManager\AssetCollection;

 $asset =  AssetCollection::make(function($asset)
 {
    $asset->where('footer')
          ->isExternal(true)
          ->add('style', array('path' => 'https://ajax.googleapis.com/ajax/libs/angularjs/1.3.14/angular.min.js'));

    return $asset;

 });

 $asset->where('footer')->dump('style');

```
###Compressing Resources Together

Asset Manager provides built-in implementation to combine all assets together and also compress the content of resources. The Developer can add collection of resources to Assets Manager and instruct it which collection of resources should be minified and which collections should be simply combined together. By Compressing content of assets you will be able to boost performance of your application, as you will no longer request to multiple resources, only single request will be made to render asset.


```php
 use Cygnite\AssetManager\AssetCollection;

 $asset =  AssetCollection::make(function($asset)
 {
    $asset->where('header')
          ->add('style', array('path' => 'assets/css/cygnite/style.css'))
          ->add('style', array('path' => 'assets/css/cygnite/dashboard.css'))
          ->combine('style', 'assets/css/', 'final.css', true);

    $asset->where('footer')
          ->add('script', array('path' => 'assets/js/cygnite/jquery.js'))
          ->add('script', array('path' => 'assets/js/custom.js'))
          ->combine('script', 'assets/css/', 'final.script.js');


    return $asset;

 });

$asset->where('header')->dump('style.final');

$asset->where('footer')->dump('script.final.script');

```
Please note that you can only dump resources using the name of your combined resource. In the above example we named final 'style' resource as 'final.css'. In this case 'style.final' should be the key to dump style sheets. Similarly for script 'final.script'.

###Minify The Contents Of Resources

You can simply pass last parameter as true in order to activate compressor. For example.

```php

    $asset->where('header')
          ->combine('script', 'assets/css/', 'final.script.js', true); // Will combine and compress contents

    $asset->where('footer')
          ->combine('script', 'assets/css/', 'final.script.js'); // Will only combine, won't compress contents

```
[Note: Your style sheet / scripts should not have any Line Comment, because while compressing error occurs, alternatively you may have Block Comment into resource file. ]  

###Adding a single stylesheet

Apart from creating asset collection you can also render assets directly into page using static functions. For example.

```php

    use Cygnite\AssetManager\Asset;

    echo Asset::style('assets/css/cygnite/style.css');

```
###Adding a script into the Page

Add a particular script to the page using Asset.

```php

use Cygnite\AssetManager\Asset;

echo Asset::script('assets/js/cygnite/jquery.js');

```
###Adding a link


```php
 use Cygnite\AssetManager\Asset;

  echo Asset::link('category', 'Back', array('class' => 'btn btn btn-info'));

```
###HTML Entities


```php
   use Cygnite\AssetManager\Html;

   echo Html::entities($your-string);

```
###HTML Entities Decode


```php
 use Cygnite\AssetManager\Html;

 echo Html::decode($your-string);

```
###HTML Special Characters


```php
 use Cygnite\AssetManager\Html;

 echo Html::specialCharacters($your-string);

```
###HTML Sanitize


```php
 use Cygnite\AssetManager\Html;

 echo Html::sanitize($your-string);
```