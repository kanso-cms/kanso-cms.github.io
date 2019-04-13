# UserAgent

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Kanso's `UserAgent` manager is used to check if the current UserAgent is a bot.

--------------------------------------------------------

### Access

The cookie manager can be accessed via the `Response` object
```php
$agent = $kanso->UserAgent;
```

--------------------------------------------------------

### Usage

To check if the request is from a bot, use the `isCrawler` method:
```php
if ($agent->isCrawler())
{
	# Bot request
}
```

You can optionally pass your own user agent string
```php
if ($agent->isCrawler('google-bot'))
{
	# Bot request
}
```