# Custom Posts

- [Creating](#creating)
- [Managing](#managing)

--------------------------------------------------------

You can easily register a custom post type with Kanso. This allows you to create posts for various other applications facets like a forum or products for example. Creating and managing customs posts is very simple and straight forward.

> Custom posts are part of the CMS group of services.

--------------------------------------------------------

### Creating

To register a custom post type, use the `registerPostType` method on Kanso's `Admin` property:
```php
$kanso->Admin->registerPostType('Product', 'product', 'shopping-cart', 'products/(:year)/(:month)/(:postname)/');
```

| Parameter   | Type     | Description                                                                                                                                                                                                                |
|-------------|----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `$title`    | `string` | This will dictate the title of the page in the admin panel, as well as the text that goes in the dropdown menu in the writer post-type select menu.                                                                        |
| `$type`     | `string` | The post-type value. This is used for the type column in the post table. It is also used as the template file to load in the theme. In the example above, Kanso will try to load `single-product.php` before `single.php.` |
| `$icon`     | `string` | The icon suffix that will go in the admin panel sidebar. Use any [FontAwesome](https://fontawesome.com/) icon suffix.                                                                                                      |
| `$route`    | `string` | This is where Kanso will try to load the posts when route matching. It also defines the permalink structure to use.                                                                                                        |

--------------------------------------------------------

### Managing

Once you have created a custom post type, you will be able to manage the posts via the admin panel the same way you would other post types. Kanso will create all the necessary pages for you.

You can manage custom posts the same way you interact with regular posts. See the [Posts](/posts/posts.md) documentation for more details.