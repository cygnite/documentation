## Email Manager

### Configuration

Cygnite Framework provides you a clean and simple wrapper which is build on most popular **SwiftMailer** library. Mail configurations are available in **apps/configs/application.php**. Basic configuration can be done with smtp transport. You can change the configuration based on your application needs, change host, username, password, port, your admin email which is globally accessible in your application.

### Sending Email Using Cygnite Mailer

Sending email with Cygnite Mailer is very simple with beautiful closure syntax. Get admin email from your configuration or provide one to set from address. 
Setting From Address.

```php
use Cygnite\Helpers\Config;

$config = Config::get('global_config', 'params');

$emailInfo = array(
        'subject' => "Cygnite Framework Mailer",
        'from'    => $config['adminEmail'],
        'content' => "
 Hello World! 
" //Set your html content
);
```
### Send Email

Make sure you have provided swiftmailer path in **apps/configs/application.php** file.

```php
use Cygnite\Common\Mailer;

 Mailer::instance(
    function($mailer) use($emailDetails) {

       $message = $mailer->getMessageInstance()
            ->setSubject($emailDetails['subject'])
            ->setFrom($emailDetails['from'])
            ->setTo("sanjoy09@hotmail.com")
            ->setBody($emailDetails['content'], 'text/html');//'text/plain'

       return $mailer->send($message);
   }
 );
```

### Sending Email to Multiple Recipients

```php
 // Using setTo() to set all recipients in one go
 $mailer->getMessageInstance()->setTo(array(
          'person1@example.org',
          'person2@otherdomain.org' => 'Person 2 Name',
          'person3@example.org',
          'person4@example.org' => 'Person 4 Name'
 ));
```
You can also set recipients with setTo(), setCc() and/or setBcc().

### Get total count of outgoing email
```php
$count = Mailer::instance(
            function($mailer) use($emailInfo) {
               .........
             return $mailer->send($message);
            }
        );

show($count);//get total count of outgoing email
```
### Sending Email With Attachment

Add attachment with your email, addAttachment() method allow you to send email with attachment.

```php
   Mailer::instance(
       function($mailer) use($emailInfo) {
            .........
            $mailer->addAttachment('/path/to/image.jpg');

           return $mailer->send($message);
       }
   );
```
### Inline Attachment
```php
   $mailerAttachment = $mailer->addAttachment("Your Attchment Path");
   $mailerAttachment->setDisposition('inline');
```
### Setting the Message Priority

You can also set priority of the message. Set the priority as an integer between 1 and 5 with the setPriority() method on the Message.
```php
// Indicate "High" priority
  $message->setPriority(2);
```
For more details Setting the Message Priority

> Note: You have full access on SwiftMailer library via Mailer. For more documentation please have a look SwiftMailer Documentation.