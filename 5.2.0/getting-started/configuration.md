# Configuration

--------------------------------------------------------

* [Config Files](#config-files)
* [Cascading Configuration](#cascading-configuration)
* [Access](#access)
* [Usage](#usage)

--------------------------------------------------------

The configuration and instantiation of the Kanso core is done in the `app/bootstrap.php` file. This is where you set the error reporting level and define the paths to the application and vendor directories.

All of the remaining framework configuration can be done via the admin panel (if you are using the CMS) or editing the files that are located in the `app/configurations` directory.

--------------------------------------------------------

### Config Files

Kanso configurations are simple `.php` files that return an array:

```php
return
[
    'key_1' => 'value',
    'key_2' => 'value',
];
```

There are separate configuration files for different application components. All default configuration files are extensively commented, please refer to these files for more details on individual configuration options.

--------------------------------------------------------

### Cascading Configuration

The configuration files found in `app/configurations`, use a cascading file system to load. This gives you the ability to run your application with separate configurations for different environments - for example `sandbox` or `production`.

To create an additional configuration environment, all you have to do is create a subdirectory with the name of your environment in the `app/configurations` directory and copy the environment specific files into it.

You can then set the configuration environment in Kanso's `app/boostrap.php` file:

```php
define('KANSO_ENV', 'sandbox');
```
> The `defaults` directory is used by the CMS for installation and developer reference on active applications and should not be changed or altered.

--------------------------------------------------------

### Access

Kanso's configuration can be accessed via the IoC container using the `Config` key.

```php
$config = $kanso->Config;
```

--------------------------------------------------------

### Usage

To retrieve a configuration value (or values), use the `get` method:
```php
$cmsConfig = $kanso->Config->get('cms');
```

You can also fetch config items using `dot.notation`:
```php
$postPerPage = $kanso->Config->get('cms.posts_per_page');
```

The above method returns the `posts_per_page` key value from the `cms.php` configuration file. The method is recursive and will work on nested arrays as well:
```php
$thumbnails = $kanso->Config->get('cms.uploads.thumbnail_sizes');
```

The `set` method allows you to set a configuration value at runtime:
```php
$kanso->Config->set('cms.foo', 'bar');
```

Removing the custom configuration is done using the `remove` method:
```php
$kanso->Config->remove('cms.foo');
```

The `save` method saves the current configuration under the current environment. 
```php
$kanso->Config->save();
```
> Note that if your current `KANSO_ENV` is set to `sandbox`, only the configuration files inside the `app/configurations/sandbox` directory will be overwritten when saved.