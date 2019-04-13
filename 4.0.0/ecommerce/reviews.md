# Reviews

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Kanso products come with a handy review system to help manage customer reviews. Reviews are stored internally as comments with additional entries for ratings and other functionality.

--------------------------------------------------------

### Access

You can access the `Reviews` component through the `Ecommerce` service via the IoC container:
```php
$reviews = $kanso->Ecommerce->reviews();
```

--------------------------------------------------------

### Usage

The `all` method returns an array of all reviews by product `id`, sorted by relevancy (up-votes):
```php
$all = $reviews->all(1);
```

The `rating` method returns the rating (1-5) of a single review by review `id`:
```php
$rating = $reviews->rating(3);
```

The `recommends` method returns `TRUE` if a review marked a product as `recommended` or `FALSE` if not (by review `id`):
```php
$recommends = $reviews->recommends(3);
```

The `upVotes` method returns returns the total number of up-votes on a review by `id`:
```php
$upVotes = $reviews->upVotes(3);
```

The `downVotes` method returns returns the total number of down-votes on a review by `id`:
```php
$downVotes = $reviews->downVotes(3);
```

The `upVote` method up-votes a review by `id`:
```php
$reviews->upVote(3);
```

The `downVote` method down-votes a review by `id`:
```php
$reviews->downVote(3);
```

The `ratings` method returns a summary of all reviews on a product by `id`:
```php
$reviews->ratings(1);
/*[
    'average' => 4.5,
    'count'   => 26,
    'best'    => 5,
    'stars'   => [
        [
            'number' => 5,
            'count'  => 10,
        ],
        [
            'number' => 4,
            'count'  => 7,
        ],
        [
            'number' => 3,
            'count'  => 6,
        ],
        [
            'number' => 2,
            'count'  => 1,
        ],
        [
            'number' => 1,
            'count'  => 0,
        ],
    ],
];*/
```