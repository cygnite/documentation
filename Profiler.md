##Profiler

###Benchmarking/Profiling Your Application

Benchmark your code, find execution time and memory consumption by the script using Cygnite profiler. 
Create a block and start profiling.

```php             

use Cygnite\Helpers\Profiler;
Profiler::start('block1');
```
Start your profiler before your custom code and end the profiler where your code completes with same block key.
Stop Profiling

```php            

   use Cygnite\Helpers\Profiler;
   Profiler::end('block1');

```
###Multiple Profiler Block In Same Page

You can also have multiple block section to check each script execution time. But be sure you are closing correctly profiler block.

               
```php
 use Cygnite\Helpers\Profiler;

 Profiler::start('block1');
 Profiler::start('block2');
   ....................
   Your custom code goes here
   ....................
 Profiler::end('block2');
 Profiler::end('block1');
```