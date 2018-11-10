# Dependency injection

--------------------------------------------------------

* [Auto Loading](#auto-loading)
* [Registering Services](#registering-services)
* [Access](#access)
* [Usage](#usage)

--------------------------------------------------------

Kanso comes with an easy to use inversion of control container and an autoloader.

The IoC Container uses dependency injection to prepare, manage, and inject application dependencies making Kanso more maintainable and decoupled.

All controllers, migrations and commands are instantiated using the IoC container making it easy to inject dependencies.

--------------------------------------------------------

### Auto Loading

Kanso's built in autoloader should be used for registering any external dependences. External dependencies should be stored in your `vendor/` directory.

Register a namespace with Kanso's autoloader inside the `app/bootstrap.php` file. Make sure you register your namespace before the autoloader is registered.

```php
$autoloader->addPrefix('VendorRepo', $SERVER['DOCUMENT_ROOT'] . DIRECTORY_SEPARATOR . 'vendor' . DIRECTORY_SEPARATOR . 'vendor-repo');
```

Now your classes found under the `VendorRepo` namespace will be autoloaded when called following the `PSR-4` spec.

If you have installed Kanso via Composer, you should comment out Kanso's default autoloader inside the `app/bootstrap.php` and require composer's autoloader. 

--------------------------------------------------------

### Registering Services

If you want register a service inside Kanso's IoC container, you can create a custom boot service inside the `app/services` directory.

```php
namespace kanso\app\services;

use kanso\framework\application\services\Service;

class CustoService extends Service
{
    /**
     * {@inheritdoc}
     */
    public function register()
    {
        $this->container->singleton('MyClass', function ()
        {
            return new namespace\MyClass;
        });
    }
}
```

Then get Kanso to boot the service at runtime by adding it into Kanso's `app/configuraions/application.php` file under the `services.app` key.

```php
'app' =>
[
    '\kanso\app\services\CustoService'
],
```

The `CustoService` service will now boot at runtime.

--------------------------------------------------------

### Access

You can get direct access to Kanso's container via a Kanso instance using the container method:

```php
$container = $kanso->container();
```

If you don't have access to the Kanso application use the instance method:

```php
use kanso\Kanso;

...

$container = Kanso::instance()->container();
```

--------------------------------------------------------

### Usage

To register a dependency in the container use the `set` method:
```php
$container->set('foo', new namespace\ClassName());
```

You can also register dependencies using a `closure`. The `closure` will not be executed until it is required.
```php
$container->set('foo', function($container)
{
    return new namespace\ClassName();
});
```

The `singleton` method does the same as the `set` method except that it makes sure that the same instance is returned every time the class is resolved through the container.
```php
$container->singleton('foo', function($container)
{
    return new namespace\ClassName($container->bar);
});
```

The `setInstance` method is similar to the `singleton` method. The only difference is that it allows you to register an existing instance in the container.
```php
$container->setInstance('foo', new Bar);
```

The `has` method allows you to check for the presence of an item in the container.
```php
if ($container->has('foo'))
{
    // do something
}
```

The `get` method lets you resolve a dependency through the IoC container.
```php
$foo = $container->get('foo');
```

You can also use PHP's magic methods to retrieve dependencies.

```php
$foo = $container->foo;
```