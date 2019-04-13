# Error Handling

--------------------------------------------------------

* [Access](#access)
* [Usage](#usage)
* [Logging](#logging)
* [Error Messages](#error-messages)
	* [Displaying Error Messages](#displaying-error-messages)
	* [Suppressing Errors Messages](#suppressing-error-messages)
* [Throwing Exceptions](#throwing-exceptions)

--------------------------------------------------------

Kanso converts all `exceptions` to `ErrorException` and has a built in error handler and logger. This allows the error handler to display an error page that contains a lot more information than the standard PHP error pages when something bad happens.

The configuration options of error handling are found in the `app/configurations/application` file under `error_handler`.

--------------------------------------------------------

### Access
You can access Kanso's error handler via the IoC container with the ErrorHandler key.

```php
$errorHandler = $kanso->ErrorHandler;
```
--------------------------------------------------------

### Usage

You can register custom error handlers for different kinds of exception types using the handle() method.

A great place to do so is in the `app/bootstrap.php` file.

> Your error handler should not have a return value unless you want it to stop further error handling.

You can register multiple error handlers for any type of error. Note that the last one registered is the first one to get executed.

To register a custom error handler use the `handle` method:
```php
$errorHandler->handle('\PDOException', function(Throwable $exception)
{
    # Do some custom logging here
    
    # return false; Will stop subsequent error handling.
});
```

The `clearHandlers` method clears all handlers for a given exception type.
```php
$errorHandler->clearHandlers('\PDOException');
```

The `disableShutdownHandler` method will stop kanso from handling fatal errors.

```php
$errorHandler->disableShutdownHandler();
```

--------------------------------------------------------

### Logging

The framework's default error logger will log all errors and exceptions to the `app/storage/logs` directory. This includes `404` responses.

Having error logging enabled can be useful even when in production, but not all error types are worth logging.

You can disable error logging for specific exception types by using the `disableLoggingFor` method:
```php
$errorHandler->disableLoggingFor('\Kanso\Framework\Http\Response\Exceptions\NotFoundException');
```

You can also pass an array of exception types:

```php
$errorHandler->disableLoggingFor
([
    '\Kanso\Framework\Http\Response\Exceptions\NotFoundException',
    '\Kanso\Framework\Http\Response\Exceptions\MethodNotAllowedException',
]);
```
--------------------------------------------------------

### Error Messages

When an exception is thrown or the application encounters an error, the application will handle the error depending on your configuration.

#### Displaying Error Messages

To show error and exception messages, set `display_errors` to `true` in the `app/configurations/application.php` file.

Kanso will output a custom error page with detailed debugging information when an error occurs or an exception is thrown.

If the error or exception occurred in an `HTTP POST` request via `AJAX` the debug information will be output via `JSON`.

#### Suppressing Error Messages

To suppress error and exception messages, set `display_errors` to `false` in the `app/configurations/application.php` file.

Kanso will display a generic error page with no information.

If the error or exception occurred in an `HTTP POST` request via `AJAX` the output will be sent via `JSON`.

> If the CMS component is enabled and `display_errors` set to `false`, `404` errors will load the `404.php` file in the active theme if it exists or fallback to a generic page if it doesn't exist.

--------------------------------------------------------

### Throwing Exceptions

To throw an exception simply follow the standard `PHP` syntax. The error will be logged and/or displayed depending on your configuration.

```php
if ($somethingBad)
{
    throw new E\xception('Something bad happened.');
}
```

Kanso comes with a handy group of exceptions to use when you need to stop the application.

These exceptions are application specific and will immediately stop the application and send an `HTTP` response. The `HTTP` response is subject to your `display_errors` configuration as outlined above.

The `NotFoundException` can be used to send a `404` response:
```php
use \kanso\framework\http\response\exceptions\NotFoundException;

...

if ($somethingBad)
{
    throw new NotFoundException('The requested document could not be found.');
}
```

The `ForbiddenException` can be used to send a `403` response:
```php
use \kanso\framework\http\response\exceptions\ForbiddenException;

...

if ($somethingBad)
{
    throw new ForbiddenException('You are not allowed access to this file.');
}
```

The `MethodNotAllowedException` can be used to send a `405` response when an `HTTP` request was made over an invalid method:
```php
use \kanso\framework\http\response\exceptions\MethodNotAllowedException;

...

if ($isPostRequest)
{
    throw new MethodNotAllowedException(['GET'], 'Invalid HTTP POST request. Request to this page can only be made via GET.');
}
```

The `InvalidTokenException` can be used to send a `498` response when a request provided an invalid `CSRF` token:
```php
use \kanso\framework\http\response\exceptions\InvalidTokenException;

...

if ($somethingBad)
{
    throw new InvalidTokenException('Invalid or expired token. Please clear your browser cache and/or cookies.');
}
```

The `RequestException` allows you to throw a custom exception with the response code of your choosing:
```php
use \kanso\framework\http\response\exceptions\RequestException;

...

if ($somethingBad)
{
    throw new RequestException(500, 'Something unexpected happened.');
}
```

The `Stop` exception immediately stops the application.
```php
use \kanso\framework\http\response\exceptions\Stop;

...

if ($somethingBad)
{
    throw new stop;
}
```