# Thumbnails

- [Access](#access)
- [Usage](#usage)
- [Provider](#provider)
- [Media](#media)

--------------------------------------------------------

Kanso's `MediaManager` class is used throughout the application to manage adding, deleting and interacting with media uploads. The `MediaManager` also serves as an interface to the `MediaPorivider`.

> Media are part of the CMS group of services.

--------------------------------------------------------

### Access

You can access the MediaManager via the IoC container:
```php
$mediaManager = $kanso->MediaManager;
```

--------------------------------------------------------

### Usage
To create a new media item, use the `create` method. A `Media` object will be returned:
```php
$media = $mediaManager->create('public/uploads/image.png', 'Landscape Image', 'Image of hills');
```

The `byId` method returns a `Media` object by id (if it exists) or `FALSE` if not.
```php
$media = $mediaManager->byId(1);
```

The `upload` method will validate an uploaded a file. If the upload is successful an instance of `Media` instance will be returned or a response code if it fails:
```php
$media = $mediaManager->upload($FILE, 'Landscape Image', 'Image of hills');
```

To skip validation, pass an extra parameter as `FALSE`
```php
$media = $mediaManager->upload($FILE, 'Landscape Image', 'Image of hills', false);
```

The possible status codes for erroneous uploads are:

| Constant                         | Description                                                         |
|----------------------------------|---------------------------------------------------------------------|
| `MediaManager::CORRUPT_FILE`     | The file had an error while uploading or is not a valid PHP upload. |
| `MediaManager::UNSUPPORTED_TYPE` | The file is unsupported and not allowed to be uploaded.             |


```php
$media = $mediaManager->upload($FILE, 'Landscape Image', 'Image of hills');

if ($media === $medialibrary::CORRUPT_FILE || !$media)
{
    echo 'The file is invalid!';
}
else if ($media === $medialibrary::UNSUPPORTED_TYPE)
{
    echo 'You cannot upload files of this type!';
}
else if ($media instanceof Media)
{
    echo 'The file was successfully uploaded and can be viewed at: '.$media->url;
}
```

--------------------------------------------------------

### Provider

The `MediaProvider` provides a convenient set of methods for managing media creation and retrieval:
```php
$provider = $mediaManager->provider();
```

The `byId` method returns a single `Media` object by id (if it exists) or `FALSE` if not:
```php
$media = $provider->byId(1);
```

The `byKey` method returns an array of `Media` objects by column value:
```php
$media = $provider->byKey('title', 'Foobar');
```

A third `boolean` parameter will return only a single `Media` instance (if set to `TRUE`) (`FALSE` by default):
```php
$media = $provider->byKey('title', 'Foobar', true);
```

The `create` method creates a new `Media` item based on an array:
```php
$media = $provider->create($row);
```

--------------------------------------------------------

### Media

Once you have a reference to `Media` instance, you can set and get values using PHP's magic methods:
```php
# Set the slug
$media->title = 'Foobar';

# Get the slug
$title = $media->title;
```

The `isImage` method returns `TRUE` if the `Media` is an image or `FALSE` if not:
```php
if ($media->isImage())
{
}
```

The `ext` method returns the file type extension:
```php
if ($media->ext() === 'png')
{
}
```

The `imgSize` method returns the URL for the item at various thumbnail sizes:
```php
$url = $media->imgSize('small');
```

The `imgSizePath` method returns the system file path for the item at various thumbnail sizes:
```php
$path = $media->imgSizePath('small');
```

> Thumbnail sizes are defined in the `app/configurations/cms.php` file under `uploads.thumbnail_sizes`.

If the media is a valid image, the `width` and `height` methods will return the image dimensions in pixels:
```php
$height = $media->height();
$width = $media->width();
```

You can also pass a thumbnail size:
```php
$height = $media->height('small');
$width = $media->width('small');
```

The `save` method saves the media item:
```php
$media->save();
```

The `delete` method deletes the media from the database and the file(s):
```php
$media->delete();
```