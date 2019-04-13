# Visitors

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Manager](#manager)
- [Provider](#provider)
- [Visitor](#visitor)
- [Visit](#visit)

--------------------------------------------------------

Kanso's `Crm` service is used to manage and track site visitors across your application. This allows you to monitor how visitors are interacting with your site.

The CRM will automatically manages visitors throughout the application.

> Visitors are part of the CMS group of services.

--------------------------------------------------------

### Crm

The `Crm` can be accessed via the the IoC container:
```php
$crm = $kanso->Crm;
```

--------------------------------------------------------

### Usage

The `visitor` method returns the current site visitor:
```php
$visitor = $crm->visitor();
```

The `login` method links the current CRM visitor with a logged in CMS user.
```php
$crm->login();
```

The `logout` method links the current CRM visitor with a logged out in CMS user.
```php
$crm->logout();
```

--------------------------------------------------------

### Manager

The `LeadManager` is used to manage visitors and can be accessed via the the IoC container:
```php
$manager = $kanso->LeadManager;
```

The `create` method creates a new visitor and returns a `Visitor` instance:
```php
$visitor = $manager->create($name, $email);
```

The `byId` method returns a single `Visitor` instance by `id` or `FALSE` if they don't exist:
```php
$visitor = $manager->byId(3);
```

The `byEmail` method returns a single `Visitor` instance by `email` or `FALSE` if they don't exist:
```php
$visitor = $manager->byEmail('foo@bar.com');
```
--------------------------------------------------------

### Provider

The `LeadProvider` be accessed via the `Crm`, the `LeadManager` or the IoC container:
```php
$provider = $kanso->Crm->leadProvider();

$provider = $kanso->LeadManager->provider();

$provider = $kanso->LeadProvider;
```

The `create` method creates a new visitor and returns a `Visitor` instance:
```php
$visitor = $provider->create([
	'ip_address'  => $kanso->Request->environment()->REMOTE_ADDR,
	'name'        => 'John Doe',
	'email'       => 'john@doe.com',
	'last_active' => time(),
]);
```

The `byId` method returns a single `Visitor` instance by `id` or `FALSE` if they don't exist:
```php
$visitor = $provider->byId(3);
```

The `byKey` method returns an array of visitors by key/value from the database `crm_visitors` table. The third argument defines to limit the results to single row:
```php
# Get the first visitor with name is 'John Doe'
$users = $provider->byKey('name', 'John Doe');

# Get all the visitors that under name 'John Doe'
$users = $provider->byKey('name', 'John Doe', false);
```

The `all` method returns an array of all visitors:
```php
$visitors = $provider->all();
```

The `leads` method returns an array of all visitors that have an `email`:
```php
$leads = $provider->leads();
```

--------------------------------------------------------

### Visitor

Once you have reference to a visitor, you can set values user PHP's magic methods. The `save` method will save the visitor:
```php
$visitor->name  = 'John Doe';
$visitor->email = 'John@doe.com';
$visitor->save();
```

The `delete` method will delete the visitor and all of their visits:
```php
$visitor->delete();
```

The `regenerateId` method will regenerate a visitors `UUID`:
```php
$visitor->regenerateId();

$id = $visitor->visitor_id;
```

The `addVisit` method adds a new visit entry for the visitor:

> The `medium`, `channel`, `campaign`, `keyword` and `creative` are used to track inbound visits based on their source.

```php
#URL = https://example.com?md=facebook&ch=cpc&cp=lead-gen&kw=products&cr=banner
$queries = $kanso->Request->queries();

$visitor->addVisit([
	'visitor_id'   => $visitor->visitor_id,
	'ip_address'   => $kanso->Request->environment()->REMOTE_ADDR,
	'page'         => $kanso->Request->environment()->REQUEST_URL,
	'date'         => time(),
	'medium'       => isset($queries['md']) ? $queries['md'] : null,
	'channel'      => isset($queries['ch']) ? $queries['ch'] : 'direct',
	'campaign'     => isset($queries['cp']) ? $queries['cp'] : null,
	'keyword'      => isset($queries['kw']) ? $queries['kw'] : null,
	'creative'     => isset($queries['cr']) ? $queries['cr'] : null,
]);
```

The `isLead` method will return `TRUE` if the visitor has an `email` or `FALSE` if not:
```php
if ($visitor->isLead())
{
}
```

The `isFirstVisit` method will return `TRUE` if the visitor has only a single visit:
```php
if ($visitor->isFirstVisit())
{
}
```

The `visits` method will an array of `Visits` in chronological order:
```php
$visits = $visitor->visits();
```

The `countVisits` method returns the number of visits a visitor has:
```php
if ($visitor->countVisits() > 5)
{
}
```

The `previousVisit` method will return the visitor's previous `Visit` instance or `FALSE` if it does not exist:
```php
$prevVisit = $visitor->previousVisit();
```

The `timeSincePrevVisit` method returns the number of seconds since a visitors previous visit: 
```php
// More than 1 day
if ($visitor->timeSincePrevVisit() > 86400)
{
}
```

The `firstVisit` method will return the visitor's first `Visit` instance:
```php
$firstVisit = $visitor->firstVisit();
```

The `heartBeat` method updates a visitor's `last_active` time and saves the visitor:
```php
$visitor->heartBeat();
```

The `makeLead` method updates a visitor to a lead:
```php
$visitor->makeLead('email@domain.com');

$visitor->makeLead('email@domain.com', 'John Doe');
```

The `bounced` method checks if a visitor bounced:
```php
if ($visitor->bounced())
{
}
```

The `channel` method returns a visitor's initial landing channel:
```php
#URL = https://example.com?ch=cpc

if ($visitor->channel() === 'cpc')
{
}
```

The `medium` method returns a visitor's initial landing medium:
```php
#URL = https://example.com?md=facebook

if ($visitor->medium() === 'facebook')
{
}
```

The `grade` method grades a visitor based on their site usage and returns their score:
```php
$grade = $visitor->grade();

$grade = $visitor->grade($visitor);
```

To get their `grade` as a string, set the second paramter to `TRUE`: 
```php
$grade = $visitor->grade(null, true);
```

| Grade | String    | Description                        |
|-------|-----------|------------------------------------|
| 1     | `visitor` | Visitor is not a lead.             |
| 2     | `lead`    | Visitor is a lead.                 |
| 2     | `sql`     | Visitor is a sales qualified lead. |


