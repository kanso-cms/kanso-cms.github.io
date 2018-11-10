# Admin Panel

--------------------------------------------------------

- [Access](#access)
- [Pages](#pages)
- [Custom Posts](#custom-posts)
- [Custom CSS and JS](#custom-css-and-js)
- [Filters](#filters)
- [Media Library](#)

--------------------------------------------------------

Kanso's Admin service allow you to interact with and customize the Admin Panel a number of ways.

> The Admin Panel is part of the CMS group of services.

--------------------------------------------------------

The `Admin` class can be accessed directly via the IoC container:

```php
$admin = $kanso->Admin;
```

--------------------------------------------------------

### Pages

The `addPage()` method adds a new page to the Admin Panel:

```php
$kanso->Admin->addPage('Integrations', 'integrations', 'plug', '\app\models\admin\Integrations', APP_DIR.'/views/admin/page-integrations.php', null, true);
```

| Parameter    | Type                              | Default |  Description                                                                              |
|--------------|-----------------------------------|---------|-------------------------------------------------------------------------------------------|
| `$pageTitle` | string                            |         | The name of the page on the admin sidebar as well as the HTML title.                      |
| `$slug`      | string                            |         | The url slug for the page.                                                                |
| `$icon`      | string                            |         | The icon suffix of the sidebar menu item. Use any FontAwesome icon suffix.                |
| `$model`     | string                            |         | A string representation of a class model to load.                                         |
| `$view`      | string                            |         | Full file path to view template to load into the page content.                            |
| `$parent`    | string                            | `null`  | The parent admin page slug. This will make new page a child of the parent in the sidebar. |
| `$adminOnly` | bool                              | `false` | Make the page only accessible to admins (not writers).                                    |
| `$styles`    | array                             | `null`  | Array of stylesheets to add to the Admin Panel's head.                                    |
| `$scripts`   | array                             | `null`  | Array of scripts to add to before the Admin Panel's closing body tag.                     |

> When you add a page, the sidebar is automatically updated to include your new page.

--------------------------------------------------------

### Custom Posts

Please see the [Custom Post Types](/posts/custom) section for more details.

--------------------------------------------------------

### Custom CSS and JS

You can add custom scripts or stylesheets to Admin Panel using Kanso's built in filters. Use the `adminHeaderScripts` filter to add your own `CSS` in the `HEAD`:

```php
$kanso->Filters->on('adminHeaderScripts', function($CSS)
{
    $CSS[] = '<link href="/url/path/styles.css" rel="stylesheet">';

    return $CSS;
});
```

Use the `adminFooterScripts` filter to add your own `scripts`. They will be appended before the closing `body` tag:

```php
$kanso->Filters->on('adminFooterScripts', function($JS)
{
    $JS[] = '<script src="/url/path/scripts.js"></script>';

    return $JS;
});
```

--------------------------------------------------------

### Filters

Kanso's built in filters allow you to customise the output of the Admin Panel.

The `adminPageTitle` filter lets you change the page title:
```php
$kanso->Filters->on('adminPageTitle', function($title)
{
    return 'New Title';
});
```

The `adminSidebar` filter lets you change the built in sidebar:
```php
$kanso->Filters->on('adminSidebar', function($sidebar)
{
    $sidebar['custom'] = [
        'link'     => '/admin/custom/',
        'text'     => 'Custom',
        'icon'     => 'font-awesome',
        'children' =>
        [
            'customChild' =>
            [
                'link'     => '/admin/custom/nested/',
                'text'     => 'Nested',
                'icon'     => 'font-awesome',
            ],
        ],
    ];

    return $sidebar
});
```

--------------------------------------------------------

### Media Library

You can use Kanso's built in Media Library inside any custom page's view using the `admin_media_library` function:
```php
echo admin_media_library();
```

The Media Library will be hidden by default. To display it, simply create a trigger:

```html
<button type="button" class="js-show-media-lib">Media Library</button>
```

You can also set your Media Library to be displayed by default:

```html
<script>
    Modules.get('MediaLibrary')._showLibrary();
</script>
```

The Media Library can be closed via the default `Select Image` button. You can use this button in your own setup by adding an event listener on it.

```html
<script>
    var setImgTrigger = Helper.$('.js-set-image-trigger');

    setImgTrigger.addEventListener('click', function(e)
    {
        // Prevent form submition in some browsers
        e = e || window.event;
        e.preventDefault();

        // Hide the Media Library
        Modules.get('MediaLibrary')._hideLibrary();

        // Get the selected image id
        console.log(Helper.$('#media_id').value);

        // Get the selected media URL
        Helper.$('#media_url').value;

        // Do whatever you want with the image here
    });
</script>
```