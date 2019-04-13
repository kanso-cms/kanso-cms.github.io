# Filesystem

--------------------------------------------------------

- [Usage](#filesystem)

--------------------------------------------------------

The `Filesystem` class contains methods that assist in working with the file system.

> The Filesystem is part of the Framework group of services.

--------------------------------------------------------

## Usage

You can use the `Filesystem` object statically or fetch the instance present in the IoC container. In the following examples we'll assume that you'll be using the instance from the container.

The `exists` method return `TRUE` if the provided path exists and `FALSE` if not:
```php
$exists = $kanso->Filesystem->exists('/foo/bar.txt');
```

The `isFile` method return `TRUE` if the provided path is a file and `FALSE` if not:
```php
$isFile = $kanso->Filesystem->isFile('/foo/bar.txt');
```

The `isDirectory` method returns `TRUE` if the provided path is a directory and `FALSE` it not:
```php
$isDirectory = $kanso->Filesystem->isDirectory('/foo');
```

The `isReadable` method returns `TRUE` if the provided path is readable and `FALSE` if not:
```php
$isReadable = $kanso->Filesystem->isReadable('/foo/bar.txt');
```

The `isWritable` method returns `TRUE` if the provided path is writable and `FALSE` if not:
```php
$isWritable = $kanso->Filesystem->isWritable('/foo/bar.txt');
```

The `lastModified` method returs the time (unix timestamp) when the data blocks of a file were being written to, that is, the time when the content of the file was changed:
```php
$lastModified = $kanso->Filesystem->lastModified('/foo/bar.txt');
```

The `size` method returns the size of the file in bytes:
```php
$size = $kanso->Filesystem->size('/foo/bar.txt');
```

The `extension` method returns the extension of the file:
```php
$extension = $kanso->Filesystem->extension('/foo/bar.txt');
```

The `mime` method returns the mime type of the file. It returns `FALSE` if the mime type is not found:
```php
$mime = $kanso->Filesystem->mime('/foo/bar.txt');
```

The `delete` method will delete a file from disk:
```php
$kanso->Filesystem->delete('/foo/bar.txt');
```

The `glob` method returns an array of path names matching the provided pattern:
```php
$paths = $kanso->Filesystem->glob('/foo/*.txt');
```

The `getContents` method returns the contents of a file:
```php
$contents = $kanso->Filesystem->getContents('/foo/bar.txt');
```

The `putContents` method puts the provided contents to the file. There's an optional third parameter that will set an exclusive write lock if set to `TRUE`:
```php
$kanso->Filesystem->putContents('/foo/bar.txt', 'hello, world!');
```

The `prependContents` method will prepend the provided conetents to the file. There's an optional third parameter that will set an exclusive write lock if set to `TRUE`:
```php
$kanso->Filesystem->prependContents('/foo/bar.txt', 'hello, world!');
```

The `appendContents` method will append the provided conetents to the file. There's an optional third parameter that will set an exclusive write lock if set to `TRUE`:
```php
$kanso->Filesystem->appendContents('/foo/bar.txt', 'hello, world!');
```

The `truncateContents` method will truncate the contents of the file. There's an optional third parameter that will set an exclusive write lock if set to `TRUE`:
```php
$kanso->Filesystem->truncateContents('/foo/bar.txt');
```

The `includeFile` method will include a file:
```php
$kanso->Filesystem->includeFile('/foo/bar.txt');
```

The `includeFileOnce` method will include a file if it hasn't already been included:
```php
$kanso->Filesystem->includeFileOnce('/foo/bar.txt');
```

The `requireFile` method will require a file:
```php
$kanso->Filesystem->requireFile('/foo/bar.txt');
```

The `requireFileOnce` method will require a file if it hasn't already been required:
```php
$kanso->Filesystem->requireFileOnce('/foo/bar.txt');
```

The `file` method will return a [SplFileObject](http://php.net/manual/en/class.splfileobject.php):
```php
$file = $kanso->Filesystem->file('/foo/bar.txt', 'r');
```

The `tmpfile` method creates a temporary file and returns it's path:
```php
$kanso->Filesystem->tmpfile();
```

The `ob_read` method includes a file and returns it's output:
```php
$kanso->Filesystem->ob_read('/foo/bar.txt');

$kanso->Filesystem->ob_read('/foo/bar.txt', ['foo' => 'bar']);
```
