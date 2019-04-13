# The Query

--------------------------------------------------------

-   [Access](#access)
-   [New Queries](#new-queries)
-   [Filters](#filters)

--------------------------------------------------------

Kanso's `Query` class is the engine for building and interacting with the CMS. When working within a template file the Query object servers as the main source for outputting [Template Methods](themes/template-methods).

On a valid request, once the router matches a route it will load and initialize the Query class telling it what data to load. For example if a request matches a post route, the Query will load the data for the appropriate post from the database.

--------------------------------------------------------

### Access

You can access the Query class directly through the IoC container:

```php
$query = $kanso->Query;
```

--------------------------------------------------------

### New Queries

When working in a template file, Kanso's Query object has already been initialized with the current query. This means the posts available will be filtered to the query at hand. For example if the query is for `http://example.com/page/4/` and there are `10` posts per page in Kanso's configuration, the Query class will have posts `40-50`.

In a case like this, you may want to get a new Query object, for example to get list of related posts. The Query constructor accepts a string with a special syntax to filter the posts. The following example illustrates how to create a new Query object to get a list of 5 related posts:

```php
<?php

# Create a new Query object
$query = $kanso->Query->create('post_status = published : post_type = post : tag_name = CSS : limit = 5');

# Start a loop on the query
if ( $query->have_posts() ) : while ( $query->have_posts() ) : $post = $query->the_post(); ?>

...

<?php endwhile; endif; ?>
```

You can also use a `foreach` loop on the `Query` by using the `the_posts()` method

```php
<?php

# Create a new Query object
$posts = $kanso->Query->create('post_status = published : post_type = post : tag_name = CSS : limit = 5')->the_posts();

# Start a loop on the query
foreach ($posts as $post) : ?>

...

<?php endforeach; ?>
```

--------------------------------------------------------

### Filters

When using a query string in the Query's constructor, a colon `:` is used to separate statements while the `||` and `&&` operators can be used to set conditionals.

The available logical operators are `&&`, `||`, `=`, `!=`, `>`, `<`, `>=`, `<=` and `LIKE`.

Here are some more examples:

```php
# Filter two different tag names
$query = new \Kanso\View\Query('post_status = published : tag_name = CSS || tag_name = HTML : limit = 5');
```

Below are the available keys.

```noHighlight
# Posts
post_id
post_created   
post_modified  
post_status    
post_type      
post_slug      
post_title     
post_excerpt   
post_thumbnail 

# Tag
tag_id 
tag_name       
tag_slug       

# Category
category_id    
category_name  
category_slug  

# Author
author_id       
author_username 
author_email    
author_name     
author_slug     
author_facebook 
author_twitter  
author_gplus    
author_thumbnail

# Organizers
orderBy
limit
```