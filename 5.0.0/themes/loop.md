[Documentation Menu](#)

# The Loop

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

You can use PHP's native loop iterators to display posts with Kanso. When using The Loop you can use Kanso's [Template Methods](/themes/template-methods) to retrieve the information you need to display data. It's most common to use PHP's `while` loop on Kanso's [Query](/themes/query/) object when inside a template to display post data or content.

--------------------------------------------------------

### Usage

You can use The Loop in almost any template file. The most common of which is the `index.php` file for displaying the homepage.

You can start The Loop like this:

```php
if ( have_posts() ) : while ( have_posts() ) : ?>
```

And end it like this:

```php
<?php the_post(); endwhile; else : ?>
<p>Sorry, no posts matched your criteria.</p>
<?php endif; ?>
```

This can be expressed as follows using PHP's regular syntax for control structures:

```php
<?php 
if ( have_posts() )
{
    while ( have_posts() )
    {
        # ... content 
       
        the_post(); 
    } 
}
else
{
    echo '<p>Sorry, no posts matched your criteria.</p>';
}
?>
```