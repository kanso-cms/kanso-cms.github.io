# Response

--------------------------------------------------------

- [Access](#access)
- [Status](#status)
- [Headers](#headers)
- [Body](#body)
- [Format](#format)
- [Protocol](#protocol)
- [Cache](#cache)
- [Helpers](#helpers)

--------------------------------------------------------

The response service is an abstraction of Kanso's `HTTP` response that is returned to the `HTTP` client. An instance of the response object is always available in via the IoC container.

The response class provides helper methods, that help you interact with these HTTP response properties. The default response object will send a 200 OK HTTP response with the text/html content type.

--------------------------------------------------------

### Access

You can access the Response object directly through the IoC container:

```php
$response = $kanso->Response;
```

--------------------------------------------------------

### Status

The `status` method returns the status object which is used for managing the response status.

```php
$status = $kanso->Response->status();
```

The `set` method sets the status code:
```php
$status->set(200);
```

The `get` method returns the currently set status code:
```php
$code = $status->get();
```

The `message` method returns the message for the current code.
```php
$message = $status->message();
```

To validate the response status, use one of the built in helper methods:
```php
$status->isEmpty();
$status->isInformational();
$status->isOk();
$status->isSuccessful();
$status->isRedirect();
$status->isForbidden();
$status->isNotFound();
$status->isClientError();
$status->isServerError();
```

--------------------------------------------------------

### Headers

The `headers` method returns the Headers object which is used to manage response headers:
```php
$headers = $kanso->Response->headers();
```

To set a header, use the `set` method:
```php
$headers->set('Keep-Alive', 'timeout=5, max=100');
```

To set multiple headers at once, use the `setMultipl`e method with an array:
```php
$headers->setMultiple([
    'Keep-Alive' => 'timeout=5, max=100',
    'Date'       => date('c'),
]);
```

The `get` method returns a header value by key or `false` if it doesn't exist:
```php
$keepAlive = $headers->get('Keep-Alive');
```

The `has` method checks if a header exists:
```php
if ($headers->has('Keep-Alive'))
{
    # Do something here
}
```

The `remove` method removes a header by key:
```php
$headers->remove('Keep-Alive');
```

The `clear` method removes all the headers:
```php
$headers->clear();
```

You can also iterate the response headers as an array:
```php
foreach ($headers as $k => $v)
{
    # Do something here
}
```

The `asArray` method returns an associative array of all the headers:
```php
$headerArray = $headers->asArray();
```

The `sent` method checks if the headers have been sent:
```php
if ($headers->sent())
{

}
```

The `send` method immediately sends the headers. Note that this is handled internally by the `Response` object.
```php
$headers->send();
```
--------------------------------------------------------

### Body

The `body` method returns the response body which is used to manage the response body:
```php
$body = $kanso->Response->body();
```

To set the body, use the `set` method:
```php
$body->set('Hello World!');
```

To get the body, use the `get` method:
```php
$html = $body->get();
```

The `append` method appends a string to the body:
```php
$body->append('foo');
```

The `clear` method clears the body:
```php
$body->clear();
```

The `length` method returns the body length:
```php
$length = $body->length();
```
--------------------------------------------------------

### Format

The `format` method returns the response format which is sent as a header:
```php
$format = $kanso->Response->format();
```

To set the response format, use the `set` method with a valid mime type:
```php
$format->set('text/html');
```

You can pass a file extension instead, the method will automatically convert it to the correct mime type:
```php
$format->set('json');
```

The `setEncoding` method sets the format encoding:
```php
$format->setEncoding('UTF-8');
```

The `get` method returns the current format as a mime type:
```php
$mime = $format->get();
```

The `getEncoding` method returns the current format encoding:
```php
$encoding = $format->getEncoding();
```

--------------------------------------------------------

### Protocol

The `protocol` method returns the Protocol object which is used to set the response protocol:
```php
$protocol = $kanso->Response->protocol();
```

To set the response protocol, use the `set` method:
```php
$protocol->set('https');
```

To get the response protocol, use the `get` method:
```php
$proto = $protocol->get();
```

The `isSecure` method checks if the response will sent over SSL:
```php
if (!$protocol->isSecure())
{
    # No https
}
```

--------------------------------------------------------

### Cache

The response cache component serves as a way of caching the HTTP response body over multiple requests.

The idea here is that when enabled, performance can be significantly improved as the body is loaded as a string from file - rather than being constructed via PHP.

The `cache` method returns the response Cache:
```php
$cache = $kanso->Response->cache();
```

The `enable` method enables the response cache:
```php
$cache->enable();
```

The `disable` method disables the response cache:
```php
$cache->disable();
```

The `enabled` method checks if the response cache is enabled:
```php
if ($cache->enabled())
{
    # Do something
}
```

The `get` method returns the cached response body as a string if caching is enabled and the response has been cached.
```php
$html = $cache->get();
```

The `put` method sets and saves the current request to the cache
```php
$cache->put($html);
```

The `has` method checks if the current request is cached:

```php
if ($cache->has())
{
    # do something here
}
```

The `delete` method removes the response cache for the current request:

```php
$cache->delete();
```

The `expired` method checks if the current request cache is expired:
```php
if ($cache->expired())
{
    # do something here
}
```

--------------------------------------------------------

### Helpers

The Response object provides a number of helper methods to quickly send common responses.

The `send` method immediately sends the response to the HTTP cleint. It will also filter the response via the [CDN](#/learn-more/CDN) (if enabled) and load the body from the cache (if enabled).

> Note that the sending of the response is handled by the framework internally. You should not need to send the response manually under normal circumstances.

```php
$response->send();
```

> The following methods all throw request exceptions. Please see the [Throwing Exceptions](/getting-started/error-handling#throwing-exceptions) section for more details on output.

The `redirect` method immediately sends an HTTP redirect (302) response to the client and halts the application. No code will be executed after `redirect` is called.
```php
$response->redirect('http://example.com/moved');
```

The redirect method uses a temporary 302 status code by default. You can set a different redirect code via a second parameter:
```php
$response->redirect('http://example.com/moved-permanently', 304);
```

The `notFound` method immediately sends an HTTP not found (404) response to the client and halts the application. No code will be executed after `notFound` is called.
```php
$response->notFound();
```

The `forbidden` method immediately sends an HTTP forbidden (403) response to the client and halts the application. No code will be executed after `forbidden` is called.
```php
$response->forbidden();
```

The `invalidToken` method immediately sends an HTTP Invalid Token (498) response to the client and halts the application. No code will be executed after `invalidToken` is called.
```php
$response->invalidToken();
```

The `methodNotAllowed` method immediately sends a HTTP Method Not Allowed (405) response to the client and halts the application. No code will be executed after `methodNotAllowed` is called.
```php
$response->methodNotAllowed();
```
