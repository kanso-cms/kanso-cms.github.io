# Application access

--------------------------------------------------------

* [Instantiated reference](#instantiated-reference)
* [Static instance](#static-instance)
* [Currying](#currying)
* [Controllers and Models](#controllers-and-models)
* [Views](#views)
* [Container Aware](#container-aware)

--------------------------------------------------------

When you are working with Kanso you will enter various scopes in your code (e.g. global scope and function scope). You will likely need a reference to the Kanso instance in each scope. There are several ways to do this:

- Use Kanso's static `::instance` method.
- Curry an application instance into a function scope with the use keyword.
- Use Kanso's `ContainerAware` trait inside a class.
- Use the `$kanso` variable inside any view template file.

--------------------------------------------------------

### Instantiated reference

Within your front-controller file where Kanso is first instantiated (`app/bootstrap.php`) will always be a reference to the Kanso application.

This is always the `$kanso` variable. If you're working within this scope, you can simply use the instantiated instance:

```php
$SQL = $kanso->Database->Builder();
```
--------------------------------------------------------

### Static instance

If you need to get a reference to the Kanso application instance from a different scope (e.g in a class or function), you can use Kanso's static `::instance` method to get an instance:

```php
use kanso\Kanso;

...

function fooBar()
{
    $SQL = Kanso::instance()->Database->Builder();
}
```

--------------------------------------------------------

### Currying

When declaring an anonymous function (e.g a callback), you can inject a `$kanso` reference into the callback function with the use keyword:

```php
use kanso\Kanso;

...

$kanso = Kanso::instance();

$callback = function($args) use ($kanso)
{ 
    $SQL = $kanso->Database->Builder();
};
```
> Keep in mind that isn't the most performance efficient way to get a reference to Kanso.

--------------------------------------------------------

### Controllers and Models

Kanso comes with a base [model](/mvc/model) and [controller](/mvc/controller) you can use by extending them.

The base model and controller use Kanso's `ContainerAwareTrait` for application access. This means that any service that is loaded into the Kanso IoC container will be available to any model or controller you create.

The following example controller retrieves a database SQL builder instance:

```php
use kanso\framework\mvc\controller\Controller;

...

class MyController extends Controller
{
    public function sqlBuilder()
    {
        return $this->Database->builder();
    }
}

```

The following example model retrieves a database SQL builder instance:

```php
use kanso\framework\mvc\model\Model;

...

class MyModel extends Model
{
    public function sqlBuilder()
    {
        return $this->Database->builder();
    }
}
```

--------------------------------------------------------

### Views

When working within any file loaded from the [view](/mvc/view), a Kanso instance is always available via the `$kanso` variable.

```php
$SQL = $kanso->Database->Builder();
```

--------------------------------------------------------

### Container Aware

Kanso comes with a handy trait that you can apply to any class called `ContainerAwareTrait`. 

The trait can be applied inside any class for application access. This means that any service that is loaded into the Kanso application via the IoC container will be available to a class with this trait.

To use the trait, load the container into your class:

```php
use kanso\Kanso;
use kanso\framework\ioc\ContainerAwareTrait;

...

class Example
{
    use ContainerAwareTrait;
}

```

You will now have access to all Kanso services inside the container in the class via PHP's magic methods:

```php

$myClass = new Example;

$SQL = $myClass->Database->builder();
```
