# Email

- [Configuration](#configuration)
	- [Native](#native)
	- [SMTP Login](#smtp-login)
	- [SMTP XOAUTH2](#smtp-XOAUTH2)
- [Access](#access)
- [Usage](#usage)
- [theme](#theme)
- [Log](#log)
- [Queue](#queue)

--------------------------------------------------------

Kanso's `Email` class is used throughout the application to send consistently styled HTML emails for various user actions.

All emails sent via the email service are logged and available in the admin panel.

> The Email service is part of the CMS group of services.

--------------------------------------------------------

### Configuration

Configuration for the `Email` services are in the `app/configurations/email.php` file.


The `Email`component by default will send emails using PHP's native `mail` function. However, it also has the ability to send emails via both `SMTP` and `XOAUTH2` with gmail.

#### Native
To send emails using PHP's native `mail` function, set `use_smtp` to `false`.

#### SMTP Login
To send emails via regular unsecured `SMTP` with gmail:

1. Set the `use_smtp` key to `true` and `smtp_settings.auth_type` to `LOGIN`.
2. Set `smtp_settings.username` and `smtp_settings.hashed_pass` to their respective values. You will need to set the `smtp_settings.hashed_pass` to your gmail password hashed via the framework's `Crypto->encrypt()` method.
3. Finally you will need to enable Less secure apps & your Google Account. For details please see this link [here](https://support.google.com/accounts/answer/6010255?hl=en).

#### SMTP XOAUTH2
Using XOAUTH2 via gmail is the most secure way to send emails with Kanso. However the setup is very complicated. Additionally, authentication with Google is very slow, so it is highly advised that you enable the CMS's email `queue` component and send emails via a `CRON` job.

> See the [CRON Documentation](/learn-more/cron) for more details on CRON jobs.

1. Set the `use_smtp` key to `true` and `smtp_settings.auth_type` to `XOAUTH2`.
2. Set the `client_id`, `client_secret` and `refresh_token`. Instructions on how to generate these can be found [here](https://github.com/PHPMailer/PHPMailer/wiki/Using-Gmail-with-XOAUTH2)
3. Finally, it's strongly recmommended to set `queue` to true.

> An example for Gmail XOAUTH2 setup can be found in the `app/routes/_smtp-xoauth.php` file.

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

--------------------------------------------------------

### Log

The `Email` component logs all sent emails automatically. You can access the log history via the CMS admin panel. You can also re-send logged emails there.

To access the log component directly, use the `log` method:
```php
$log = $kanso->Email->log();
```

If you need to log an email manually, use the `save` method. A string with the id of the log file will be returned:

```php
$log->save('foo@bar.com', 'Kanso', 'info@kanso.com', 'Email Subject', 'HTML OR PLAIN TEXT CONTENT', 'html');
```
To send a plain text email, set the last arguement to `plain`.

--------------------------------------------------------

### Queue

The `Email` component has a built in queue for sending emails via a `CRON` job. When enabled, emails are sent to the queue automatically, but must be processed manually later.

The advantage of using the queue is faster response times on requests when emails are sent. When enabled, rather than sending emails during the the request, emails are added to the queue and sent later. This means you can skip all the slow email authentication for `SMTP`, send the response to the client immediately.

To access the queue component directly, use the `queue` method:
```php
$queue = $kanso->Email->queue();
```

The `enabled` method checks if email queuing is enabled: 
```php
if ($queue->enabled())
{
	// Email queuing is enabled
}
```

The `disabled` method checks if email queuing is disabled: 
```php
if ($queue->disabled())
{
	// Email queuing is disabled
}
```

The `enable` method enables email queuing:
```php
$queue->enable();
```

The `disable` method disables email queuing:
```php
$queue->disable();
```

The `add` method adds an email from the `log` to the queue via the log id:
```php
$id = $log->save('foo@bar.com', 'Kanso', 'info@kanso.com', 'Email Subject', 'HTML OR PLAIN TEXT CONTENT', 'html');

$queue->add($id);
```

The `get` method returns the queue as an array of `log` ids:
```php
$list = $queue->get();
```

The `process` method runs through the queue and sends any emails
```php
 $queue->process();
```

> See the [CRON Documentation](/learn-more/cron) for more details on CRON jobs.