
##Event Handler

###Attaching an Event

For Example :

 ```php
 
use Cygnite\Base\Event;

$event = new Event();
$event->attach('beforeSave', 'setUserDefinedFunction'); 
or
$event->attach('afterSave', '\\Apps\\Components\\applyUserDefinedFunction'); 
or
$event->attach('afterValidation', '\\Apps\\Components\\General@userDefinedFunction');  
or
$event->attach('event', function () {
   echo "Hellow Here.";
}); 
 
```

###Trigger An Event

For Example :

```php
 $event->trigger('event');
or
$event->trigger('beforeSave');
or
//path : apps/components/general.php
class General 
{
    public function userDefinedFunction()
    {
       echo "Handle your requests. Triggered by Events"; 
    } 
}

```

###Flush Event

Provide event name to de-attach. If you want to de-attach all event then just call flush method. For Example : 
For Example :

 ```php
  
$event->flush('event'); // flush particular event.
or
$event->flush(); //de-attach all events
```