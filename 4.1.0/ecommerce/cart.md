# Cart

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Kanso's cart component serves as handy utility to quickly manage a user's shopping cart. Shopping carts are managed via both the user's `session` and the internal `database` system.

--------------------------------------------------------

### Access

You can access the `Cart` component through the `Ecommerce` service via the IoC container:
```php
$cart = $kanso->Ecommerce->cart();
```

--------------------------------------------------------

### Usage

The `items` method returns an array of the current user's cart:
```php
$items = $cart->items();
/*[
	'product'  => 1,
	'quantity' => 1,
	'offer'    =>
	[
		'offer_id'   => 'SKU3535',
		'name'       => 'XXL',
		'price'      => 19.95,
		'sale_price' => 9.95,
		'instock'    => false,
	],
]*/
```

The `isEmpty` method returns `TRUE` if the current user's cart is empty, or `FALSE` if not:
```php
if ($cart->isEmpty())
{
	// Do something here
}
```

The `count` counts the total number of items in the current user's cart. This includes multiple instances of the same item:
```php
$count = $cart->count();
```

The `clear` method empties the current user's shopping cart:
```php
$cart->clear();
```

The `add` method adds an item to the current user's cart by `product_id` and `offer_id`:
```php
$cart->add(1, 'SKU3535');
```

The `remove` method removes an item from the current user's cart by `product_id` and `offer_id`:
```php
$cart->remove(1, 'SKU3535');
```

The `minus` method decrements the quantity of an item in the current user's cart by 1 (by `product_id` and `offer_id`):
```php
$cart->minus(1, 'SKU3535');
```

The `subTotal` method returns the current user's cart subtotal (total price without shipping):
```php
$subtotal = $cart->subTotal();
```

The `shippingCost` method returns the current user's shipping cost:
```php
$shipping = $cart->shippingCost();
```

The `gst` method returns the current user's inclusive GST costs:
```php
$gst = $cart->gst();
```

The `addresses` method returns the returns an array of the current user's stored postal addresses:
```php
$addresses = $cart->addresses();
```

The `cards` method returns the returns an array of the current user's stored credit cards:

> Please note this these entries only store a payment token which is used by Braintree to process payments on an existing credit card. No credit card details stored in the kanso application.

```php
$cards = $cart->cards();
```




