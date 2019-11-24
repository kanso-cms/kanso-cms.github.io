# Template Methods

--------------------------------------------------------

-   [Validation](#validation)
-   [HTML meta](#html-meta)
-   [URLS And Directories](#urls-and-directories)
-   [Page type](#page-type)
-   [Templates](#templates)
-   [Post Iteration](#post-iteration)
-   [Post data](#post-data)
-   [Tags](#tags)
-   [Category](#category)
-   [Author](#author)
-   [Comments](#comments)
-   [Search](#search)
-   [Attachments](#attachments)

--------------------------------------------------------

Template Methods are used within a theme's Template files to display information dynamically or otherwise customize a Kanso Theme. The Template Methods are made available in all theme files and are derived from Kanso's `Query` Class. All methods can be found in the `kanso/cms/query/methods` directory.

Basically, Template Methods are just pre-made helper functions to aid faster theme development.

A reference to the Kanso object instance is also available in all template files by simply accessing the `$kanso` variable.

The following list describes all the available Template Methods made available within the template file scope. They have been grouped into their respective context. For further documentation, please read through the source code in the `kanso/cms/query/methods` directory or the [API](/api/6.0.0/index.html).

--------------------------------------------------------

#### Validation

-   user
-   is\_loggedIn
-   user\_is\_admin

--------------------------------------------------------

#### HTML meta

-   website\_title
-   website\_description
-   domain\_name
-   the\_meta\_description
-   the\_meta\_title
-   the\_canonical\_url
-   the\_previous\_page\_title
-   the\_next\_page\_title
-   the\_next\_page\_url
-   the\_previous\_page\_url
-   the\_previous\_page
-   the\_next\_page

--------------------------------------------------------

#### URLS And Directories

-   themes\_directory
-   theme\_name
-   theme\_directory
-   theme\_url
-   home\_url
-   blog\_url
-   blog\_location
-   the\_attachments\_url
-   base\_url

--------------------------------------------------------

#### Page type

-   the\_page\_type
-   is\_single
-   is\_custom\_post
-   is\_home
-   is\_blog\_location
-   is\_page
-   is\_search
-   is\_tag
-   is\_category
-   is\_author
-   is\_admin
-   is\_attachment
-   is\_not\_found

--------------------------------------------------------

#### Templates

-   the\_header
-   the\_footer
-   the\_sidebar
-   include\_template

--------------------------------------------------------

#### Post Iteration

-   the\_post
-   the\_posts
-   the\_posts\_count
-   posts\_per\_page
-   have\_posts
-   rewind\_posts
-   \_next
-   \_previous

--------------------------------------------------------

#### Post data

-   the\_post\_id
-   the\_excerpt
-   the\_post\_status
-   the\_post\_type
-   the\_post\_meta
-   the\_time
-   the\_modified\_time
-   has\_post\_thumbnail
-   the\_title
-   the\_permalink
-   the\_slug
-   the\_content
-   the\_post\_thumbnail
-   the\_post\_thumbnail\_src
-   display\_thumbnail
-   all\_static\_pages
-   all\_custom\_posts
-   pagination\_links

--------------------------------------------------------

#### Tags

-   tag\_exists
-   the\_tags
-   the\_tags\_list
-   the\_tag\_slug
-   the\_tag\_url
-   the\_taxonomy
-   all\_the\_tags
-   has\_tags
-   the\_tag\_posts

--------------------------------------------------------

#### Category

-   category\_exists
-   the\_category
-   the\_category\_name
-   the\_categories
-   the\_categories\_list
-   the\_category\_slug
-   the\_category\_url
-   all\_the\_categories
-   has\_categories
-   the\_category\_posts

--------------------------------------------------------

#### Author

-   the\_author
-   author\_exists
-   has\_author\_thumbnail
-   the\_author\_name
-   the\_author\_url
-   the\_author\_thumbnail
-   the\_author\_bio
-   the\_author\_twitter
-   the\_author\_google
-   the\_author\_facebook
-   the\_author\_instagram
-   all\_the\_authors
-   the\_author\_posts

--------------------------------------------------------

#### Comments

-   comments\_open
-   has\_comments
-   comments\_number
-   comment
-   comments
-   display\_comments
-   comment\_form
-   get\_gravatar

--------------------------------------------------------

#### Search

-   search\_query
-   search\_form

--------------------------------------------------------

#### Attachments

-   the\_attachment
-   all\_the\_attachments
-   the\_attachment\_url
-   the\_attachment\_size