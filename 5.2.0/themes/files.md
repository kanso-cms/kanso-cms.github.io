# Theme files

--------------------------------------------------------

- [Mandatory Files](#mandatory-files)
    - [index.php](#indexphp)
    - [single.php](#singlephp)
    - [page.php](#pagephp)
    - [taxonomy.php](#taxonomyphp)
    - [search.php](#searchphp)
- [Optional Files](#optional-files)
    - [functions.php](#functionsphp)
    - [header.php](#headerphp)
    - [footer.php](#footerphp)
    - [sidebar.php](#sidebarphp)
    - [searchform.php](#searchformphp)
    - [commentform.php](#commentformphp)
    - [comments.php](#commentsphp)
    - [attachment.php](#attachmentphp)
    - [404.php](#404php)
    - [sitemap.php](#sitemapphp)
- [Wildcard Files](#wildcard-files)
    - [homepage.php](#homepagephp)
    - [home-`$slug`.php](#home-slugphp)
    - [blog.php](#blogphp)
    - [page-`$slug`.php](#page-slugphp)
    - [single-`$slug`.php](#single-slugphp)
    - [single-`$typ`e.php](#single-typephp)
    - [tag-`$slu`g.php](#tag-slugphp)
    - [taxonomy-`$tag`.php](#taxonomy-tagphp)
    - [category-`$slug`.php](#category-slugphp)
    - [taxonomy-`$category`.php](#taxonomy-categoryphp)
    - [author-`$slug`.php](#author-slugphp)
    - [taxonomy-`$author`.php](#taxonomy-authorphp)

--------------------------------------------------------

> This sections covers Kanso's fundamental pages. Be sure you have read and understand the template [load behavior](/getting-started/themes#load-behavior) before proceeding.

Kanso Themes are where you'll spend the most of your time if you are using the CMS. Kanso's theming system follows a very similar structure as other CMS's like WordPress. One major difference however is because Kanso is OO (Object Oriented), the global namespace isn't polluted with variables or class objects. This section outlines the core template files for Kanso Themes as well as covering some of the optional ones as well.

> When working within any Theme templating file, a reference to the Kanso instance is always available via the `$kanso` variable.

--------------------------------------------------------

### Mandatory Files

Mandatory files are required for core CMS functionality. All themes should include these files.

#### index.php

The `index.php` file is loaded whenever a valid request to the homepage or a valid request to the homepage with pagination is made. For example, if you have enough posts both `http://example.com/` and `http://example.com/page/3/` will load the `index.php` file.

In most cases the index.php file is used to display the most recent posts using [The Loop](/themes/loop), however you can do just about whatever you like with it.

```php
<?php if ( have_posts() ) : while ( have_posts() ) : ?>

...

<?php  the_post(); endwhile; endif; ?>
```

--------------------------------------------------------

#### single.php

The `single.php` file is loaded whenever a valid request for a post is made - i.e your permalinks structure and the post exists. For example, if your permalinks are set on `year/month/my-example-post/`, a post titled `My Example Post` would be loaded into the theme's `single.php` on a request made to `http://example.com/2016/02/my-example-post` (assuming the post was published in February 2016).

```php
<article> 
    <?php echo the_content(); ?>
</article>
```

--------------------------------------------------------

#### page.php

The `page.php` file is loaded whenever a valid request for a static page is made. With Kanso, static pages are managed via the Admin Panel the same way that posts are but are saved with the post type `page`.  
For example, if you create a page titled `My Example Page`, the theme's `page.php` will be loaded on a request made to `http://example.com/my-example-page/`.

```php
<article> 
    <?php echo the_content(); ?>
</article>
```

--------------------------------------------------------

#### taxonomy.php

The `taxonomy.php` file is loaded whenever a valid request for a list of articles by tag, category or author is made. You can enable or disabled each in the Admin Panel. A `taxonomy.php` is very similar to the `index.php` with the exception that posts are listed by the respective field being requested (tag, category, author).  
For example, if you created a number of posts with the tag `Taxonomy Example`, the `taxonomy.php` file would be loaded on requests made to `http://example.com/tag/taxonomy-example/` and `http://example.com/tag/taxonomy-example/page/3/` (assuming there are 3 pages of posts under that tag).  
The same structure is used for category and author requests.

```php
<?php if ( have_posts() ) : while ( have_posts() ) : ?>

...

<?php the_post(); endwhile; endif; ?>
```

--------------------------------------------------------

#### search.php

The `search.php` file as the name suggests is used for displaying search results. It receives a list of posts based on the search query.  
For example `http://example.com/search-results/q=search-term/` will load the `search.php` template into Kanso's view, while filtering the posts based on the search term `search-term`.

```php
<h1>Search Results for “<?php echo search_query(); ?>”</h1>

<?php if ( have_posts() ) : while ( have_posts() ) : ?>

...

<?php  the_post(); endwhile; endif; ?>
```

--------------------------------------------------------

### Optional Files

Optional files are not required by the CMS but add extra functionality.

--------------------------------------------------------


#### functions.php

The `functions.php` file of any Kanso theme serves as an opportunity to customize your theme. Whenever a template from the active Theme is loaded, the `functions.php` file (if it exists) and anything inside it is made available to the theme template.

The `functions.php` file is not loaded into Kanso itself. If you add any hooks or filters in this file, they will have no effect on Kanso's behavior as the file is not loaded until Kanso is completely ready to display a page.

The recommended way to use the `functions.php` file is essentially as a helper library to a theme. A typical example might be to have a function for a custom excerpt length.

```php
# Display the post excerpt at a custom length
function customExcerpt($length, $suffix = '', $toChar = true)
{
    $excerpt = the_excerpt();

    if ($toChar) return (strlen($excerpt) > $length ) ? substr($excerpt, 0, $length).$suffix : $excerpt;

    $words = explode(' ', $excerpt);

    if(count($words) > $length) return implode(' ', array_slice($words, 0, $length)).$suffix;

    return $excerpt;
}
```

--------------------------------------------------------

#### header.php

The `header.php` file allows for a consistent way to to display your header content. While the `header.php` file is not mandatory, because it's included in almost all theme development, Kanso has support for including it via the `the_header` function.

Kanso has a number of helper methods to help with creating SEO friendly header content, however it is entirely the developer's responsibility to write their own code. While Kanso is all for helper functions it encourages you to write both HTML and PHP as much as possible.

```php
<!DOCTYPE html>
<html lang="en-US" xmlns="http://www.w3.org/1999/xhtml">
<head>

<meta charset="utf-8">
<title><?php echo the_page_title();?></title>
...
```

--------------------------------------------------------

#### footer.php

The `footer.php` has the the same functionality the `header.php` and can be included in any template via the `the_footer`.

```php
    <footer>
...
</footer>
<script type="text/javascript" src="<?php echo theme_url();?>/assets/js/scripts.js"></script>
</body>
</html>
```

--------------------------------------------------------

#### sidebar.php

The `sidebar.php` serves no real mandatory purpose excerpt that it can be included in any template via the `the_sidebar` function. It's up to you if you want to use it.

--------------------------------------------------------

#### searchform.php

The `searchform.php` file is an optional template for displaying a search-form when `search_form` is called. If the file does not exist, Kanso's default search form is will be returned.

```php
<form role="search" method="get" action="<?php echo home_url(); ?>/search-results/">

    <fieldset>

        <label for="q">Search: </label>

        <input type="search" name="q" id="q" placeholder="Search..." />

        <button type"submit" class="button">Search</button>

    </fieldset>

</form>
```

--------------------------------------------------------

#### commentform.php

The `commentform.php` file is an optional template for displaying a custom comment form when `comment_form` is called. The `comment_form` function accepts a number of arguments to display comments form, however in some cases you may want to follow a completely customized approach. In this case, if `commentform.php` exists, `comment_form` will output the contents of the template. If the file does not exist, Kanso's default comment form will be returned.

```php
<form class="comment-form">

    <label for="name">Name:</label>
    
    <input type="text" name="name" placeholder="Name" autocomplete="off" />

...

</form>
```

--------------------------------------------------------

#### attachment.php

The `attachment.php` file is an optional template for displaying uploaded media attachments via the cms. By default, Kanso has its own attachement template, but you can customize this by including your own.

--------------------------------------------------------

#### comments.php

The `comments.php` file is an optional template for displaying the comments when `display_comments` is called. The `display_comments` function accepts a number of arguments to display a comments, however in some cases you may want to follow a completely customized approach. In this case, if `comments.php` exists, `display_comments` will output the contents of the template. The comments list will be available as the `$comments` variable.

```php
<?php foreach ($commets as $comment) : ?>

...

<?php endforeach; ?>
```

--------------------------------------------------------

#### 404.php

What else is there to say here? The `404.php` template is used when Kanso can't find a route for the request.

```php
<h1>Opps! The page could not be found</h1>
```

--------------------------------------------------------

#### sitemap.php

When a request is made for the `XML` sitemap (by default at `http://example.com/sitemap.xml`) the CMS will load the the default sitemap. However if `sitemap.php` exists, this file will be used instead.

```php
<?php echo '<?xml version="1.0" encoding="UTF-8"?>';?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd" xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc><?php echo $kanso->Request->environment()->HTTP_HOST;?></loc>
        <lastmod><?php echo date("Y-m-d", time()) . 'T'.date("H:i:sP", time());?></lastmod>
        <changefreq>daily</changefreq>
        <priority>1.0</priority>
    </url>
    <?php if (have_posts()) : while (have_posts()) : the_post(); ?>
    <url>
        <loc><?php echo the_permalink();?></loc>
        <lastmod><?php echo the_modified_time('Y-m-d\TH:i:sP');?></lastmod>
        <changefreq>monthly</changefreq>
        <priority>0.6</priority>
    </url>
    <?php endwhile; endif; ?>
    <url>
        <loc><?php echo $kanso->Request->environment()->HTTP_HOST; ?>/search-results/</loc>
        <lastmod><?php echo date("Y-m-d\TH:i:sP", time());?></lastmod>
        <changefreq>always</changefreq>
        <priority>0.3</priority>
    </url>
</urlset>
```

--------------------------------------------------------

#### Wildcard Files

Wildcard files are used when you need render a template for a specific request. When a wildcard template it exists, it will overrider the generic template loading for the files above (on valid requests). For more details on the load behavior, please refer to the [Load Behavior](/getting-started/themes#load-behavior) section of the documentation.

--------------------------------------------------------

#### homepage.php

The `homepage.php` is loaded exclusively for requests to the homepage and paginated results of the homepage (if `blog_location`) is not set. This includes paginated results e.g `http://example.com/page/4/`.

--------------------------------------------------------

#### home-$slug.php

if you are using a `blog_location` in your CMS configuration, there will not be any paginated results on to the homepage - e.g `http://example.com/page/3/` will return a `404`. Instead you posts will be paginated at your `blog_location` slug.  
For example, if you are using `blog` as your `blog_location`, paginated results for posts will be available at `http://example.com/blog/` and `http://example.com/blog/page/3/`. In these cases the `home-blog.php` page will be loaded.

--------------------------------------------------------

#### blog.php

if you are using a `blog_location` in your CMS configuration, there will not be any paginated results on to the homepage - e.g `http://example.com/page/3/` will return a `404`. Instead you posts will be paginated at your `blog_location` slug.  
For example, if you are using `foo-bar` as your `blog_location`, paginated results for posts will be available at `http://example.com/foo-bar/` and `http://example.com/foo-bar/page/3/`. In these cases the `blog.php` page will be loaded (if the `home-$slug` page does not exist).

--------------------------------------------------------

#### page-$slug.php

For specific rendering of static pages, you can create an optional template for the page to load by using the slug. This will override the `page.php` loading.  
For example, if you create a static post-page called `My Static Page`, the `page-my-static-page.php` template will be loaded instead of `page.php` (if it exists).

--------------------------------------------------------

#### single-$slug.php

For specific rendering of posts, you can create an optional template for the post to load by using the slug. This will override the `single.php` loading.  
For example, if you create a post called `My Example Post`, the `single-my-example-post.php` template will be loaded instead of `single.php` (if it exists).

--------------------------------------------------------

#### single-$type.php

If you have registered a custom post-type, you can load a template tethered to the post type.  
For example, if you create a post type called `products`, the `single-products.php` template will be loaded instead of `single.php` (if it exists).

--------------------------------------------------------

#### tag-$slug.php

You can use a custom template to render specific tags by `slug`.  
For example, if you create a tag called `Foo Bar`, the `tag-foo-bar.php` template will be loaded instead of `taxonomy.php` or `tag.php` (if it exists).

--------------------------------------------------------

#### taxonomy-$tag.php

The `taxonomy-tag.php` file is used the exact same way as `tag.php`, however it has been created so that taxonomy templates are grouped alphabetically in the theme folder structure.

--------------------------------------------------------

#### category-$slug.php

You can use a custom template to render specific categories by `slug`.  
For example, if you create a category called `Foo Bar`, the `category-foo-bar.php` template will be loaded instead of `taxonomy.php` or `category.php` (if it exists).

--------------------------------------------------------

#### taxonomy-$category.php

The `taxonomy-category.php` file is used the exact same way as `category.php`, however it has been created so that taxonomy templates are grouped alphabetically in the theme folder structure.

--------------------------------------------------------

#### author-$slug.php

You can use a custom template to render specific authors by `slug`.  
For example, if you create a author called `Foo Bar`, the `author-foo-bar.php` template will be loaded instead of `taxonomy.php` or `author.php` (if it exists).

--------------------------------------------------------

#### taxonomy-$author.php

The `taxonomy-author.php` file is used the exact same way as `author.php`, however it has been created so that taxonomy templates are grouped alphabetically in the theme folder structure.