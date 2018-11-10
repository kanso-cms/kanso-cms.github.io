# Post Manager

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Provider](#provider)
- [Posts](#posts)
    - [Tags](#tags)
    - [Categories](#categories)
    - [Author](#author)
    - [Thumbnail](#thumbnail)
    - [Comments](#comments)

--------------------------------------------------------

Kanso's `PostManager` class is used throughout the application to manage adding, deleting and interacting with posts. The `PostManager` also serves as an interface to the `PostProvider`.

> Posts are part of the CMS group of services.

--------------------------------------------------------

### Access

You can access the PostManager via the IoC container:
```php
$postManager = $kanso->PostManager;
```

--------------------------------------------------------

### Usage

To create a new post, use the `create` method. A `Post` instance will be returned on success or `FALSE` if there was an error
```php
$post = $postManager->create($row);
```

The `byId` method returns a single `Post` by id (if it exists) or `FALSE` if not:
```php
$post = $postManager->byId(1);
```

The `delete` method deletes a `Post` by id:
```php
$post = $postManager->byId(1);
```

--------------------------------------------------------

### Provider

The post provider provides a convenient set of methods managing post creation and retrieval.

```php
$provider = $postManager->provider();
```

The `byId` method returns a single `Post` by post id (if it exists) or `FALSE` if not.

```php
$post = $provider->byId(1);
```

The `byKey` method returns an array of `Post`s by column value.
```php
$posts = $provider->byKey('author_id', 1);
```

A third `boolean` parameter will return only a single post (if set to `TRUE`):
```php
$post = $provider->byKey('author_id', 1, true);
```

A fourth `boolean` parameter will only include published post (`TRUE` by default) or include drafts when set to `FALSE`:
```php
$includeDrafts = $provider->byKey('author_id', 1, true, false);
```

The `create` method creates a new `Post` based on an array.

```php
$post = $provider->create($row);
```

--------------------------------------------------------

### Posts

Once you have a reference to a `Post`, you can set and get values using PHP's magic methods:
```php
# Set the title
$post->title = 'Foo Bar';

# Get the title
$title = $post->title;
```

You can set values for the columns that exists in Kanso's `posts` table:
```php
$row = [
    'created'     => 'INTEGER | UNSIGNED',
    'modified'    => 'INTEGER | UNSIGNED',
    'status'      => 'VARCHAR(255)',
    'type'        => 'VARCHAR(255)',
    'slug'        => 'VARCHAR(255)',
    'title'       => 'VARCHAR(255)',
    'meta'        => 'VARCHAR(255)',
    'excerpt'     => 'TEXT',
    'author_id'   => 'INTEGER | UNSIGNED',
    'category_id' => 'INTEGER | UNSIGNED',
    'thumbnail_id' => 'INTEGER | UNSIGNED',
    'comments_enabled' => 'BOOLEAN DEFAULT FALSE'
];
foreach ($row as $key => $value)
{
    $post->$key = $value;
}
```

The `content` key will set and get the post content:
```php
$post->content = 'Hello World!';
```

The `delete` method deletes the post permanently:
```php
$post->delete();
```

The `save` method saves the post:
```php
$post->save();
```

--------------------------------------------------------

### Tags

You can set the tags as a comma separated list of tag names, array of tag names, or array of [Tag objects](/posts/tags).

```php
# Comma list
$post->tags = 'tag 1, tag 2, tag 3';

# Array list
$post->tags = ['tag 1', 'tag 2', 'tag 3'];

# Array of tag objects
$post->tags = [$tagObject1, $tagObject2];
```

When you retrieve a post's tags, you will receive an `array` of [Tag objects](/posts/tags).

```php
$tags = $post->tags;
```

> After setting tags on a post, if the tags do not yet exist when the post is saved, they will be automatically created.

--------------------------------------------------------

### Categories

You can set the categories as a comma separated list of category names, array of tag names, or array of [Category objects](/posts/categories).

```php
# Comma list
$post->categories = 'cat 1, cat 2, cat 3';

# Array list
$post->categories = ['cat 1', 'cat 2', 'cat 3'];

# Array of category objects
$post->categories = [$catObject1, $catObject2];
```

When you retrieve a post's categories, you will receive an `array` of [Category objects](/posts/categories).

```php
$categories = $post->categories;
```

> After setting the category on a post, if the category does not yet exist when the post is saved, it will be automatically created.

--------------------------------------------------------

### Author

You can set the author by `name`, `id` or [User object](/users/users)

```php
# By name
$post->author = 'John Doe';

# By id
$post->author = 1;

# By Object
$post->author = $userObject;
```

When you retrieve a post's author, you will receive a [User object](/users/users).

```php
$author = $post->author;
```

--------------------------------------------------------

### Thumbnail

You can set the thumbnail by the `thumbnail_id` which references an [Attachment object](/media/attachment).

```php
# By id
$post->thumbnail_id = 1;
```

When you retrieve a post's thumbnail, you will receive an [Attachment object](/media/attachment) or `NULL` if it doesn't have one.

```php
$thumbnail = $post->thumbnail;
```

--------------------------------------------------------

### Comments

The `comments` key will return an array of [Comment](/comments/comments/) objects. You cannot set comments this way however, you should use the [CommentManager](/comments/comments/) instead.

```php
$comments = $post->comments;
```