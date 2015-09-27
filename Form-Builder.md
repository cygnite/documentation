###Dealing With Forms

Dealing with html form is one of the day to day and challenging task for a developer. Cygnite Framework gives you more freedom and easy way to build forms with most elegant syntax.

Make your view page much more simple and clean. Build your HTML Forms either inside a controller, view page or as a component and then render it in a layout or into the view page.

###Creating A Simple Form

You can build your amusing forms pass the object into your view. Suppose if you want to build a contact us form with add/edit functionalities, this is where Cygnite Framework makes you job much simple and takes your pain build form. Forms can be build 3 different ways

Get the form instance and add elements to it, create your amusing form. i. Building Form as Component. ii. Build Form without Closure. iii. Build Form beautiful syntax using Closure.

###Building Form as Component


```php
namespace Apps\Components\Form;

use Cygnite\Foundation\Application;
use Cygnite\FormBuilder\Form;
use Cygnite\Common\UrlManager\Url;

/**
 * Sample Form using Cygnite Form Builder
 *
 */

class RegisterForm extends Form
{

    private $model;

    public $errors;

    private $segment;

    public function __construct($object = null, $segment = null)
    {
        $this->model = $object;
        $this->segment = $segment;
    }


    /**
     *  Build form and return object
     * @return Registration
     */
    public function buildForm()
    {
        $id = (isset($this->model->id)) ? $this->model->id : '';

        if (is_object($this->validation)) {
                        
            /* Set your custom validation errors*/
            //$this->validator->setCustomError('category_type_error', 'Your Custom Error');
        }

        $this->open(
                    'category', array(
                    'method' => 'post',
                    'action' => Url::sitePath('category/type/' . $id . '/' . $this->segment),
                    'role' => 'form',
                    'style' => 'width:500px;margin-top:35px;float:left;'
                        )
                )
                ->addElement('label', 'Category Type', array(
                    'class' => 'col-sm-2 control-label',
                    'style' => 'width:100%;'
                        )
                )
                ->addElement('text', 'category_type', array(
                    'value' => (isset($this->model->category_type)) ? $this->model->category_type : '',
                    'class' => 'form-control'))
                ->addElement('label', 'Category Name', array('class' => 'col-sm-2 control-label', 'style' => 'width:100%;'))
                ->addElement('text', 'category_name', array(
                    'value' => (isset($this->model->category_name)) ? $this->model->category_name : '',
                    'class' => 'form-control'))
                ->addElement('label', 'Short Description', array('class' => 'col-sm-2 control-label',  'style' => 'width:100%;'))
                ->addElement('textarea', 'description', array(
                    'value' => (isset($this->model->description)) ? $this->model->description : '',
                    'class' => 'form-control'))
                ->addElement('submit', 'btnSubmit', array(
                    'value' => 'Save',
                    'class' => 'btn btn-primary',
                    'style' => 'margin-top:15px;'
                        )
                )
                ->close()
                ->createForm();

        return $this;
    }

    /**
     * Render form
     * @return type
     */
    public function render()
    {
        return $this->getForm();
    }
}

```
###Rendering Form

```php

use Apps\Components\Form\RegisterForm;

// your $id form id if set then edit page else add
if (isset($id) && $id !== null) {
    // Update the category
    $model = array();
    $model = Category::find($id);
    $form = new RegisterForm($model, Url::segment(4));
    $form->errors = $errors;
    $form->validator = $validator;//Set validator instance to form builder to set error to form elements

    echo $form->buildForm()->render(); // render update form with values
} else {

    //Add a new Category
    $form = new RegisterForm();
    $form->errors = $errors;
    $form->validator = $validator;// set the validator instance

    echo $form->buildForm()->render(); // Render your category view page

}
```

Cygnite also shipped with sample crud application, if you may want to have a look at.

###Build Form without Closure
Create an array of elements and pass to closure to create your awesome form.

