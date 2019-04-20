# Models

--------------------------------------------------------

Models serve as main point of validation for controllers. They communicate with their controller and either alter the response object directly or communicate to their controller to do so.

--------------------------------------------------------

Kanso comes with a super handy base model that you can extend for your own purposes. You should store your models in the `app/models` directory.

The model is instantiated by its controller. Beyond that, there is no predefined design pattern.

Models are `ContainerAware` so you can access all properties of the dependency container via properties.

```php
$this->Response->body()->set('Hello World!');
```

An example model can be found in the `app/models/Example.php` file.