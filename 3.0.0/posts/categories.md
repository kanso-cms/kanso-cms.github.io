# Categories

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Provider](#provider)
- [Category](#category)

--------------------------------------------------------

Kanso categories are used as a taxonomy for posts. In Kanso, each post can have multiple categories and each category can have an optional parent category.

Kanso's `CategoryManager` class is used throughout the application to manage adding, deleting and interacting with categories. The `CategoryManager` also serves as an interface to the `CategoryPorivider`.

> Categories are part of the CMS group of services.

--------------------------------------------------------

### Accessing the CategoryManager

You can access the `CategoryManager` via the IoC container.
```php
$categoryManager = $kanso->CategoryManager;
```

--------------------------------------------------------

### Usage

To create a new category, use the `create` method. A new `Category` will be returned on success or `FALSE` if the category already exists:
```php
$category = $categoryManager->create($name, $slug);
```

The `slug` is optional and will be automatically generated if not provided:
```php
$category = $categoryManager->create($name);
```

The `byId` method returns a `Category` by `id` (if it exists) or `FALSE` if not:
```php
$category = $categoryManager->byId(1);
```

The `byName` method returns a `Category` by `name` (if it exists) or `FALSE` if not:
```php
$category = $categoryManager->byName('CSS');
```

The `bySlug` method returns a `Category` by `slug` (if it exists) or `FALSE` if not:
```php
$category = $categoryManager->bySlug('css');
```

The `delete` method returns deletes a category by `name`, `slug` or `id`:
```php
# Delete by id
$categoryManager->delete(3);

# Delete by name
$categoryManager->delete('css');

# Delete by slug
$categoryManager->delete('category-slug');
```

> Not that when a category is deleted, any child categories will also be deleted. Any posts under these categories will automatically be set to `Uncategorized`.

--------------------------------------------------------

### Provider

The `CategoryProvider` provides a convenient set of methods for managing category creation and retrieval.
```php
$provider = $categoryManager->provider();
```

The `byId` method returns a single `Category` by `id` (if it exists) or `FALSE` if not:
```php
$category = $provider->byId(1);
```

The `byKey` method returns an array of `Category` objects by column value:
```php
$categories = $provider->byKey('slug', 'foobar');
```

A third `boolean` parameter defines to return only a single category (if set to `TRUE`) (`FALSE` by default):
```php
$category = $provider->byKey('slug', 'foo', true);
```

The `create` method creates a new `Category` based on an array:
```php
$category = $provider->create(['name' => 'foo', 'slug' => 'bar']);
```

--------------------------------------------------------

### Category

Once you have a reference to a `Category`, you can set and get values using PHP's magic methods:
```php
# Set the slug
$category->slug = 'foobar';

# Get the slug
$slug = $category->slug;
```

The `parent` method returns the parent `Category` if it has one or `FALSE` if not:
```php
$parent = $category->parent();
```

The `children` method returns an array of `Category` objects that are direct children or an empty array if it doesn't have any:
```php
$children = $category->children();
```

The `clear` method removes all posts from the category and sets them to `Uncategorized`:
```php
$category->clear();
```

The `save` method saves the category:
```php
$category->save();
```

The `delete` method deletes the category:
```php
$category->delete();
```

> Not that when a category is deleted, any child categories will also be deleted. Any posts under these categories will automatically be set to `Uncategorized`.
