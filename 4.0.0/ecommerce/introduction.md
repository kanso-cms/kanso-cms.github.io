# Ecommerce

--------------------------------------------------------

- [Configuration](#configuration)
- [Components](#components)

--------------------------------------------------------

Kanso comes pre built with a handy e-commerce component to help facilitate online purchases. Kanso uses [Braintree](https://developers.braintreepayments.com/) as a payment gateway and currently supports credit-card only transactions.

While the `Ecommerce` service is designed to make working with Braintree as easy as possible, you will need to carefully read this documentation and follow strict security measures to ensure a safe application.

> The Ecommerce service is part of the CMS group of services.

--------------------------------------------------------

### Configuration

If you intend to use the `Ecommerce` component in your application, you will first need to signup to Braintree. This can be a complicated process. To get started, you can create a `sandbox` account for testing however.

Configuration options for the `Ecommerce` component are managed in the `app/configurations/ecommerce.php` file. Please see the comments there for more details.

The `Ecommerce` service, uses Braintree's PHP library and will not work without it. To install it, simply navigate to your kanso installation and install it via composer.

```bash
cd path/to/my/project/kanso
composer install
```

If you are not using the `Ecommerce` service, you can disable it by removing it from your application in the `app/configurations/application.php` file.