```php

 use Cygnite\FormBuilder\Form;

 $form = Form::make()
        ->open(
            'contact',
            array(
                'method' => 'post',
                'action' => Url::sitePath('contact/add'),
                'role' => 'form',
                'style' => 'width:500px;margin-top:35px;float:left;'
            )
        )
        ->addElement('label', 'Name', array(
            'class' => 'col-sm-2 control-label',
            'style' => 'width:37.667%;'
        ))
        ->addElement('text', 'user_name' , array(
            'value' => '',
            'class' => 'form-control'))
        ->addElement('label', 'Email Address', array('class' => 'col-sm-2 control-label'))
        ->addElement('text', 'email_address', array(
            'value' => '',
            'class' => 'form-control'))
        ->addElement('label', 'Document', array('class' => 'col-sm-2 control-label'))
        ->addElement('file', 'document', array(
            'value' => '',
            'class' => 'form-control'))
        ->addElement('submit', 'btnSubmit', array(
            'value' => 'Save',
            'class' => 'btn btn-primary',
            'style' => 'margin-top:15px;'
        )
        )
        ->close()
        ->createForm();

  echo $form->getForm();

```
###iii. Using Closure Syntax
Build Form using most expressive syntax.


```php
 use Cygnite\FormBuilder\Form;

  $form = Form::make(
     function ($form) {
       return $form->open(
            'users',
            array(
                'method' => 'post',
                'action' => Url::sitePath('welcome/upload'),
                'enctype' => 'multipart/form-data'
            )
          )
          ->addElement('label', 'Name', array(
            'class' => 'col-sm-2 control-label',
            'style' => 'width:37.667%;'
            ))
          ->addElement('text', 'name' , array(
            'value' => '',
            'class' => 'form-control'))
          ->close()
          ->createForm();
     }
 );

 echo $form->getForm();
```

###Build Form With Array of Elements.

Alternatively you can also create stack of elements as array and pass into addElements() method to generate your form.

```php


  $elements = array(
        array(
            'Name' => array(
                'class' => 'text-box',
                'type' => 'label',
            ),
            'name' => array(
                'value' => 'Cygnite Framework',
                'class' => 'text-box',
                'type' => 'text',
            ),
            'Email' => array(
                'class' => 'text-box',
                'type' => 'label',
            ),
            'email-address' => array(
                'value' => 'sanjoy@hotmail.com',
                'class' => 'text-box',
                'type' => 'text',
            ),
            'document' => array(
                'class' => 'text-box',
                'type' => 'file',
            ),
            'submit' => array(
                'value' => 'Submit',
                'type' => 'submit',
            )
        )
    );

 $form = Form::make(
        function ($form) use($elements) {
            return $form->open(
                'users',
                array(
                    'method' => 'post',
                    'action' => Url::sitePath('welcome/register'),
                    'enctype' => 'multipart/form-data'
                )
            )
            ->addElements($elements)
            ->close()
            ->createForm();
        }
    );

 echo $form->getForm();
```

###Creating Other HTML Elements

Similarly you can render other html tags, you just have to provide field type in order to render.

For example

If you want to render other html elements simply pass first parameter as type of the element (addElement()) or If you are using array of element then pass as type as below.

###Checkbox
```php
    $form->addElement('checkbox', 'user_name' , array(
                'value' => '',
                'class' => 'form-control'))
```
###Creating File HTML Element

```php

  $elements = array('document' => array(
                'class' => 'text-box',
                'type' => 'file',
            ));

  $form->addElements($elements);

  OR

  $form->addElement('file', 'document', array(
            'value' => '',
            'class' => 'form-control'))

```
###Creating Radio Button

```php

 $form->addElement('radio', 'gender' , array(
                'value' => '',
                'class' => 'form-control'))
  or 

 $element = array(
             'gender' => array(
                'class' => 'text-box',
                'type' => 'radio',
            )
 );
 
```
###Creating Select Box


```php
 $form->addElement("select", "Country", 
                  array("options" => array('Select Country', 'America', 'China', 'India', 'Bangladesh'), 
                  "class" => "form-control")
 );

 // Default value to select box

 
 $form->addElement("select", "Country", 
                  array("options" => array('Select Country', 'America', 'China', 'India', 'Bangladesh'),
                  "selected" => 3,
                  "class" => "form-control")
 );
 
```