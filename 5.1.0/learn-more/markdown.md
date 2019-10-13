# Markdown

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Kanso's `Markdown` helper can be used to convert `markdown` into `HTML`.

> The Markdown service is part of the Framework group of services.

--------------------------------------------------------

### Access

You can access the `Markdown` class via the IoC container:
```php
$markdown = $kanso->Markdown;
```

--------------------------------------------------------

### Usage

Convert markdown into html with the `convert` method:
```php
$html = $markdown->convert('### Heading 3');
```

The `plainText` method converts markdown into plain text:
```php
$text = $markdown->plainText('### Heading 3');
```