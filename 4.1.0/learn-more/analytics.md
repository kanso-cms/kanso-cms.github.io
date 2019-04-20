# Analytics

- [Configuration](#configuration)
- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Kanso's `Analytics` service is used to quickly and easily insert your tracking codes for external analytics services throughout the Kanso application.

--------------------------------------------------------

### Configuration

The analytics options can be configured in the `app/configurations/analytics.php` file.

--------------------------------------------------------

### Access

An instance can be accessed via the `Analytics` key through the IoC container.

```php
$analytics = $kanso->Analytics;
```

--------------------------------------------------------

### Usage

The `googleTrackingCode` method returns your tag as string for Google Analytics and Adwords:
```php
echo $analytics->googleTrackingCode();
```

The `facebookTrackingCode` method returns your tag as string for Facebook's Pixel:
```php
echo $analytics->facebookTrackingCode();
```

The `googleTrackingProductView` method returns a Google Analytics tag as string for a view event on the current product:
```php
echo $analytics->googleTrackingProductView();
```

The `facebookTrackingProductView` method returns a Facebook Pixel tag as string for a view event on the current product:
```php
echo $analytics->facebookTrackingProductView();
```

The `googleTrackingStartCheckout` method returns a Google Analytics tag as string for an initiate checkout event:
```php
echo $analytics->googleTrackingStartCheckout();
```

The `facebookTrackingStartCheckout` method returns a Facebook Pixel tag as string for an initiate checkout event:
```php
echo $analytics->facebookTrackingStartCheckout();
```

The `googleTrackCheckoutComplete` method returns a Google Analytics tag as string for a complete checkout event:
```php
echo $analytics->googleTrackCheckoutComplete();
```

The `facebookTrackCheckoutComplete` method returns a Facebook Pixel tag as string for a complete checkout event:
```php
echo $analytics->facebookTrackCheckoutComplete();
```