# Users

--------------------------------------------------------

- [Roles](#roles)
	- [Admins](#admins)
	- [Writers](#writers)
	- [Guests](#guests)
- [Registration](#registration)
- [Creating Users](#creating-users)
	- [New Admins](#new-admins)
	- [New Users](#new-admins)
	- [Status Codes](#status-codes)
	- [Management](#management)
- [User Provider](#user-provider)

--------------------------------------------------------

Users are defined by their `role` value in the database. Users are used throughout the CMS for both front-end and back-end management.

> Users are part of the CMS service.

--------------------------------------------------------

### Roles

In kanso, users are given privalages to the CMS based on their role. There are two types of admins - `administrators` and `writers`. Both these user types have access to the Admin panel.

Any user that does not have a role of either `administrator` or `writer` is considered a guest and cannot access the admin panel.

Kanso runs on an invite-only scheme - The only way a new an admin can be added, is if they are are invited by an Administrator via the admin panel.

> You can define as many roles as you like.

#### Admins

Administrators, as the name suggests, have absolute admin privileges - They can edit posts, tags, categories, moderate comments, add or delete users, change Kanso settings and any other option in the admin Panel.

#### Writers

Writers can edit, delete and create posts, categories and tags as well as update their own author information. Writers cannot edit or change any of Kanso's configuration settings.

#### Guests

Any other user role cannot access the admin panel but can still be managed via the `UserManager`.

--------------------------------------------------------

### Registration

When registering a new user via the Admin Panel, the registration process is as follows:

1. A new entry is made in the database with the user's email address. A username and password is generated for them.
2. An email is sent to the user with their login credentials.
3. The user must accept their invitation via email to activate their account.

--------------------------------------------------------

### Creating Users

Users are managed via the `UserManager`. You can access the `UserManager` class directly through the IoC container:
```php
$userManager = $kanso->Gatekeeper;
```

#### New Admins

The `createAdmin` method attempts to register a new administrator. The method will return a `User` instance on success and a status code if not:
```php
$admin = $userManager->createAdmin($email, $role);
```

The method will send the admin their credentials via email by default. You can override this behavior by providing a third `boolean` parameter:
```php
$admin = $userManager->createAdmin($email, $role, false);
```

#### New Users

The `create` method attempts to create a new user. The method will return a `User` instance on success and a status code if not:
```php
$user = $userManager->create($email, $username, $password, 'guest');
```

New users are set to `pending` by default. You can override this behavior by providing a fifth `boolean` parameter:
```php
$user = $userManager->create($email, $username, $password, 'guest', true);
```

To register a new user with included email verification, use the `createUser` method. The method will return a `User` instance on success and a status code if not:
```php
$user = $userManager->createUser($email, $password, $name, $username, 'guest');
```

The `createUser` method will set the new user to `pending` by default. You can override this behavior and activate them by providing a fifth `boolean` parameter:
```php
$user = $userManager->createUser($email, $password, $name, $username, 'guest', false);
```

The method will also send them an email to verify their email address if their account is set to pending. You can override this behavior and activate them by providing a sixth `boolean` parameter:
```php
$user = $userManager->createUser($email, $password, $name, $username, 'guest', true, false);
```

> Vefification emails for non admins are sent a link to `example.com/login/`. You can change this URL in the `app/configurations/email.php` file under `urls`.

#### Status Codes

The possible status codes for failed new users are:

| Constant                       | Description                                 |
|--------------------------------|---------------------------------------------|
| `UserManager::USERNAME_EXISTS` | Another user with the same username exists. |
| `UserManager::SLUG_EXISTS`     | Another user with the same slug exists.     |
| `UserManager::EMAIL_EXISTS`    | Another user with the same email exists.    |

```php
$user = $userManager->create($email, $username, $password, 'guest');

if ($user === $userManager::USERNAME_EXISTS)
{
	// Username taken
}
else if ($user === $userManager::SLUG_EXISTS)
{
	// Slug taken
}
else if ($user === $userManager::EMAIL_EXISTS)
{
	// Email taken
}
else if ($user instanceof User)
{
	// Success
}
```

#### Management

The `byId` method returns a single user by `id` if it exists or `FALSE` if not:
```php
$user = $userManager->byId(5);
```

The `byEmail` method returns a single user by `email` if it exists or `FALSE` if not:
```php
$user = $userManager->byEmail('foo@bar.com');
```

The `byUsername` method returns a single user by `username` if it exists or `FALSE` if not:
```php
$user = $userManager->byUsername('johndoe');
```

The `byToken` method returns a single user by their CSRF `token` if it exists or `FALSE` if not:
```php
$user = $userManager->byToken('csrf-token');
```

The `activate` method allows you to activate an existing unconfirmed user by their activation token. The method will return `TRUE` on success and `FALSE` if the activation fails:

```php
$activated = $userManager->activate($token);
```

The `delete` method allows you to delete an existing user by `id`, `email` or `username`. The method will return `TRUE` on success and `FALSE` if the user doesn't exist:

```php
// Delete by id
$deleted = $userManager->delete(3);

// Delete by email
$deleted = $userManager->delete('foo@bar.com');

// Delete by username
$deleted = $userManager->delete('johndoe');
```

--------------------------------------------------------

### User Provider

The `provider` method returns the CMS user provider:
```php
$userProvider = $userManager->provider();
```

The `byId` method returns a single user class by id:
```php
$user = $userProvider->byId(1);
```

The `byKey` method returns an array of users by key/value from the database `users` table. The third argument defines to limit the results to single row:
```php
# Get the first user with name is 'John Doe'
$users = $userProvider->byKey('name', 'John Doe' true);

# Get all the users that under name 'John Doe'
$users = $userProvider->byKey('name', 'John Doe');
```

Once you have reference to a user, you can set values user PHP's magic methods. The `save` method will save the user:
```php
$user->name  = 'John Doe';
$user->email = 'John@doe.com';
$user->save();
```

The `delete` method will delete the user:
```php
$user->delete();
```

The `generateAccessToken` method generates a new access token for the user and saves it to the database:
```php
$user->generateAccessToken();

$token = $user->access_token;
```