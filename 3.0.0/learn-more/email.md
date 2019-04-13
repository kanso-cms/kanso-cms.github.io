# Email

- [Configuration](#configuration)
- [Access](#access)
- [Usage](#usage)
- [theme](#theme)

--------------------------------------------------------

Kanso's `Email` class is used throughout the application to send consistently styled HTML emails for various user actions.

All emails sent via the email service are logged and available in the admin panel.

> The Email service is part of the CMS group of services.

--------------------------------------------------------

### Configuration

Configuration for the `Email` services are in the `app/configurations/email.php` file.

--------------------------------------------------------

### Access

You can access The `Email` class via the IoC container.
```php
$email = $kanso->Email;
```

--------------------------------------------------------

### Usage

The `html` method will load Kanso's default HTML email body with the content you provide inserted into the body. The content you provide should be in HTML format:
```php
$html = $email->html('My Email Subject', '<h2>Hello World</h2>');
```

The `button` method provides a handy and quick way to load a styled button:
```php
$btn = $email->button('http://example.com', 'Click Me');
```

An optional third paramter can set the size of the button. Possible sizes are `xs`, `sm`, `md`, `lg`, `xl`. The method defaults to `md`:
```php
$btn = $email->button('http://example.com', 'Click Me', 'xl');
```

The `send` method sends an HTML or plain text email and returns a boolean. The last parameter is optional defining the format to send (defaults to HTML):
```php
# Send an HTML email
$sent = $email->send('foo@bar.com', 'Kanso', 'no-reply@kanso.com', 'My Subject', 'HTML OR PLAIN TEXT CONTENT');

# Send a plain text
$sent = $email->send('foo@bar.com', 'Kanso', 'no-reply@kanso.com', 'My Subject', 'HTML OR PLAIN TEXT CONTENT', false);
```

--------------------------------------------------------

### Theme

You can customize the styling of HTML emails using the `theme` method:
```php
$email->theme(['line_height' => '30px']);
```

The `theme` method will also return the current theme as an associative array:
```php
$theme = $email->theme();
```

For a full list of theme options, see the `app/configurations/email.php` file.