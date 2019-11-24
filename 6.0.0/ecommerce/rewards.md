# Rewards

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Kanso comes with a handy rewards system to manage discounts for repeat customers. If a user opts to create an account during checkout, or is logged in and checks out, they will earn points on their purchase.

Those points can be used to redeem exclusive coupon codes that can only be used by that user.

The ratio of `dollars spent -> points earned` and `points used -> discount percentage` are stored in the `app/configurations/ecommerce.php` file.

--------------------------------------------------------

### Access

You can access the `Rewards` component through the `Ecommerce` service via the IoC container:
```php
$rewards = $kanso->Ecommerce->rewards();
```

--------------------------------------------------------

### Usage

The `coupons` method returns all of a user's redeemed coupons either by user `id` or defaults to the currently logged in user:
```php
// User id 1
$coupons = $rewards->coupons(1);

// Currently logged in user
$coupons = $rewards->coupons();
```

The `points` method returns the balance of a user's account by user `id` or defaults to the currently logged in user:
```php
// User id 1
$balance = $rewards->points(1);

// Currently logged in user
$balance = $rewards->points();
```

The `lifetimePoints` method returns all points (both used and unused) on a user's account by user `id` or defaults to the currently logged in user:
```php
// User id 1
$total = $rewards->lifetimePoints(1);

// Currently logged in user
$total = $rewards->lifetimePoints();
```

The `history` method returns an array of all points earned and points spent on a user's account by user `id` or defaults to the currently logged in user:
```php
// User id 1
$history = $rewards->history(1);

// Currently logged in user
$balance = $rewards->history();

/*[
	[
		'id'           => 1,
		'user_id'      => 3,
		'description'  => 'Online Products Purchase ($26.95)',
		'points_add'   => 100,
		'points_minus' => 0,
		'balance'      => 100,
	],
	[
		'id'           => 1,
		'user_id'      => 3,
		'description'  => 'Redeemed 10% Off Coupon',
		'points_add'   => 0,
		'points_minus' => 100,
		'balance'      => 0,
	],
]*/
```

The `addPoints` method adds points to user's account by user `id` or defaults to the currently logged in user:
```php
// 50 points to user id 1
$rewards->addPoints(50, 'Online Products Purchase ($26.95)', 1);

// 50 points to currently logged in user
$total = $rewards->lifetimePoints(50, 'Online Products Purchase ($26.95)');
```

The `minusPoints` method removes points to user's account by user `id` or defaults to the currently logged in user:
```php
// 50 points to user id 1
$rewards->minusPoints(50, 'Redeemed 10% Off Coupon', 1);

// 50 points to currently logged in user
$total = $rewards->lifetimePoints(50, 'Redeemed 10% Off Coupon');
```

The `createCoupon` method creates a coupon on a user's account and returns the coupon code (by user `id` or defaults to the currently logged in user):
```php
// Creates a 10% off coupon using 100 points on user id 1
$coupon = $rewards->createCoupon('10% Off Coupon', '10% off any and all products', 10, 100, 1);

// Creates a 10% off coupon using 100 points on currently logged in user
$coupon = $rewards->createCoupon('10% Off Coupon', '10% off any and all products', 10, 100);
```

The `calcPoints` method returns the number of points that will be earned by dollars spent:
```php
$earned = $rewards->calcPoints(19.95);
```
