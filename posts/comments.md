# Comments

- [Access](#access)
- [Usage](#usage)
- [Provider](#provider)
- [Comment](#comment)

--------------------------------------------------------

Kanso supports nested level comment threads on posts.

Kanso's `CommentManager` class is used throughout the application to manage adding, deleting and interacting with comments. The `CommentManager` also serves as an interface to the `CommentPorivider`.

> Comments are part of the CMS group of services.

--------------------------------------------------------

### Access

You can access the `CommentManager` via the IoC container.
```php
$commentManager = $kanso->CommentManager;
```

--------------------------------------------------------

### Usage

The `create` method creates a new comment and returns a `Comment`:
```php
$comment = $commentManager->create($content, $name, $email, $postid);
```

| Type     | Variable            | Default   | Description                                                |
|----------|---------------------|-----------|------------------------------------------------------------|
| `string` | `$content`          |           | The comment content in plain markdown or text.             |
| `string` | `$name`             |           | The name of the comment author.                            |
| `string` | `$email`            |           | The email address of the comment author.                   |
| `int`    | `$postId`           |           | The post id of the comment.                                |
| `int`    | `$parentId`         | `NULL`    | The id of the parent comment (if the comment is a reply).  |
| `bool`   | `$validate`         | `true`    | Validate the comment as spam.                              |
| `bool`   | `$subscribeThread`  | `true`    | Subscribe the email address to new comment notifications.  |
| `bool`   | `$subscribeReplies` | `true`    | Subscribe the email address to reply notifications.        |
| `bool`   | `$sendEmails`       | `true`    | Send notifacation emails to subcribed comments and admins. |


If `validate` is set to `TRUE` the comment's status will be set to `spam`, `pending` or `approved`:

```php
$comment = $commentManager->create('This is a comment', 'John Doe', 'John@doe.com', 1);

if ($comment->status === 'spam')
{
    # Comment was marked as spam
}
else if ($comment->status === 'pending')
{
    # Comment was flagged for moderation
}
else if ($comment->status === 'approved')
{
    # Comment was approved
}
```

The `byId` method returns a `Comment` object by id (if it exists) or `NULL` if not.

```php
$comment = $commentManager->byId(1);
```

--------------------------------------------------------

### Provider

The `CommentProvider` provides a convenient set of methods managing comment creation and retrieval:
```php
$provider = $commentManager->provider();
```

The `create` method creates and returns a single `Comment` object from an array:
```php
$comment = $provider->create($data);
```

The `byId` method returns a single `Comment` object by id (if it exists) or `FALSE` if not:
```php
$comment = $provider->byId(1);
```

The `byKey` method returns an array of `Comment` objects by column value:
```php
$comments = $provider->byKey('email', 'foo@bar.com');
```

A third `boolean` parameter returns only a single `Comment` (if set to `TRUE`) (`FALSE` by default):
```php
$comment = $provider->byKey('email', 'foo@bar.com', true);
```

--------------------------------------------------------

### Comment

Once you have a reference to `Comment` instance, you can set and get values using PHP's magic methods:
```php
# Set the slug
$comment->email = 'foo@bar.com';

# Get the slug
$email = $comment->email;
```

The `children` method returns an array of direct `Comment` children (if it has any) or an empty array:
```php
$children = $comment->children();
```

The `parent` method returns the parent `Comment` if it exists or `FALSE` if not:
```php
$parent = $comment->parent();
```

The `save` method saves the comment:
```php
$comment->save();
```

The `delete` method deletes the comment and all children:
```php
$comment->delete();
```