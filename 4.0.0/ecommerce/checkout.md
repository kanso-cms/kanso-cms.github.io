# Checkout

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Options](#options)
- [Return Values](#Return-Values)
- [Errors](#errors)

--------------------------------------------------------

Kanso's `Checkout` component serves as the core utility to process transactions. The `Checkout` component manages all the complexities of processing transactions, using coupons, creating accounts and earning rewards.

--------------------------------------------------------

### Access

You can access the `Checkout` component through the `Ecommerce` service via the IoC container:
```php
$checkout = $kanso->Ecommerce->checkout();
```

--------------------------------------------------------

### Usage

The `payment` method is used to process the payment for the current site visitor and their cart:
```php
$processed = $checkout->payment($options);
```

--------------------------------------------------------

### Options

The options parameter is an array to configure your payment options:

##### Account
| Key                         | Type      | Behaviour                                                                                                                                     | Notes                                                                                                                                                                                                                               |
|--:--------------------------|-:---------|-:---------------------------------------------------------------------------------------------------------------------------------------------|-:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `create_account`            | `boolean` | Create an account before processing transaction. Log the user into the account on success and apply any rewards points earned on transaction. | If set to `TRUE` - User must not be already logged in, an existing account must not already exist under `shipping_email`, `password` must be provided. If set to `FALSE`, user will checkout as `guest` and won't earn any rewards. |
| `password`                  | `string`  | Unhashed password for new account                                                                                                             | Must be provided when `create_account` is set to `TRUE`                                                                                                                                                                             |

##### Coupon
| Key                         | Type      | Behaviour                                                                                                                                     | Notes                                                                                                                                                                                                                               |
|--:--------------------------|-:---------|-:---------------------------------------------------------------------------------------------------------------------------------------------|-:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `coupon`                    | `string`  | Coupon code to use on transaction                                                                                                             | Optional                                                                                                                                                                                                                            |
| `apply_coupon`              | `boolean` | Apply the supplied coupon to the transaction                                                                                                  | Coupon must exist, and not already be used.                                                                                                                                                                                         |

##### Shipping
| Key                         | Type      | Behaviour                                                                                                                                     | Notes                                                                                                                                                                                                                               |
|--:--------------------------|-:---------|-:---------------------------------------------------------------------------------------------------------------------------------------------|-:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `shipping_use_new_address`  | `boolean` | Use a new shipping address for transaction.                                                                                                   | If set to `TRUE`, all shipping details must be provided. If set to `FALSE`, `shipping_existing_address` must be set, and the current user must be logged in.                                                                        |
| `shipping_existing_address` | `integer` | The shipping row id from the database to use.                                                                                                 | If set, `shipping_use_new_address` must be set to `FALSE` and the current user must be logged in.                                                                                                                                   |
| `shipping_save_address`     | `boolean` | Save the provided address details.                                                                                                            | If set to `TRUE`, all shipping details must be provided, the user must be logged in or `create_account` must be set to `TRUE`.                                                                                                      |
| `shipping_first_name`       | `string`  | First name for shipping                                                                                                                       |                                                                                                                                                                                                                                     |
| `shipping_last_name`        | `string`  | Last name for shipping                                                                                                                        |                                                                                                                                                                                                                                     |
| `shipping_email`            | `string`  | Email address of shipping                                                                                                                     | Must be a valid email address.                                                                                                                                                                                                      |
| `shipping_address_1`        | `string`  | Last name for shipping                                                                                                                        |                                                                                                                                                                                                                                     |
| `shipping_address_2`        | `string`  | Last name for shipping                                                                                                                        | Optional                                                                                                                                                                                                                            |
| `shipping_suburb`           | `string`  | Last name for shipping                                                                                                                        |                                                                                                                                                                                                                                     |
| `shipping_zip`              | `string`  | Last name for shipping                                                                                                                        |                                                                                                                                                                                                                                     |
| `shipping_state`            | `string`  | Last name for shipping                                                                                                                        |                                                                                                                                                                                                                                     |
| `shipping_country`          | `string`  | Last name for shipping                                                                                                                        |                                                                                                                                                                                                                                     |
| `shipping_phone`            | `string`  | Last name for shipping                                                                                                                        |                                                                                                                                                                                                                                     |

##### Billing
| Key                         | Type      | Behaviour                                                                                                                                     | Notes                                                                                                                                                                                                                               |
|--:--------------------------|-:---------|-:---------------------------------------------------------------------------------------------------------------------------------------------|-:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `billing_use_new_card`      | `boolean` | Use a new credit card for transaction.                                                                                                        | If set to `TRUE`, all billing details must be provided. If set to `FALSE`, `billing_existing_card` must be set, and the current user must be logged in and own the credit card.                                                     |
| `billing_existing_card`     | `integer` | The credit card row id from the database to use.                                                                                              | If set, `billing_use_new_card` must be set to `FALSE` and the current user must be logged in and own the credit card.                                                                                                               |
| `billing_save_card_info`    | `boolean` | Save the provided credit card details.                                                                                                        | If set to `TRUE`, all shipping details must be provided, the user must be logged in or `create_account` must be set to `TRUE`.                                                                                                      |
| `billing_card_last_four`    | `string`  | Last 4 digits of credit card                                                                                                                  |                                                                                                                                                                                                                                     |
| `billing_card_type`         | `string`  | Credit card issuer (e.g `visa`)                                                                                                               |                                                                                                                                                                                                                                     |
| `billing_card_name`         | `string`  | Name on credit card                                                                                                                           |                                                                                                                                                                                                                                     |
| `billing_card_mm`           | `string`  | Expiry month of credit card                                                                                                                   |                                                                                                                                                                                                                                     |
| `billing_card_yy`           | `string`  | Expiry year of credit card                                                                                                                    |                                                                                                                                                                                                                                     |
| `billing_method_nonce`      | `string`  | Braintree generated credit card nonce                                                                                                         |                                                                                                                                                                                                                                     |

--------------------------------------------------------

### Return Values

No matter the result, the `payment` method always return a status code, which is a constant from the `Checkout` class. Below is a list of possible status codes:

| Constant         | Code   | Reason                                                           |
|--:---------------|--:-----|--:---------------------------------------------------------------|
| `EMPTY_CART`     | `100`  | The user's shopping cart is empty.                               |
| `USER_EXISTS`    | `200`  | A user already exists when attempting to create a new user.      |
| `COUPON_INVALID` | `300`  | The provided coupon does not exist                               |
| `COUPON_USED`    | `400`  | The provided coupon has already been used                        |
| `GATEWAY_ERROR`  | `500`  | A gateway error was encountered by Braintree (e.g invalid nonce) |
| `CARD_DECLINED`  | `700`  | The credit card was declined.                                    |
| `SUCCESS`        | `8000` | The payment was successfully processed.                          |

--------------------------------------------------------

### Errors

Use the status code cosntants to validate a transaction result:
```php
$result = $checkout->payment($options);

if ($result !== $checkout::SUCCESS)
{
	// Process error here
}
```

The `successful` method returns `TRUE` if the payment was successfully processed or `FALSE` if there was an error:
```php
$result = $checkout->payment($options);

if (!$checkout->successful())
{
	// Process error here
}
```

The `errorMessage` method returns the error message:
```php
$result = $checkout->payment($options);

if (!$checkout->successful())
{
	if ($result === $checkout::COUPON_USED)
	{
		echo $checkout->errorMessage();
	}
}
```

> The `Checkout` components automtically logs all failed transactions to a separate error log under `/storage/logs/transaction_errors.log` for troubleshooting.

For more details on Braintree response errors, please see the documentation [here](https://developers.braintreepayments.com/reference/general/validation-errors/all/php).

