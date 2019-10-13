# Gatekeeper

--------------------------------------------------------

-   [Access](#access)
-   [Usage](#usage)

--------------------------------------------------------

Kanso provides a number of utilities and classes for authentication. This section provides documentation on Kanso's `Gatekeeper` class which is Kanso's core implementation for user authentication.

--------------------------------------------------------

### Access

Kanso's Gatekeeper class acts as Kanso's core implementation for all user authentication. You can access the Gatekeeper class directly through the IoC container.

```php
$gatekeeper = $kanso->Gatekeeper;
```

--------------------------------------------------------

### Usage

The Gatekeeper provides the following rudimentary validation methods.

The `getUser` method returns a `User` object if they are logged in. The object is a wrapper around the user table in the database:
```php
$user = $gatekeeper->getUser();
```

To validate if a user is logged in use the `isLoggedIn` method:
```php
if ($gatekeeper->isLoggedIn())
{
    # Do something
}
```

Similarly you can validate if a user is not allowed to access the admin panel in with the `isGuest` method:
```php
if ($gatekeeper->isGuest())
{
    # Do something
}
```

Or use the `isAdmin` method to check if the user is an `administrator` or `writer`:
```php
if ($gatekeeper->isAdmin())
{
    # Do something
}
```

The `verifyToken` method validates a user's access token. The token is stored as a cookie. If the user is logged in, the token is also cross referenced with the database:
```php
if ($gatekeeper->verifyToken($token))
{
    # Do something
}
```

The `refreshUser` method refreshed the gatekeeper's current user data. This is useful if you change a user's data:
```php
$gatekeeper->refreshUser();
```

The `login` method attempts to log a user in based on their username and password. The method will return `TRUE` on success and a status code if not.

```php
$success = $gatekeeper->login($username, $password);
```

The possible status codes for failed logins:

| Constant                       | Description                              |
|--------------------------------|------------------------------------------|
| `Gatekeeper::LOGIN_INCORRECT`  | The username or password was incorrect.  |
| `Gatekeeper::LOGIN_ACTIVATING` | The account is not yet activated.        |
| `Gatekeeper::LOGIN_LOCKED`     | The account is currently locked.         |
| `Gatekeeper::LOGIN_BANNED`     | The account has been permanently banned. |


```php
$login = $gatekeeper->login($username, $password);

if ($login === Gatekeeper::LOGIN_INCORRECT)
{
    # Your password or username was incorrect.
}
else if ($login === Gatekeeper::LOGIN_ACTIVATING)
{
    # Your account is not yet activated
}
else if ($login === Gatekeeper::LOGIN_LOCKED)
{
    # Your account has been locked
}
else if ($login === Gatekeeper::LOGIN_BANNED)
{
    # Your account has been banned
}
else if ($login === true)
{
    # Success you logged in
}
```

You can pass a third parameter as `TRUE` to force the login, regardless of the password:
```php
$gatekeeper->login($username, $password, true);
```

The `logout` method will attempt to log the current user out:
```php
$gatekeeper->logout();
```

> You can change the default urls for user emails in the `app/configurations/email.php` file under `urls`.

The `forgotPassword` method will validate a forgot password request and send the appropriate emails so the user can reset their password. If the user is an administrator they will be sent a link to the admin panel reset password page. Otherwise they will be sent a link to `example.com/reset-password/?token=` with their reset token.

```php
$success = $gatekeeper->forgotPassword($username);
```

The `resetPassword` method allows you to to reset a user's password based on their token. The user's reset token must be provided. The method will return `TRUE` on success and `FALSE` if the reset fails.

```php
$reset = $gatekeeper->resetPassword($password, $token);
```

The `forgotUsername` method will send a user a reminder of their username via email. The method will return `TRUE` on success and `FALSE` if the user doesn't exist.

```php
$sendReminder = $gatekeeper->forgotUsername($email);
```
