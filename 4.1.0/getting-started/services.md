# Services

--------------------------------------------------------

- [Framework Services](#framework-services)
- [CMS Services](#cms-services)
- [App Services](#app-services)

--------------------------------------------------------

Services are an easy and clean way of registering dependencies in the IoC container.

Kanso includes a number of services for your convenience and you'll find a complete list in the `app/configurations/application.php` file. You can add your own or use the existing ones.

Services are split up in 3 groups. `framework`, `cms` and `app`. Note that if you are only using the framework and not the CMS, you should remove the services found in the `services.cms` key inside the `app/configurations/application.php` file.

All Kanso core decencies are registered as services, however there are additional utility dependencies that use static methods that do not require a service. These are documented in the Learn More section of this documentation.

--------------------------------------------------------

### Framework Services

| Service             | Namespace                                     | Key             | Description              |
|:--------------------|:----------------------------------------------|:----------------|:-------------------------|
|                     | `kanso\framework\ioc\Container`               | `Container`     | IoC Container            |
|                     | `kanso\Config\Config`                         | `Config`        | Configuration manager    |
| ErrorHandlerService | `kanso\framework\exception\ErrorHandler`      | `ErrorHandler`  | Error handler            |
| SecurityService     | `kanso\framework\security\Crypto`             | `Crypto`        | Encryption manager       |
| SecurityService     | `kanso\framework\security\spam\SpamProtector` | `SpamProtector` | SPAM protector           |
| DatabaseService     | `kanso\database\Database`                     | `Database`      | Database manager         |
| CacheService        | `kanso\cache\cache`                           | `Cache`         | Cache manager            |
| HttpService         | `kanso\http\route\Router`                     | `Router`        | HTTP request router      |
| HttpService         | `kanso\http\request\request`                  | `Request`       | HTTP request object      |
| HttpService         | `kanso\http\Response\Response`                | `Response`      | HTTP response object     |
| HttpService         | `kanso\http\cookie\Cookie`                    | `Cookie`        | HTTP cookie manager      |
| HttpService         | `kanso\http\session\Session`                  | `Session`       | HTTP session manager     |
| ViewService         | `kanso\framework\mvc\view\View`               | `View`          | View                     |
| OnionService        | `kanso\framework\onion\Onion`                 | `Onion`         | Onion middleware manager |
| UtilityService      | `kanso\framework\utility\GUMP`                | `Validation`    | Validation service       |

--------------------------------------------------------

### CMS Services

| Service           | Namespace                                     | Key               | Description          |
|:------------------|:----------------------------------------------|:------------------|:---------------------|
| BootService       | `kanso\cms\application\Application`           | `CMS`             | CMS application core |
| InstallerService  | `kanso\cms\install\Installer`                 | `Installer`       | CMS installer        |
| EmailService      | `kanso\cms\email\Email`                       | `Email`           | Email utility        |
| EventService      | `kanso\cms\event\Events`                      | `Events`          | Events manager       |
| EventService      | `kanso\cms\event\Filters`                     | `Filters`         | Filters manager      |
| GatekeeperService | `kanso\cms\auth\Gatekeeper`                   | `Gatekeeper`      | CMS Gatekeeper       |
| QueryService      | `kanso\cms\query\Query`                       | `Query`           | CMS Query            |
| AdminService      | `kanso\cms\admin\Admin`                       | `Admin`           | Admin panel access   |
| WrapperService    | `kanso\cms\wrappers\managers\UserManager`     | `UserManager`     | User manager         |
| WrapperService    | `kanso\cms\wrappers\managers\CategoryManager` | `CategoryManager` | Category manager     |
| WrapperService    | `kanso\cms\wrappers\managers\TagManager`      | `TagManager`      | Tag manager          |
| WrapperService    | `kanso\cms\wrappers\managers\PostManager`     | `PostManager`     | Post manager         |
| WrapperService    | `kanso\cms\wrappers\managers\CommentManager`  | `CommentManager`  | Comment manager      |
| WrapperService    | `kanso\cms\wrappers\managers\MediaManager`    | `MediaManager`    | Media manager        |

--------------------------------------------------------

### App Services
App services are for registering custom dependencies inside your own application. Please see [Registering Services](/getting-started/dependency-injection#registering-services) for more details on how to register your own dependencies.

