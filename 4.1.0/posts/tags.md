# Tags

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Provider](#provider)
- [Tag](#tag)

--------------------------------------------------------

Kanso tags are used as a taxonomy for posts. In Kanso, each post can have multiple tags.

Kanso's `TagManager` class is used throughout the application to manage adding, deleting and interacting with tags. The `TagManager` also serves as an interface to the `TagPorivider`.

> Tags are part of the CMS group of services.

--------------------------------------------------------

### Accessing the TagManager

You can access the `TagManager` via the IoC container.
```php
$tagManager = $kanso->TagManager;
```

--------------------------------------------------------

### Usage

To create a new tag, use the `create` method. A new `Tag` will be returned on success or `FALSE` if the tag already exists:
```php
$tag = $tagManager->create($name, $slug);
```

The `slug` is optional and will be automatically generated if not provided:
```php
$tag = $tagManager->create($name);
```

The `byId` method returns a `Tag` by `id` (if it exists) or `FALSE` if not:
```php
$tag = $tagManager->byId(1);
```

The `byName` method returns a `Tag` by `name` (if it exists) or `FALSE` if not:
```php
$tag = $tagManager->byName('CSS');
```

The `bySlug` method returns a `Tag` by `slug` (if it exists) or `FALSE` if not:
```php
$tag = $tagManager->bySlug('css');
```

The `delete` method returns deletes a tag by `name`, `slug` or `id`:
```php
# Delete by id
$tagManager->delete(3);

# Delete by name
$tagManager->delete('css');

# Delete by slug
$tagManager->delete('tag-slug');
```

> Not that when a tag is deleted any posts will automatically be set to `Untagged`.

--------------------------------------------------------

### Provider

The `TagProvider` provides a convenient set of methods for managing tag creation and retrieval.
```php
$provider = $tagManager->provider();
```

The `byId` method returns a single `Tag` by `id` (if it exists) or `FALSE` if not:
```php
$tag = $provider->byId(1);
```

The `byKey` method returns an array of `Tag` objects by column value:
```php
$tags = $provider->byKey('slug', 'foobar');
```

A third `boolean` parameter defines to return only a single tag (if set to `TRUE`) (`FALSE` by default):
```php
$tag = $provider->byKey('slug', 'foo', true);
```

The `create` method creates a new `Tag` based on an array:
```php
$tag = $provider->create(['name' => 'foo', 'slug' => 'bar']);
```

--------------------------------------------------------

### Tag

Once you have a reference to a `Tag`, you can set and get values using PHP's magic methods:
```php
# Set the slug
$tag->slug = 'foobar';

# Get the slug
$slug = $tag->slug;
```

The `parent` method returns the parent `Tag` if it has one or `FALSE` if not:
```php
$parent = $tag->parent();
```

The `children` method returns an array of `Tag` objects that are direct children or an empty array if it doesn't have any:
```php
$children = $tag->children();
```

The `clear` method removes all posts from the tag and sets them to `Untagged`:
```php
$tag->clear();
```

The `save` method saves the tag:
```php
$tag->save();
```

The `delete` method deletes the tag:
```php
$tag->delete();
```

> Not that when a tag is deleted any posts under will automatically be set to `Untagged`.
