# Coupons

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Kanso comes with a handy coupon management system to manage discounts and special promotions. Coupons are managed in the `app/configurations/ecommerce.php` file and also in the database as part of the [Rewards](/ecommerce/rewards) system.

There are two types of coupons in Kanso:

1. **Public coupons**<br>
Public coupons are managed via the admin panel and can be used 1 time each by any site visitor during checkout.

2. **Rewards coupons**<br>
Rewards coupons, are special single-use redeemable coupons for logged in users. Users earn points on each purchase (when checking out while logged in or creating an account) and can redeem those points for exclusive coupons. Please see the [Rewards](/ecommerce/rewards) section of the documentation for further details.

Both are managed as a single entity via the `Coupons` component.

--------------------------------------------------------

### Access

You can access the `Coupons` component through the `Ecommerce` service via the IoC container:
```php
$coupons = $kanso->Ecommerce->coupons();
```

--------------------------------------------------------

### Usage

The `exists` method returns `TRUE` if a coupon exists or `FALSE` if not:
```php
if ($coupons->exists('SPECIAL_10'))
{
    // Do something here
}
```

The `used` method checks if the current user has used a coupon:
```php
if ($coupons->used('SPECIAL_10'))
{
    // Do something here
}
```

If the user is checking out as guest, you can check if they have used the coupon by supplying their email address:
```php
if ($coupons->used('SPECIAL_10', 'test@example.com'))
{
    // Do something here
}
```

The `discount` method returns the discount percentage of a coupon:
```php
$discount = $coupons->discount('SPECIAL_10');
```

The `setUsed` method marks a coupon as used on the current site visitor:
```php
$coupons->setUsed('SPECIAL_10');
```

The `setUsed` method marks a coupon as used on the current site visitor:
```php
$coupons->setUsed('SPECIAL_10');
```
