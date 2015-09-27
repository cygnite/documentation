##Form Validator

###Getting Validator Instance

Validating your form inputs are very simple and expresive. Get your validator instance, set rules, run validator, that's all. Your job done.
For Example :

 
 ```php

 use Cygnite\Validation\Validator;

 $input = Input::make();

 $validator = Validator::instance(
  $input,
  function ($validate) {
      ...........
  }
 );
```

###Adding Rules To Validator

Set validation rules to cygnite validator and run validation. For Example

 

```php
    $validator = null;
    $validator = Validator::instance(
        $input,
        function ($validate) {
            $validate->addRule('username', 'required|min:3|max:5')
                ->addRule('password', 'required|is_int|valid_date')
                ->addRule('phone', 'phone|is_string')
                ->addRule('email', 'valid_email');
    
            return $validate;
        }
    );

```

###Validating Form Input

 

```php
 if ($validator->run()) {
   echo 'Validate Successfully';
   // Do something  
 } else {
   show($validator->getErrors());
 }

```
###Get Validation Errors

You can access the validation errors as array as well as single string.

For Example : 
i. Get all the errors as array.

 
```php
        show($validator->getErrors()); 
 ```

ii. Get errors by form element name.

 

```php
        echo $validator->errors['name_error'];
        echo $validator->errors['phone_error'];

 ```

###Displaying Errors Below To Form Element

You just need to set the Validator object into the Form property in order to display validation error below the form element.


  ```php
 use Apps\Components\Form\Registration;
  
 $form = new Registration();
 $form->validation = $validator;// set the validator instance
```