### File Upload

Cygnite made file upload very simple with expressive syntax and allow you files to be uploaded. You can set various preferences, restricting the type extension and size of the files.

### File upload process includes

* i. Html Form to allow user to select a file and upload it. You may have a look at Cygnite Form Builder for creating file upload element.
* ii. Validate the file size and type before uploading, if not match throw exception.
* iii. Once form submitted file will upload to the destination folder and show success message to the user.
 Creating the Upload Form

You can use Form Builder to build your form and your form html looks like this below.
```php
 use Cygnite\FormBuilder\Form;
 use Cygnite\Common\UrlManager\Url;

  $form = Form::make(function ($form)
  {
      return $form->open(
               'users',
               array(
                   'method' => 'post',
                   'action' => Url::sitePath('home/upload'),
                   'enctype' => 'multipart/form-data'
                )
              )
               ->addElement('label', 'File Upload', array(
                       'class' => 'col-sm-2 control-label',
                       'style' => 'width:37.667%;'
                   ))
               ->addElement('file', 'document' , array(
                       'class' => 'form-control'))
               ->close()
               ->createForm();
       }
   );

   echo $form->getForm();
```
Above code will generate a form with file input element html code as below.

```html
<form name='users' method='post' action='http://example.com/cygnite/home/upload' enctype='multipart/form-data' >
    <label for='File Upload' class='col-sm-2 control-label' style='width:37.667%;' >File Upload</label>
    <input name='document' class='form-control' type='file'  />
</form>
```
> Read more about [Form Builder](http://www.cygniteframework.com/2013/08/form-builder.html).

Lets assume you have build form using Cygnite Form Builder, let us upload the file.

### Setting Root Directory

Some case you may not be wanted to use the cygnite default root path, in such case you can also set root base path where your uploads folder located.
```php
$upload->setRootDir('your-root-directory-path');
```

### Setting Destination Folder For Upload

Your uploads directory path can be given as "destination" array into the save() method. For example:
```php
 $upload->save(array("destination"=> "assets/upload"));
```

### Validating file before uploading

You can validate file type using file extension and size. For example:
```php
 $upload->ext = array('PNG'); // file extension validation
```
```php
 $upload->setUploadSize('32092'); // file size for validation
 Or
 $upload->size = '32092'; // file size for validation
```

### Uploading a File
```php
 use Cygnite\Common\File\Upload\Upload;

 $response = Upload::process( function($upload)
 {
     // Your code goes here
     $upload->file = 'document'; // input element name
     $upload->ext = array("JPG"); // file extension validation
     $upload->size = '32092';  // file size for validation
     //$upload->setRootDir('your-root-directory-path');

     $response = array();
     $config = array("destination"=> "assets/upload", "fileName"=>"file_new_name");
 
     if ( $upload->save($config) ) {
         // Upload Success
         $response=array('status' => true, 'msg' => 'Profile Photo Uploaded Successfully!');
     } else {
         // Catch errors here
         $response =array('status' => false, 'msg' => implode(', \n', $upload->getError()));
     }

     return $response;
 });

 // var_dump($response['status']);
```

### The Upload Folder

You'll need a destination folder for your uploaded images. From the root of your Cygnite installation folder you will find assets folder, create a folder called uploads and set its file permissions to 777.

### Creating Thumbnail Image

You can also crop uploaded image using Cygnite image thumbnail. It is bundled with Cygnite Framework package.

Cropping or creating thumbnail image is very simple. 
Example Usage:
```php
 use Cygnite\Common\File\Thumbnail\Image;

 $image = new Image();

 $image->setRootDir(CYGNITE_BASE); // set root path
 //Your image location anywhere inside the framework  root directory
 $image->directory = 'Your image location'; // Original image location directory
 $image->fixedWidth  = 100; // width of the thumbnail image
 $image->fixedHeight = 100; // height of the thumbnail image
 $image->thumbPath = 'webroot/'; // destination path for thumbnail image
 $image->thumbName = ''; // new name for thumbnail image
 $image->resize(); // Crop the image and create a thumbnail image into specified location
```

Make sure your destination path has file permissions as 777.

### Setting Root Directory

Some case you may not be want to use the cygnite default root path, in such case you can also change root path where your uploads folder located.
```php
$image->setRootDir(); // system will take getcwd() as root path
    or 
$image->setRootDir(CYGNITE_BASE); // cygnite root base path
```

### Downloading File

You can download file from the specific path using File Downloader as below.
```php
 use Cygnite\Common\File\File;

 $file = new File();
 // By default system will use Cygnite installation root directory if you don't set your root dir path
 $file->setRootDirectory('your-root-directory-path'); 

 $file->download('your/file/path');
```

By default file downloader allows various types of files to download as jpeg, jpg, gif, png, pdf, docx, zip, xls, video etc.