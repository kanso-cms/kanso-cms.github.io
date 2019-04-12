# Products

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

With Kanso, Products are essentially a custom post type. They do however have a number of extra options. To create a new product, simply go to your Admin panel and create a new post and set the post `type` to product. You must also set all the extra options such as `price` etc...

--------------------------------------------------------

### Access

You can access the `Products` component through the `Ecommerce` service via the IoC container:
```php
$products = $kanso->Ecommerce->products();
```

Additionally, you can get access to any product via the `PostProvider`:
```php
$product = $kanso->PostManager->provider()->byId(1);
```

Please see the [Posts](/posts/posts) section of the documentation for further details.

--------------------------------------------------------

### Usage

All kanso `Products` must have at least one `offer`. Offers are serve as a way to have multiple variations of a single product. 

The `offer` method returns a single product offer by `offer_id` (SKU). If the product does not exist, or does not have an offer under that `id`, the method will return `FALSE`:
```php
// Returns offer 'SKU3535' on product id '1'
$offer = $products->offer(1, 'SKU3535');
```

An offer is simply an array of the offer data.
```php
$offer = $products->offer(1, 'SKU3535');
/*[
	'offer_id'   => 'SKU3535',
	'name'       => 'XXL',
	'price'      => 19.95,
	'sale_price' => 9.95,
	'instock'    => false,
];*/
```

The `offers` method returns all of a products offers by as an array. If the product does not exist, or does not have any offers, an empty array will be returned:
```php
// Returns all offers on product id '1'
$offers = $products->offers(1,);
```

If you have reference to a single product, offers are stored as part of the `post_meta` under the `offers` key:
```php
$product = $kanso->PostManager->provider()->byId(1);

$offers  = $product->meta['offers'];
```

If you are inside a [view](/mvc/view) scope, or any theme [template file](/themes/files), you can use the `the_post_meta` method:
```php
$offers  = the_post_meta()['offers'];
```